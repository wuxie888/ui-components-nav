<!-- Smooth Button · @educalvolpz · https://21st.dev/@educalvolpz/components/smooth-button
     license: MIT · category: button
     A polished button component with gradient candy/destructive variants, sizes, and a subtle press-scale animation. -->

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
"use client";

import { Slot } from "@radix-ui/react-slot";
import { cn } from "@/lib/utils";
import { cva, type VariantProps } from "class-variance-authority";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import type { ButtonHTMLAttributes, ReactNode, Ref } from "react";
import { useEffect, useRef } from "react";

/**
 * SmoothButton — the first primitive of the SmoothUI Design System.
 *
 * Three orthogonal axes (see design-system/components/button.md):
 *   variant → appearance only (solid | soft | outline | ghost | link | candy)
 *   color   → hue (accent | neutral | destructive | blue | amber | green)
 *   size    → xs | sm | default | lg + icon-*
 *
 * The `color` axis only sets CSS custom props (--btn, --btn-hover, --btn-fg);
 * each `variant` consumes them generically, so 6 variants × 6 colors stay 12
 * class strings, not 36. When no `color` is given, CSS var fallbacks apply
 * (candy → brand, everything else → neutral/foreground).
 *
 * Legacy variants `default` / `secondary` / `destructive` are preserved verbatim
 * for back-compat with existing call sites and ignore the `color` axis.
 */
const smoothButtonVariants = cva(
  "relative inline-flex cursor-pointer items-center justify-center gap-2 whitespace-nowrap font-medium outline-none ring-offset-background transition-[transform,background-color,border-color,color,box-shadow] duration-150 ease-out focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 active:scale-[0.97] disabled:pointer-events-none disabled:opacity-50 motion-reduce:transition-none motion-reduce:active:scale-100 [&_svg]:pointer-events-none [&_svg]:shrink-0",
  {
    defaultVariants: {
      shape: "default",
      size: "default",
      variant: "default",
    },
    variants: {
      color: {
        accent:
          "[--btn-fg:#fff] [--btn-hover:var(--color-brand-secondary)] [--btn:var(--color-brand)]",
        amber:
          "[--btn-fg:var(--color-amber-fg)] [--btn-hover:var(--color-amber-hover)] [--btn:var(--color-amber)]",
        blue: "[--btn-fg:var(--color-blue-fg)] [--btn-hover:var(--color-blue-hover)] [--btn:var(--color-blue)]",
        destructive:
          "[--btn-fg:#fff] [--btn-hover:color-mix(in_oklab,var(--color-destructive)_85%,black)] [--btn:var(--color-destructive)]",
        green:
          "[--btn-fg:var(--color-green-fg)] [--btn-hover:var(--color-green-hover)] [--btn:var(--color-green)]",
        neutral:
          "[--btn-fg:var(--color-background)] [--btn-hover:var(--color-smooth-900)] [--btn:var(--color-foreground)]",
      },
      shape: {
        default: "",
        pill: "rounded-full!",
        square: "rounded-none!",
      },
      size: {
        default: "h-10 gap-2 rounded-md px-4 py-2 text-sm [&_svg]:size-4",
        icon: "size-10 rounded-md [&_svg]:size-4",
        "icon-lg": "size-11 rounded-lg [&_svg]:size-5",
        "icon-sm": "size-9 rounded-md [&_svg]:size-4",
        lg: "h-11 gap-2 rounded-lg px-8 text-base [&_svg]:size-5",
        sm: "h-9 gap-1.5 rounded-md px-3 text-sm [&_svg]:size-4",
        xs: "h-7 gap-1.5 rounded-sm px-2.5 text-xs [&_svg]:size-3.5",
      },
      variant: {
        candy:
          "border-[0.5px] border-white/25 bg-gradient-to-b from-[var(--btn,var(--color-brand))] to-[var(--btn-hover,var(--color-brand-secondary))] text-[var(--btn-fg,#fff)] text-shadow-sm shadow-black/20 shadow-md ring-1 ring-[color-mix(in_oklab,var(--color-foreground)_15%,var(--btn,var(--color-brand)))] hover:from-[var(--btn-hover,var(--color-brand-secondary))] hover:to-[var(--btn-hover,var(--color-brand-secondary))] [&_svg]:drop-shadow-sm",
        // --- legacy (preserved verbatim, ignore `color`) ---
        default:
          "bg-primary text-primary-foreground shadow-xs hover:bg-primary/90",
        destructive:
          "bg-gradient-to-b from-[#FD4B4E] to-destructive text-shadow-sm text-white shadow-[0px_1px_2px_rgba(0,0,0,0.4),0px_0px_0px_1px_#F61418,inset_0px_0.75px_0px_rgba(255,255,255,0.2)] hover:from-destructive hover:to-destructive",
        ghost:
          "text-[var(--btn,var(--color-foreground))] hover:bg-[color-mix(in_oklab,var(--btn,var(--color-foreground))_10%,transparent)]",
        link: "text-[var(--btn,var(--color-foreground))] underline-offset-4 hover:underline",
        outline:
          "border border-transparent bg-background text-[var(--btn,var(--color-foreground))] shadow-black/15 shadow-sm ring-1 ring-foreground/10 hover:bg-primary dark:ring-foreground/15",
        secondary:
          "bg-secondary text-secondary-foreground shadow-xs hover:bg-secondary/80",
        soft: "bg-[color-mix(in_oklab,var(--btn,var(--color-foreground))_12%,transparent)] text-[var(--btn,var(--color-foreground))] hover:bg-[color-mix(in_oklab,var(--btn,var(--color-foreground))_18%,transparent)]",
        // --- new decoupled system (consume --btn / --btn-hover / --btn-fg) ---
        solid:
          "bg-[var(--btn,var(--color-foreground))] text-[var(--btn-fg,var(--color-background))] shadow-xs hover:bg-[var(--btn-hover,var(--color-smooth-900))]",
      },
    },
  }
);

