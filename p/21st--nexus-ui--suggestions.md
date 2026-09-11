<!-- Suggestions · @nexus-ui · https://21st.dev/@nexus-ui/components/suggestions
     license: MIT · category: ai-chat
     A row of clickable prompt suggestion chips that let users pick a common query to fill an AI chat input, with optional variants, vertical layout, and an expandable panel of related suggestions. -->

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
components/nexus-ui/suggestions.tsx
"use client";

import * as React from "react";
import { Presence } from "@radix-ui/react-presence";
import { Slot } from "@radix-ui/react-slot";

import { cva, type VariantProps } from "class-variance-authority";

import { cn } from "@/lib/utils";
import { Button } from "@/components/ui/button";

const suggestionVariants = cva(
  "h-8 gap-1.5 rounded-full px-4 text-sm font-normal shadow-none outline-0 transition-all duration-150 focus-visible:ring-2 focus-visible:ring-ring active:scale-[0.99]",
  {
    variants: {
      variant: {
        filled:
          "border-none bg-muted text-primary hover:bg-border",
        outline:
          "border border-input bg-transparent text-primary hover:bg-muted",
        ghost:
          "border-none bg-transparent text-muted-foreground hover:bg-muted hover:text-primary",
      },
    },
    defaultVariants: {
      variant: "filled",
    },
  },
);

type SuggestionsContextValue = {
  onSelect?: (value: string) => void;
};

const SuggestionsContext = React.createContext<SuggestionsContextValue>({});

type SuggestionsProps = Omit<
  React.HTMLAttributes<HTMLDivElement>,
  "onSelect"
> & {
  onSelect?: (value: string) => void;
};

function Suggestions({ className, onSelect, ...props }: SuggestionsProps) {
  return (
    <SuggestionsContext.Provider value={{ onSelect }}>
      <div
        data-slot="suggestions"
        role="group"
        aria-label="Suggestions"
        className={cn("flex flex-col gap-2", className)}
        {...props}
      />
    </SuggestionsContext.Provider>
  );
}

type SuggestionListProps = React.HTMLAttributes<HTMLDivElement> & {
  orientation?: "horizontal" | "vertical";
};

function SuggestionList({
  className,
  orientation = "horizontal",
  ...props
}: SuggestionListProps) {
  return (
    <div
      data-slot="suggestion-list"
      role="group"
      aria-label="Suggestions"
      className={cn(
        "flex gap-2 duration-150",
        orientation === "horizontal"
          ? "flex-row flex-wrap items-center justify-center"
          : "flex-col items-start",
        className,
      )}
      {...props}
    />
  );
}

type SuggestionProps = Omit<React.ComponentProps<typeof Button>, "variant"> &
  VariantProps<typeof suggestionVariants> & {
    value?: string;
    highlight?: string | string[];
  };

function highlightText(
  text: string,
  terms: string | string[],
): React.ReactNode {
  const termList = Array.isArray(terms) ? terms : [terms];
  const escaped = termList.map((t) => t.replace(/[.*+?^${}()|[\]\\]/g, "\\$&"));
  const pattern = new RegExp(`(${escaped.join("|")})`, "gi");
  const parts = text.split(pattern);

  return (
    <span>
      {parts.map((part, i) =>
        escaped.some((e) => new RegExp(`^${e}$`, "i").test(part)) ? (
          <span key={i} className="text-muted-foreground">
            {part}
          </span>
        ) : (
          <span key={i} className="text-secondary-foreground">
            {part}
          </span>
        ),
      )}
    </span>
  );
}

function Suggestion({
  className,
  value,
  variant = "filled",
  highlight,
  onClick,
  children,
  ...props
}: SuggestionProps) {
  const { onSelect } = React.useContext(SuggestionsContext);

  const textToHighlight =
    typeof children === "string" ? children : (value ?? "");
  const nonStringChildren = React.Children.toArray(children).filter(
    (c) => typeof c !== "string",
  );
  const rendered =
    highlight && textToHighlight ? (
      <>
        {highlightText(textToHighlight, highlight)}
        {nonStringChildren}
      </>
    ) : (
      children
    );

  return (
    <Button
      data-slot="suggestion"
      className={cn(suggestionVariants({ variant }), className)}
      onClick={(e) => {
        onClick?.(e);
        const text = value ?? (typeof children === "string" ? children : "");
        if (text && onSelect) onSelect(text);
      }}
      {...props}
    >
      {rendered}
    </Button>
  );
}

const FOCUSABLE =
  'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])';

function getFocusableElements(container: HTMLElement): HTMLElement[] {
  return Array.from(container.querySelectorAll<HTMLElement>(FOCUSABLE));
}

const SuggestionPanelContext = React.createContext<{
  onOpenChange: (open: boolean) => void;
} | null>(null);

type SuggestionPanelProps = React.ComponentProps<"div"> & {
  open?: boolean;
  onOpenChange?: (open: boolean) => void;
  onClose?: () => void;
};

