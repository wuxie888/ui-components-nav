<!-- Code Block · @skyleen77 · https://21st.dev/@skyleen77/components/primitives-animate-code-block
     license: MIT · category: text
     An animated code block that types out syntax-highlighted source character by character as it comes into view. -->

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
components/ui/index.tsx
'use client';

import * as React from 'react';

import {
  useIsInView,
  type UseIsInViewOptions,
} from '@/hooks/use-is-in-view';

type CodeBlockProps = React.ComponentProps<'div'> & {
  code: string;
  lang: string;
  theme?: 'light' | 'dark';
  themes?: { light: string; dark: string };
  writing?: boolean;
  duration?: number;
  delay?: number;
  onDone?: () => void;
  onWrite?: (info: { index: number; length: number; done: boolean }) => void;
  scrollContainerRef?: React.RefObject<HTMLElement | null>;
} & UseIsInViewOptions;

function CodeBlock({
  ref,
  code,
  lang,
  theme = 'light',
  themes = {
    light: 'github-light',
    dark: 'github-dark',
  },
  writing = false,
  duration = 5000,
  delay = 0,
  onDone,
  onWrite,
  scrollContainerRef,
  inView = false,
  inViewOnce = true,
  inViewMargin = '0px',
  ...props
}: CodeBlockProps) {
  const { ref: localRef, isInView } = useIsInView(
    ref as React.Ref<HTMLDivElement>,
    {
      inView,
      inViewOnce,
      inViewMargin,
    },
  );

  const [visibleCode, setVisibleCode] = React.useState('');
  const [highlightedCode, setHighlightedCode] = React.useState('');
  const [isDone, setIsDone] = React.useState(false);

  React.useEffect(() => {
    if (!visibleCode.length || !isInView) return;

    const loadHighlightedCode = async () => {
      try {
        const { codeToHtml } = await import('shiki');

        const highlighted = await codeToHtml(visibleCode, {
          lang,
          themes,
          defaultColor: theme,
        });

        setHighlightedCode(highlighted);
      } catch (e) {
        console.error(`Language "${lang}" could not be loaded.`, e);
      }
    };

    loadHighlightedCode();
  }, [lang, themes, writing, isInView, duration, delay, visibleCode, theme]);

  React.useEffect(() => {
    if (!writing) {
      setVisibleCode(code);
      onDone?.();
      onWrite?.({ index: code.length, length: code.length, done: true });
      return;
    }

    if (!code.length || !isInView) return;

    const characters = Array.from(code);
    let index = 0;
    const totalDuration = duration;
    const interval = totalDuration / characters.length;
    let intervalId: NodeJS.Timeout;

    const timeout = setTimeout(() => {
      intervalId = setInterval(() => {
        if (index < characters.length) {
          setVisibleCode(() => {
            const nextChar = characters.slice(0, index + 1).join('');
            onWrite?.({
              index: index + 1,
              length: characters.length,
              done: false,
            });
            index += 1;
            return nextChar;
          });
          localRef.current?.scrollTo({
            top: localRef.current?.scrollHeight,
            behavior: 'smooth',
          });
        } else {
          clearInterval(intervalId);
          setIsDone(true);
          onDone?.();
          onWrite?.({
            index: characters.length,
            length: characters.length,
            done: true,
          });
        }
      }, interval);
    }, delay);

    return () => {
      clearTimeout(timeout);
      clearInterval(intervalId);
    };
  }, [code, duration, delay, isInView, writing, onDone, onWrite, localRef]);

  React.useEffect(() => {
    if (!writing || !isInView) return;
    const el =
      scrollContainerRef?.current ??
      (localRef.current?.parentElement as HTMLElement | null) ??
      (localRef.current as unknown as HTMLElement | null);

    if (!el) return;

    requestAnimationFrame(() => {
      el.scrollTo({
        top: el.scrollHeight,
        behavior: 'smooth',
      });
    });
  }, [highlightedCode, writing, isInView, scrollContainerRef, localRef]);

  return (
    <div
      ref={localRef}
      data-slot="code-block"
      data-writing={writing}
      data-done={isDone}
      dangerouslySetInnerHTML={{ __html: highlightedCode }}
      {...props}
    />
  );
}

export { CodeBlock, type CodeBlockProps };

demo.tsx
'use client';

import { CodeBlock } from '@/components/ui/primitives-animate-code-block';
import { cn } from '@/lib/utils';
import { useTheme } from 'next-themes';

interface CodeBlockDemoProps {
  duration: number;
  delay: number;
  writing: boolean;
  cursor: boolean;
}

const CodeBlockDemo = ({
  duration,
  delay,
  writing,
  cursor,
}: CodeBlockDemoProps) => {
  const { resolvedTheme } = useTheme();

  return (
    <div
      key={`${duration}-${delay}-${writing}-${cursor}`}
      className="relative bg-accent w-[420px] h-[372px] text-sm p-4 overflow-auto"
    >
      <CodeBlock
        code={`'use client';
 
import * as React from 'react';
 
type MyComponentProps = {
  myProps: string;
} & React.ComponentProps<'div'>;
 
function MyComponent(props: MyComponentProps) {
  return (
    <div {...props}>
      <p>My Component</p>
    </div>
  );
}

export { MyComponent, type MyComponentProps };`}
        lang="tsx"
        theme={resolvedTheme === 'dark' ? 'dark' : 'light'}
        writing={writing}
        duration={duration}
        delay={delay}
        className={cn(
          '[&>pre,_&_code]:!bg-transparent [&>pre,_&_code]:[background:transparent_!important] [&>pre,_&_code]:border-none [&_code]:!text-[13px] [&_code_.line]:!px-0',
          cursor &&
            "data-[done=false]:[&_.line:last-of-type::after]:content-['|'] data-[done=false]:[&_.line:last-of-type::after]:inline-block data-[done=false]:[&_.line:last-of-type::after]:w-[1ch] data-[done=false]:[&_.line:last-of-type::after]:-translate-px",
        )}
      />
    </div>
  );
};

export default function Demo() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center p-6">
      <CodeBlockDemo duration={10000} delay={0} writing cursor />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install shiki
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add hooks-use-is-in-view
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
