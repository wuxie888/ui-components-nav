<!-- Tooltip · @edwinvakayil · https://21st.dev/@edwinvakayil/components/tooltip
     license: unspecified · category: tooltip
      -->

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
components/ui/tooltip.tsx
"use client";

import * as PopoverPrimitive from "@radix-ui/react-popover";
import { Slot } from "@radix-ui/react-slot";
import { AnimatePresence, motion } from "motion/react";
import * as React from "react";

import { cn } from "@/lib/utils";

const controlCornerClassName =
  "rounded-lg supports-[corner-shape:squircle]:corner-squircle supports-[corner-shape:squircle]:rounded-[11px]";

const tooltipThemeClassName =
  "[--tt-surface:#111111] [--tt-foreground:#ffffff] dark:[--tt-surface:#f6f3ec] dark:[--tt-foreground:#111111]";

const tooltipContentClassName = cn(
  controlCornerClassName,
  "group/tooltip pointer-events-none relative z-50 max-w-60 whitespace-normal bg-[color:var(--tt-surface)] px-3 py-1.5 font-medium text-[color:var(--tt-foreground)] text-xs leading-snug shadow-[0_4px_24px_-4px_rgba(0,0,0,0.25)]"
);

const tooltipArrowClassName =
  "absolute h-2 w-2 rotate-45 bg-[color:var(--tt-surface)] group-data-[side=bottom]/tooltip:-top-1 group-data-[side=left]/tooltip:top-1/2 group-data-[side=right]/tooltip:top-1/2 group-data-[side=left]/tooltip:-right-1 group-data-[side=top]/tooltip:-bottom-1 group-data-[side=bottom]/tooltip:left-1/2 group-data-[side=right]/tooltip:-left-1 group-data-[side=top]/tooltip:left-1/2 group-data-[side=bottom]/tooltip:-translate-x-1/2 group-data-[side=top]/tooltip:-translate-x-1/2 group-data-[side=left]/tooltip:-translate-y-1/2 group-data-[side=right]/tooltip:-translate-y-1/2";

type Side = "top" | "bottom" | "left" | "right";
type TooltipTriggerElement = React.ReactElement<{
  "aria-describedby"?: string;
}>;

export interface TooltipProps {
  children: TooltipTriggerElement;
  content: string;
  side?: Side;
  delay?: number;
  className?: string;
}

const MAX_TOOLTIP_CHARACTERS = 80;

function isTooltipTriggerElement(
  node: React.ReactNode
): node is TooltipTriggerElement {
  return React.isValidElement(node) && node.type !== React.Fragment;
}

function mergeDescribedBy(...ids: Array<string | undefined>) {
  const merged = ids.filter(Boolean).join(" ");

  return merged.length > 0 ? merged : undefined;
}

