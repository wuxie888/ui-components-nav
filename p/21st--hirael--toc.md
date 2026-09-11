<!-- Table of Contents · @hirael · https://21st.dev/@hirael/components/toc
     license: MIT · category: navigation-menu
     On-this-page navigation that tracks the active heading as you scroll and highlights it with a moving border marker. -->

You are given a task to integrate an existing React component in the codebase

The codebase should support:
- shadcn project structure
- Tailwind CSS
- Typescript

If it doesn't, provide instructions on how to setup project via shadcn CLI, install Tailwind or Typescript.

Determine the default path for components and styles.
If default path for components is not /components/ui, provide instructions on why it's important to create this folder
Copy-paste this component to /components/ui folder:
```tsx
components/ui/toc.tsx
// Table of Contents from Hirael <https://hirael.com/components/navigation/toc>
// MIT · Mohammad Shehadeh · https://github.com/MohammadShehadeh/hirael

'use client';

import * as React from 'react';

import { cn } from '@/lib/utils';

export interface TocItem {
  id: string;
  text: string;
  level: number;
  children?: TocItem[];
}

type ThrottledFn = (() => void) & { cancel: () => void };

const throttle = (fn: () => void, limit: number): ThrottledFn => {
  let lastRan = 0;
  let timer: ReturnType<typeof setTimeout> | null = null;

  const throttled = (() => {
    const now = Date.now();
    const remaining = limit - (now - lastRan);

    if (remaining <= 0) {
      if (timer) {
        clearTimeout(timer);
        timer = null;
      }
      lastRan = now;
      fn();
    } else if (!timer) {
      timer = setTimeout(() => {
        lastRan = Date.now();
        timer = null;
        fn();
      }, remaining);
    }
  }) as ThrottledFn;

  throttled.cancel = () => {
    if (timer) {
      clearTimeout(timer);
      timer = null;
    }
  };

  return throttled;
};

const flattenTocItems = (items: TocItem[]): TocItem[] => {
  const out: TocItem[] = [];
  const walk = (list: TocItem[]) => {
    for (const item of list) {
      out.push(item);
      if (item.children) walk(item.children);
    }
  };
  walk(items);
  return out;
};

const prefersReducedMotion = () => {
  if (typeof window === 'undefined' || !window.matchMedia) return false;
  return window.matchMedia('(prefers-reduced-motion: reduce)').matches;
};

const TOP_OFFSET = 56;

const useActiveHeading = (items: TocItem[], enabled: boolean) => {
  const [activeId, setActiveId] = React.useState<string | null>(null);

  React.useEffect(() => {
    if (!enabled) return;
    const headings = flattenTocItems(items);

    const update = () => {
      if (window.scrollY === 0) {
        setActiveId(null);
        return;
      }

      const boxes = headings
        .map(({ id }) => {
          const el = document.getElementById(id);
          if (!el) return null;
          return { id, box: el.getBoundingClientRect() };
        })
        .filter((entry): entry is { id: string; box: DOMRect } => entry !== null);

      let current = boxes.find(({ box }) => box.bottom > TOP_OFFSET && box.top < window.innerHeight);

      if (!current) {
        current = [...boxes].reverse().find(({ box }) => box.bottom < TOP_OFFSET);
      }

      setActiveId(current ? current.id : null);
    };

    const onScroll = throttle(update, 200);

    update();
    window.addEventListener('scroll', onScroll, { passive: true });
    window.addEventListener('resize', onScroll, { passive: true });

    return () => {
      onScroll.cancel();
      window.removeEventListener('scroll', onScroll);
      window.removeEventListener('resize', onScroll);
    };
  }, [items, enabled]);

  return activeId;
};

interface TocContextValue {
  activeId: string | null;
}

const TocContext = React.createContext<TocContextValue | null>(null);

const useTocContext = (component: string) => {
  const ctx = React.useContext(TocContext);
  if (!ctx) {
    throw new Error(`${component} must be used within <TableOfContents>.`);
  }
  return ctx;
};

interface TableOfContentsProps extends Omit<React.ComponentProps<'nav'>, 'children'> {
  items?: TocItem[];
  activeId?: string | null;
  label?: React.ReactNode;
  children?: React.ReactNode;
}

const TableOfContents = ({
  items,
  activeId: controlledActiveId,
  label = 'On this page',
  className,
  children,
  'aria-label': ariaLabel,
  ...props
}: TableOfContentsProps) => {
  const trackedActiveId = useActiveHeading(items ?? [], controlledActiveId === undefined);
  const activeId = controlledActiveId !== undefined ? controlledActiveId : trackedActiveId;
  const contextValue = React.useMemo(() => ({ activeId }), [activeId]);

  const content =
    children ??
    (items && items.length > 0 ? (
      <>
        {label ? <TableOfContentsLabel>{label}</TableOfContentsLabel> : null}
        <TableOfContentsList items={items} />
      </>
    ) : null);

  if (!content) return null;

  return (
    <TocContext.Provider value={contextValue}>
      <nav
        data-slot="toc"
        aria-label={ariaLabel ?? 'On this page'}
        className={cn('flex flex-col gap-3', className)}
        {...props}
      >
        {content}
      </nav>
    </TocContext.Provider>
  );
};

type TableOfContentsLabelProps = React.ComponentProps<'p'>;

const TableOfContentsLabel = ({ className, ...props }: TableOfContentsLabelProps) => {
  return (
    <p
      data-slot="toc-label"
      className={cn('font-mono text-[10px] uppercase tracking-[0.14em] text-muted-foreground', className)}
      {...props}
    />
  );
};

interface TableOfContentsListProps extends Omit<React.ComponentProps<'ul'>, 'children'> {
  items?: TocItem[];
  level?: number;
  children?: React.ReactNode;
}

const TableOfContentsList = ({ items, level = 0, className, children, ...props }: TableOfContentsListProps) => {
  if (children === undefined && (!items || items.length === 0)) return null;

  return (
    <ul
      data-slot="toc-list"
      data-level={level}
      className={cn('flex flex-col gap-1', level === 0 ? 'border-s border-border' : 'ms-4 mt-1', className)}
      {...props}
    >
      {children ?? items?.map((item) => <TableOfContentsItem key={item.id} item={item} level={level} />)}
    </ul>
  );
};

interface TableOfContentsItemProps extends Omit<React.ComponentProps<'li'>, 'children'> {
  item: TocItem;
  level?: number;
}

const TableOfContentsItem = ({ item, level = 0, className, ...props }: TableOfContentsItemProps) => {
  return (
    <li data-slot="toc-item" className={className} {...props}>
      <TableOfContentsLink href={`#${item.id}`} level={item.level}>
        {item.text}
      </TableOfContentsLink>
      {item.children && item.children.length > 0 ? (
        <TableOfContentsList items={item.children} level={level + 1} />
      ) : null}
    </li>
  );
};