function SuggestionPanel({
  className,
  open = true,
  onOpenChange,
  onClose,
  ref,
  children,
  ...props
}: SuggestionPanelProps) {
  const panelRef = React.useRef<HTMLDivElement>(null);
  const mergedRef = React.useMemo(
    () => (node: HTMLDivElement | null) => {
      (panelRef as React.MutableRefObject<HTMLDivElement | null>).current =
        node;
      if (typeof ref === "function") ref(node);
      else if (ref)
        (ref as React.MutableRefObject<HTMLDivElement | null>).current = node;
    },
    [ref],
  );

  const handleOpenChange = React.useCallback(
    (next: boolean) => {
      onOpenChange?.(next);
    },
    [onOpenChange],
  );

  const handleAnimationEnd = React.useCallback(
    (e: React.AnimationEvent) => {
      if (e.animationName === "exit" && !open) onClose?.();
    },
    [open, onClose],
  );

  React.useEffect(() => {
    const handleKeyDown = (e: KeyboardEvent) => {
      if (e.key === "Escape") handleOpenChange(false);
    };
    window.addEventListener("keydown", handleKeyDown);
    return () => window.removeEventListener("keydown", handleKeyDown);
  }, [handleOpenChange]);

  React.useEffect(() => {
    if (!open) return;
    const panel = panelRef.current;
    if (!panel) return;
    const focusable = getFocusableElements(panel);
    if (focusable.length > 0) focusable[0].focus();
  }, [open]);

  React.useEffect(() => {
    const panel = panelRef.current;
    if (!panel) return;

    const handleKeyDown = (e: KeyboardEvent) => {
      if (e.key !== "Tab") return;
      const focusable = getFocusableElements(panel);
      if (focusable.length === 0) return;

      const first = focusable[0];
      const last = focusable[focusable.length - 1];
      const active = document.activeElement as HTMLElement | null;

      if (e.shiftKey) {
        if (active === first) {
          e.preventDefault();
          last.focus();
        }
      } else {
        if (active === last) {
          e.preventDefault();
          first.focus();
        }
      }
    };

    panel.addEventListener("keydown", handleKeyDown);
    return () => panel.removeEventListener("keydown", handleKeyDown);
  }, []);

  const ctx = React.useMemo(
    () => ({ onOpenChange: handleOpenChange }),
    [handleOpenChange],
  );

  return (
    <Presence present={open}>
      <div
        ref={mergedRef}
        data-slot="suggestion-panel"
        role="dialog"
        aria-modal="true"
        aria-label="Suggestions panel"
        data-state={open ? "open" : "closed"}
        onAnimationEnd={handleAnimationEnd}
        className={cn(
          "rounded-t-0 absolute inset-x-0 -top-7.5 z-0 mx-auto flex w-[calc(100%-16px)] flex-col items-center justify-center gap-3 rounded-b-2xl bg-muted px-2 py-3 duration-200 data-[state=closed]:animate-out data-[state=closed]:duration-0 data-[state=closed]:fade-out-0 data-[state=closed]:slide-out-to-top-2 data-[state=open]:animate-in data-[state=open]:fade-in-0 data-[state=open]:slide-in-from-top-2",
          className,
        )}
        {...props}
      >
        <SuggestionPanelContext.Provider value={ctx}>
          {children}
        </SuggestionPanelContext.Provider>
      </div>
    </Presence>
  );
}

function SuggestionPanelHeader({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) {
  return (
    <div
      data-slot="suggestion-panel-header"
      className={cn("flex w-full items-center justify-between px-3", className)}
      {...props}
    />
  );
}

function SuggestionPanelTitle({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) {
  return (
    <div
      data-slot="suggestion-panel-title"
      className={cn("flex items-center gap-1.5", className)}
      {...props}
    />
  );
}

type SuggestionPanelCloseProps =
  React.ButtonHTMLAttributes<HTMLButtonElement> & {
    asChild?: boolean;
  };

function SuggestionPanelClose({
  asChild = false,
  className,
  onClick,
  "aria-label": _ariaLabel,
  ...props
}: SuggestionPanelCloseProps) {
  const ctx = React.useContext(SuggestionPanelContext);
  const Comp = asChild ? Slot : "button";

  const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
    ctx?.onOpenChange(false);
    onClick?.(e);
  };

  return (
    <Comp
      type={asChild ? undefined : "button"}
      data-slot="suggestion-panel-close"
      aria-label="Close suggestions panel"
      className={cn(
        "flex cursor-pointer items-center justify-center text-muted-foreground hover:text-primary dark:hover:text-primary",
        className,
      )}
      onClick={handleClick}
      {...props}
    />
  );
}

type SuggestionPanelContentProps = React.HTMLAttributes<HTMLDivElement> & {
  asChild?: boolean;
};

function SuggestionPanelContent({
  asChild = false,
  className,
  ...props
}: SuggestionPanelContentProps) {
  const Comp = asChild ? Slot : "div";
  return (
    <Comp
      data-slot="suggestion-panel-content"
      className={cn("w-full", className)}
      {...props}
    />
  );
}

export {
  Suggestions,
  SuggestionList,
  Suggestion,
  SuggestionPanel,
  SuggestionPanelHeader,
  SuggestionPanelTitle,
  SuggestionPanelClose,
  SuggestionPanelContent,
};

demo.tsx
"use client";

import {
  Suggestions,
  SuggestionList,
  Suggestion,
} from "@/components/ui/suggestions";

export default function SuggestionDefault() {
  return (
    <Suggestions onSelect={(value) => console.log(value)}>
      <SuggestionList className="justify-center max-w-lg">
        <Suggestion>What is AI?</Suggestion>
        <Suggestion>Teach me Engineering from scratch</Suggestion>
        <Suggestion>Design a weekly workout plan</Suggestion>
        <Suggestion>Places to visit in France</Suggestion>
        <Suggestion>How to learn React?</Suggestion>
      </SuggestionList>
    </Suggestions>
  );
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-presence @radix-ui/react-slot class-variance-authority
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