export function Tooltip({
  children,
  content,
  side = "top",
  delay = 0.15,
  className,
}: TooltipProps) {
  const [open, setOpen] = React.useState(false);
  const timeoutRef = React.useRef<ReturnType<typeof setTimeout>>(undefined);
  const tooltipId = React.useId();
  const normalizedContent = content.trim();

  if (!isTooltipTriggerElement(children)) {
    throw new Error(
      "Tooltip expects a single element child so it can forward hover, focus, and accessibility props."
    );
  }

  React.useEffect(() => {
    if (
      process.env.NODE_ENV !== "production" &&
      (normalizedContent.length > MAX_TOOLTIP_CHARACTERS ||
        normalizedContent.includes("\n"))
    ) {
      console.warn(
        "Tooltip content should stay short, single-line, and non-interactive. Use Popover for longer or multiline content."
      );
    }
  }, [normalizedContent]);

  const childAriaDescribedBy = children.props["aria-describedby"];
  const triggerDescription = open
    ? mergeDescribedBy(childAriaDescribedBy, tooltipId)
    : childAriaDescribedBy;

  const handleEnter = () => {
    clearTimeout(timeoutRef.current);
    timeoutRef.current = setTimeout(() => setOpen(true), delay * 1000);
  };
  const handleLeave = () => {
    clearTimeout(timeoutRef.current);
    setOpen(false);
  };

  React.useEffect(() => () => clearTimeout(timeoutRef.current), []);

  if (normalizedContent.length === 0) {
    return children;
  }

  return (
    <PopoverPrimitive.Root modal={false} onOpenChange={setOpen} open={open}>
      <PopoverPrimitive.Anchor asChild>
        <Slot
          aria-describedby={triggerDescription}
          onBlur={handleLeave}
          onFocus={handleEnter}
          onMouseEnter={handleEnter}
          onMouseLeave={handleLeave}
        >
          {children}
        </Slot>
      </PopoverPrimitive.Anchor>

      <AnimatePresence>
        {open && (
          <PopoverPrimitive.Portal forceMount>
            <PopoverPrimitive.Content
              align="center"
              asChild
              avoidCollisions
              collisionPadding={12}
              forceMount
              onCloseAutoFocus={(event) => event.preventDefault()}
              onOpenAutoFocus={(event) => event.preventDefault()}
              side={side}
              sideOffset={10}
            >
              <motion.div
                animate={{
                  opacity: 1,
                  scale: 1,
                  filter: "blur(0px)",
                }}
                className={cn(
                  tooltipThemeClassName,
                  tooltipContentClassName,
                  className
                )}
                exit={{
                  opacity: 0,
                  scale: 0.92,
                  filter: "blur(4px)",
                }}
                id={tooltipId}
                initial={{
                  opacity: 0,
                  scale: 0.92,
                  filter: "blur(4px)",
                }}
                role="tooltip"
                style={{
                  transformOrigin:
                    "var(--radix-popover-content-transform-origin)",
                }}
                transition={{
                  type: "spring",
                  stiffness: 400,
                  damping: 24,
                  mass: 0.6,
                }}
              >
                <motion.span
                  animate={{ scale: 1 }}
                  className={tooltipArrowClassName}
                  exit={{ scale: 0.95, opacity: 0 }}
                  initial={{ scale: 0.95, opacity: 0 }}
                  transition={{
                    type: "spring",
                    stiffness: 500,
                    damping: 28,
                    delay: 0.03,
                  }}
                />
                {normalizedContent}
              </motion.div>
            </PopoverPrimitive.Content>
          </PopoverPrimitive.Portal>
        )}
      </AnimatePresence>
    </PopoverPrimitive.Root>
  );
}

export { Tooltip as tooltip };

demo.tsx
import { Tooltip } from "@/components/ui/tooltip";

const triggerClass =
  "rounded-lg px-0.5 font-semibold underline decoration-dotted underline-offset-[5px] transition-colors";

export function TooltipPreview() {
  return (
    <div className="flex min-h-[260px] flex-col items-center justify-center gap-8 px-4 py-8">
      <blockquote className="max-w-lg text-center">
        <p className="text-lg font-medium leading-relaxed tracking-tight dark:text-neutral-100">
          Win the{" "}
          <Tooltip content="Press, recycle, stay compact." delay={0.12} side="top">
            <button
              className={`${triggerClass} text-emerald-700 decoration-emerald-500/40 dark:text-emerald-400`}
              type="button"
            >
              midfield
            </button>
          </Tooltip>
          , then{" "}
          <Tooltip content="One ball behind the line." side="bottom">
            <button
              className={`${triggerClass} text-sky-700 decoration-sky-500/40 dark:text-sky-400`}
              type="button"
            >
              break the last line
            </button>
          </Tooltip>
          — that&apos;s the half in two beats.
        </p>
      </blockquote>

      <p className="max-w-sm text-center text-[13px] leading-relaxed text-neutral-500 dark:text-neutral-400">
        Hover the calls or{" "}
        <Tooltip content="Tab in — same note." side="right">
          <button
            className={`${triggerClass} text-neutral-700 decoration-neutral-400/70 dark:text-neutral-300`}
            type="button"
          >
            use the keyboard
          </button>
        </Tooltip>
        .
      </p>
    </div>
  );
}

export default TooltipPreview
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-popover @radix-ui/react-slot @radix-ui/react-tooltip motion
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