interface TableOfContentsLinkProps extends React.ComponentProps<'a'> {
  level?: number;
}

const TableOfContentsLink = ({
  href = '',
  level = 2,
  className,
  onClick,
  children,
  ...props
}: TableOfContentsLinkProps) => {
  const { activeId } = useTocContext('TableOfContentsLink');
  const id = href.startsWith('#') ? href.slice(1) : href;
  const isActive = id.length > 0 && activeId === id;

  const handleClick = (event: React.MouseEvent<HTMLAnchorElement>) => {
    onClick?.(event);
    if (event.defaultPrevented) return;
    if (!id || !href.startsWith('#')) return;

    const target = document.getElementById(id);
    if (!target) return;

    event.preventDefault();
    target.scrollIntoView({
      behavior: prefersReducedMotion() ? 'auto' : 'smooth',
    });
    window.history.replaceState(null, '', `#${id}`);
  };

  return (
    <a
      data-slot="toc-link"
      data-active={isActive ? '' : undefined}
      href={href}
      aria-current={isActive ? 'location' : undefined}
      onClick={handleClick}
      className={cn(
        '-ms-px block border-s py-1 ps-4 text-sm transition-colors duration-150 ease-out',
        isActive
          ? 'border-foreground font-medium text-foreground'
          : 'border-transparent text-muted-foreground hover:text-foreground',
        level >= 4 && 'text-xs',
        className,
      )}
      {...props}
    >
      {children}
    </a>
  );
};

export { TableOfContents, TableOfContentsLabel, TableOfContentsList, TableOfContentsItem, TableOfContentsLink };

demo.tsx
"use client";

import * as React from "react";

import { TableOfContents, type TocItem } from "@/components/ui/toc";

const sections: { id: string; title: string; body: string }[] = [
  {
    id: "getting-started",
    title: "Getting started",
    body: "Scroll through the article on the left and watch the marker on the right slide to the section that is currently in view.",
  },
  {
    id: "installation",
    title: "Installation",
    body: "Drop the component into your project and pass it a flat or nested list of headings. Clicking a link smooth-scrolls to the matching section and respects reduced-motion.",
  },
  {
    id: "usage",
    title: "Usage",
    body: "Give each section an id that matches an item in the list. The active heading is tracked automatically as you scroll the page.",
  },
  {
    id: "configuration",
    title: "Configuration",
    body: "Compose from parts or feed it items directly. Provide your own label, or control the active id yourself for full flexibility.",
  },
  {
    id: "accessibility",
    title: "Accessibility",
    body: "The navigation is rendered as a nav landmark and the active link is marked with aria-current for assistive technology.",
  },
  {
    id: "faq",
    title: "FAQ",
    body: "Nested headings, right-to-left layouts and long documents are all handled out of the box.",
  },
];

const items: TocItem[] = sections.map((section) => ({
  id: section.id,
  text: section.title,
  level: 2,
}));

export default function TableOfContentsDemo() {
  return (
    <div className="mx-auto flex w-full max-w-3xl gap-10 bg-background px-6 py-10 text-foreground">
      <article className="min-w-0 flex-1 space-y-10">
        {sections.map((section) => (
          <section
            key={section.id}
            id={section.id}
            className="scroll-mt-16 space-y-3"
          >
            <h2 className="text-lg font-semibold tracking-tight">
              {section.title}
            </h2>
            <p className="text-sm leading-relaxed text-muted-foreground">
              {section.body}
            </p>
          </section>
        ))}
      </article>

      <aside className="sticky top-16 hidden h-fit w-48 shrink-0 md:block">
        <TableOfContents items={items} />
      </aside>
    </div>
  );
}
```

Implementation Guidelines
 1. Analyze the component structure and identify all required dependencies
 2. Review the component's argumens and state
 3. Identify any required context providers or hooks and install them
 4. Questions to Ask
 - What data/props will be passed to this component?
 - Are there any specific state management requirements?
 - Are there any required assets (images, icons, etc.)?
 - What is the expected responsive behavior?
 - What is the best place to use this component in the app?

Steps to integrate
 0. Copy paste all the code above in the correct directories
 1. Install external dependencies
 2. Fill image assets with Unsplash stock images you know exist
 3. Use lucide-react icons for svgs or logos if component requires them