export type SmoothButtonProps = Omit<
  ButtonHTMLAttributes<HTMLButtonElement>,
  "prefix" | "color"
> &
  VariantProps<typeof smoothButtonVariants> & {
    asChild?: boolean;
    /** Show a spinner that morphs the button width without layout jump. */
    loading?: boolean;
    /** Content before the label (icon, Kbd…). */
    prefix?: ReactNode;
    /** Content after the label. */
    suffix?: ReactNode;
    /** Opt-in Safari force-press depth (scales to 0.94 under pressure). */
    forcePress?: boolean;
    ref?: Ref<HTMLButtonElement>;
  };

const Spinner = () => (
  <svg
    aria-hidden="true"
    className="size-[1em] animate-spin"
    fill="none"
    viewBox="0 0 24 24"
  >
    <circle
      className="opacity-25"
      cx="12"
      cy="12"
      r="10"
      stroke="currentColor"
      strokeWidth="3"
    />
    <path
      className="opacity-90"
      d="M12 2a10 10 0 0 1 10 10"
      stroke="currentColor"
      strokeLinecap="round"
      strokeWidth="3"
    />
  </svg>
);

function SmoothButton({
  className,
  variant,
  color,
  size,
  shape,
  asChild = false,
  loading = false,
  forcePress = false,
  prefix,
  suffix,
  disabled,
  children,
  ref,
  ...props
}: SmoothButtonProps) {
  const shouldReduceMotion = useReducedMotion();
  const localRef = useRef<HTMLButtonElement>(null);

  // Safari-only force-press: deepen the press past the normal active scale.
  useEffect(() => {
    const node = localRef.current;
    if (!(forcePress && node) || shouldReduceMotion) {
      return;
    }
    const FORCE_THRESHOLD = 2; // Safari's "force click" boundary
    const onForce = (e: Event) => {
      const force = (e as Event & { webkitForce?: number }).webkitForce ?? 0;
      node.style.transform = force >= FORCE_THRESHOLD ? "scale(0.94)" : "";
    };
    const reset = () => {
      node.style.transform = "";
    };
    node.addEventListener("webkitmouseforcechanged", onForce);
    node.addEventListener("mouseup", reset);
    node.addEventListener("mouseleave", reset);
    return () => {
      node.removeEventListener("webkitmouseforcechanged", onForce);
      node.removeEventListener("mouseup", reset);
      node.removeEventListener("mouseleave", reset);
    };
  }, [forcePress, shouldReduceMotion]);

  const classes = cn(
    smoothButtonVariants({ className, color, shape, size, variant })
  );

  // asChild defers all rendering to the consumer's element — slots/loading
  // are not injected (single-child contract of Radix Slot).
  if (asChild) {
    return (
      <Slot className={classes} ref={ref} {...props}>
        {children}
      </Slot>
    );
  }

  const setRefs = (node: HTMLButtonElement | null) => {
    localRef.current = node;
    if (typeof ref === "function") {
      ref(node);
    } else if (ref) {
      (ref as { current: HTMLButtonElement | null }).current = node;
    }
  };

  return (
    <button
      aria-busy={loading || undefined}
      className={classes}
      disabled={disabled || loading}
      ref={setRefs}
      type={props.type ?? "button"}
      {...props}
    >
      <AnimatePresence initial={false}>
        {loading ? (
          <motion.span
            animate={{ marginRight: "0.5rem", opacity: 1, width: "1em" }}
            className="inline-flex shrink-0 items-center justify-center overflow-hidden"
            exit={{ marginRight: 0, opacity: 0, width: 0 }}
            initial={
              shouldReduceMotion
                ? { marginRight: "0.5rem", opacity: 1, width: "1em" }
                : { marginRight: 0, opacity: 0, width: 0 }
            }
            key="spinner"
            transition={
              shouldReduceMotion
                ? { duration: 0 }
                : { bounce: 0.1, duration: 0.25, type: "spring" }
            }
          >
            <Spinner />
          </motion.span>
        ) : null}
      </AnimatePresence>
      {prefix}
      {children}
      {suffix}
    </button>
  );
}

export default SmoothButton;
export { smoothButtonVariants };

demo.tsx
"use client";

import SmoothButton from "@/components/ui/smooth-button";
import { ExternalLink, Heart, Trash2 } from "lucide-react";

export default function SmoothButtonDemo() {
  return (
    <div className="flex flex-col items-center gap-6">
      <div className="flex flex-wrap items-center justify-center gap-3">
        <SmoothButton variant="default">Default</SmoothButton>
        <SmoothButton variant="candy">
          Candy <ExternalLink className="h-4 w-4" />
        </SmoothButton>
        <SmoothButton variant="destructive">
          <Trash2 className="h-4 w-4" /> Delete
        </SmoothButton>
        <SmoothButton variant="outline">Outline</SmoothButton>
        <SmoothButton variant="secondary">Secondary</SmoothButton>
        <SmoothButton variant="ghost">Ghost</SmoothButton>
        <SmoothButton variant="link">Link</SmoothButton>
      </div>
      <div className="flex flex-wrap items-center justify-center gap-3">
        <SmoothButton size="sm" variant="candy">
          Small
        </SmoothButton>
        <SmoothButton size="default" variant="candy">
          Default
        </SmoothButton>
        <SmoothButton size="lg" variant="candy">
          Large
        </SmoothButton>
        <SmoothButton size="icon" variant="candy">
          <Heart className="h-4 w-4" />
        </SmoothButton>
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-slot class-variance-authority motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add tokens.json
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
