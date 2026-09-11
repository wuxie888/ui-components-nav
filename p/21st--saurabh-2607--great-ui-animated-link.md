<!-- Great UI Animated Link · @saurabh-2607 · https://21st.dev/@saurabh-2607/components/great-ui-animated-link
     license: MIT · category: navigation-menu
     An interactive link component supporting 13 premium hover variants including custom clipping masks, SVG sine waves, and text marquee animations. -->

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
components/ui/AnimatedLink.tsx
"use client";

import React, { useState } from "react";
import Link, { LinkProps } from "next/link";
import { motion } from "motion/react";
import { cn } from "@/lib/utils";

export const variantStyles = {
  underline:
    "relative inline-block text-xl font-medium text-neutral-600 dark:text-neutral-400 after:absolute after:-bottom-1 after:left-0 after:h-[2px] after:w-full after:origin-bottom-right after:scale-x-0 after:bg-neutral-900 dark:after:bg-white after:transition-transform after:duration-300 after:ease-out hover:text-neutral-900 dark:hover:text-white hover:after:origin-bottom-left hover:after:scale-x-100",
  centerUnderline:
    "relative inline-block text-xl font-medium text-neutral-600 dark:text-neutral-400 after:absolute after:-bottom-1 after:left-0 after:h-[2px] after:w-full after:origin-center after:scale-x-0 after:bg-neutral-900 dark:after:bg-white after:transition-transform after:duration-300 after:ease-out hover:text-neutral-900 dark:hover:text-white hover:after:scale-x-100",
  overline:
    "relative inline-block text-xl font-medium text-neutral-600 dark:text-neutral-400 after:absolute after:-top-1 after:left-0 after:h-[2px] after:w-full after:origin-top-right after:scale-x-0 after:bg-neutral-900 dark:after:bg-white after:transition-transform after:duration-300 after:ease-out hover:text-neutral-900 dark:hover:text-white hover:after:origin-top-left hover:after:scale-x-100",
  verticalLines:
    "relative inline-block px-2 text-xl font-medium text-neutral-600 dark:text-neutral-400 transition-colors duration-300 hover:text-neutral-900 dark:hover:text-white before:absolute before:bottom-0 before:left-0 before:h-0 before:w-[2px] before:bg-neutral-900 dark:before:bg-white before:transition-all before:duration-300 hover:before:h-full after:absolute after:top-0 after:right-0 after:h-0 after:w-[2px] after:bg-neutral-900 dark:after:bg-white after:transition-all after:duration-300 hover:after:h-full",
  revealLine:
    "relative inline-block overflow-hidden text-xl font-medium text-neutral-600 transition-all duration-300 hover:text-neutral-900 dark:text-neutral-400 dark:hover:text-white after:absolute after:bottom-0 after:left-0 after:h-[2px] after:w-full after:-translate-x-[101%] after:bg-neutral-900 after:transition-transform after:duration-300 hover:after:translate-x-0 dark:after:bg-white",
  fadeUpLine:
    "relative inline-block text-xl font-medium text-neutral-600 dark:text-neutral-400 transition-colors duration-300 hover:text-neutral-900 dark:hover:text-white after:absolute after:-bottom-2 after:left-0 after:h-[1px] after:w-full after:bg-neutral-900 dark:after:bg-white after:opacity-0 after:transition-all after:duration-300 hover:after:-bottom-1 hover:after:opacity-100",
  dashHover:
    "group relative inline-flex items-center text-xl font-medium text-neutral-600 dark:text-neutral-400 transition-colors duration-300 hover:text-neutral-900 dark:hover:text-white before:content-[''] before:w-0 before:h-[1px] before:bg-neutral-900 dark:before:bg-white before:mr-0 before:opacity-0 before:transition-all before:duration-300 hover:before:w-4 hover:before:mr-2 hover:before:opacity-100",
};

export type AnimatedLinkVariant =
  | "underline"
  | "centerUnderline"
  | "overline"
  | "verticalLines"
  | "revealLine"
  | "fadeUpLine"
  | "dashHover"
  | "clipFillY"
  | "clipFillX"
  | "clipCenter"
  | "clipDoodle"
  | "wavy"
  | "textRise";

export interface AnimatedLinkProps
  extends
    Omit<React.AnchorHTMLAttributes<HTMLAnchorElement>, keyof LinkProps>,
    LinkProps {
  children?: React.ReactNode;
  variant?: AnimatedLinkVariant;
  className?: string;
  showArrow?: boolean;
}

export default function AnimatedLink({
  href,
  children,
  variant = "underline",
  className = "",
  showArrow = false,
  ...props
}: AnimatedLinkProps) {
  const [isHovered, setIsHovered] = useState(false);
  const resolvedVariant =
    variantStyles[variant as keyof typeof variantStyles] ??
    variantStyles.underline;

  if (variant === "clipFillY") {
    return (
      <Link
        href={href}
        className={cn(
          "relative inline-block text-xl font-medium text-neutral-600 dark:text-neutral-400",
          className,
        )}
        onMouseEnter={() => setIsHovered(true)}
        onMouseLeave={() => setIsHovered(false)}
        {...props}
      >
        <span className="relative block">{children}</span>
        <motion.span
          className="pointer-events-none absolute inset-0 block text-neutral-900 dark:text-white"
          initial={{ clipPath: "inset(100% 0 0 0)" }}
          animate={{
            clipPath: isHovered ? "inset(0% 0 0 0)" : "inset(100% 0 0 0)",
          }}
          transition={{ ease: [0.19, 1, 0.22, 1], duration: 0.5 }}
          aria-hidden="true"
        >
          {children}
        </motion.span>
      </Link>
    );
  }

  if (variant === "clipFillX") {
    return (
      <Link
        href={href}
        className={cn(
          "relative inline-block text-xl font-medium text-neutral-600 dark:text-neutral-400",
          className,
        )}
        onMouseEnter={() => setIsHovered(true)}
        onMouseLeave={() => setIsHovered(false)}
        {...props}
      >
        <span className="relative block">{children}</span>
        <motion.span
          className="pointer-events-none absolute inset-0 block text-neutral-900 dark:text-white"
          initial={{ clipPath: "inset(0 100% 0 0)" }}
          animate={{
            clipPath: isHovered ? "inset(0 0% 0 0)" : "inset(0 100% 0 0)",
          }}
          transition={{ ease: [0.19, 1, 0.22, 1], duration: 0.5 }}
          aria-hidden="true"
        >
          {children}
        </motion.span>
      </Link>
    );
  }

  if (variant === "clipCenter") {
    return (
      <Link
        href={href}
        className={cn(
          "relative inline-block text-xl font-medium text-neutral-600 dark:text-neutral-400",
          className,
        )}
        onMouseEnter={() => setIsHovered(true)}
        onMouseLeave={() => setIsHovered(false)}
        {...props}
      >
        <span className="relative block">{children}</span>
        <motion.span
          className="pointer-events-none absolute inset-0 block whitespace-nowrap text-neutral-900 dark:text-white"
          initial={{ clipPath: "inset(0% 50% 0% 50%)" }}
          animate={{
            clipPath: isHovered ? "inset(0% 0% 0% 0%)" : "inset(0% 50% 0% 50%)",
          }}
          transition={{ ease: [0.19, 1, 0.22, 1], duration: 0.5 }}
          aria-hidden="true"
        >
          {children}
        </motion.span>
      </Link>
    );
  }

  if (variant === "clipDoodle") {
    const doodleShape =
      "polygon(0% 40%, 10% 20%, 20% 60%, 30% 30%, 40% 70%, 50% 40%, 60% 80%, 70% 30%, 80% 60%, 90% 20%, 100% 40%, 100% 60%, 90% 50%, 80% 80%, 70% 60%, 60% 100%, 50% 60%, 40% 90%, 30% 60%, 20% 80%, 10% 50%, 0% 70%)";

    return (
      <Link
        href={href}
        className={cn(
          "relative inline-block text-xl font-medium text-neutral-600 transition-colors duration-300 hover:text-neutral-900 dark:text-neutral-400 dark:hover:text-white",
          className,
        )}
        onMouseEnter={() => setIsHovered(true)}
        onMouseLeave={() => setIsHovered(false)}
        {...props}
      >
        <span className="relative z-10">{children}</span>
        <div
          className="pointer-events-none absolute -bottom-1 left-0 h-[4px] w-full"
          style={{ clipPath: doodleShape }}
        >
          <motion.div
            className="h-full w-full bg-neutral-900 dark:bg-white"
            initial={{ clipPath: "inset(0 100% 0 0)" }}
            animate={{
              clipPath: isHovered ? "inset(0 0% 0 0)" : "inset(0 100% 0 0)",
            }}
            transition={{ ease: [0.19, 1, 0.22, 1], duration: 0.5 }}
          />
        </div>
      </Link>
    );
  }

  if (variant === "wavy") {
    return (
      <Link
        href={href}
        className={cn(
          "relative inline-block text-xl font-medium text-neutral-600 transition-colors duration-300 hover:text-neutral-900 dark:text-neutral-400 dark:hover:text-white",
          className,
        )}
        onMouseEnter={() => setIsHovered(true)}
        onMouseLeave={() => setIsHovered(false)}
        {...props}
      >
        <span className="relative z-10">{children}</span>
        <div
          className="absolute -bottom-2 left-0 h-2 w-full overflow-hidden opacity-0 transition-opacity duration-300"
          style={{ opacity: isHovered ? 1 : 0 }}
        >
          <motion.div
            className="flex h-full w-[200%] text-neutral-900 dark:text-white"
            initial={{ x: 0 }}
            animate={{ x: isHovered ? "-50%" : "0%" }}
            transition={{ repeat: Infinity, duration: 0.8, ease: "linear" }}
          >
            <svg
              width="100%"
              height="100%"
              viewBox="0 0 100 10"
              preserveAspectRatio="none"
            >
              <path
                d="M0 5 Q 12.5 0 25 5 T 50 5 T 75 5 T 100 5 M100 5 Q 112.5 0 125 5 T 150 5 T 175 5 T 200 5"
                stroke="currentColor"
                fill="transparent"
                strokeWidth="2"
              />
            </svg>
          </motion.div>
        </div>
      </Link>
    );
  }

  if (variant === "textRise") {
    return (
      <Link
        href={href}
        className={cn(
          "relative inline-flex overflow-hidden text-xl font-medium text-neutral-600 transition-colors duration-300 hover:text-neutral-900 dark:text-neutral-400 dark:hover:text-white",
          className,
        )}
        onMouseEnter={() => setIsHovered(true)}
        onMouseLeave={() => setIsHovered(false)}
        {...props}
      >
        <motion.span
          animate={{ y: isHovered ? "-100%" : "0%" }}
          transition={{ ease: [0.19, 1, 0.22, 1], duration: 0.4 }}
          className="relative inline-block"
        >
          {children}
        </motion.span>
        <motion.span
          className="absolute top-0 left-0 inline-block"
          initial={{ y: "100%" }}
          animate={{ y: isHovered ? "0%" : "100%" }}
          transition={{ ease: [0.19, 1, 0.22, 1], duration: 0.4 }}
          aria-hidden="true"
        >
          {children}
        </motion.span>
      </Link>
    );
  }

  return (
    <Link href={href} className={cn(resolvedVariant, className)} {...props}>
      <span className="inline-flex items-center gap-2">
        {children}
        {showArrow ? (
          <span
            aria-hidden="true"
            className="transition-transform duration-300 group-hover:translate-x-0.5"
          >
            →
          </span>
        ) : null}
      </span>
    </Link>
  );
}

export { variantStyles as animatedLinkVariants };

/**
 * Great UI Component
 *
 * Built with React, TypeScript, Tailwind CSS, and Framer Motion.
 * Designed to be accessible, customizable, and production-ready.
 *
 * Website: https://great-ui.com
 * GitHub: https://github.com/Saurabh-2607/GreatUI
 * X (Great UI): https://x.com/GreatUIHQ
 *
 * Released under the MIT License.
 * Contributions, issues, and feature requests are always welcome.
 *
 * Author: Saurabh Sharma
 * X: https://x.com/srbh_s
 */

demo.tsx
"use client";

import AnimatedLink, {
  type AnimatedLinkVariant,
} from "@/components/ui/great-ui-animated-link";

const previewVariants: {
  label: string;
  variant: AnimatedLinkVariant;
  showArrow?: boolean;
}[] = [
  { label: "Classic Underline", variant: "underline" },
  { label: "Center Underline", variant: "centerUnderline" },
  { label: "Top Overline", variant: "overline" },
  { label: "Vertical Borders", variant: "verticalLines" },
  { label: "Slide Reveal Line", variant: "revealLine" },
  { label: "Fade Up Line", variant: "fadeUpLine" },
  { label: "Dash Arrow Hover", variant: "dashHover" },
  { label: "Clip Fill Y", variant: "clipFillY" },
  { label: "Clip Fill X", variant: "clipFillX" },
  { label: "Clip Center", variant: "clipCenter" },
  { label: "Doodle Underline", variant: "clipDoodle" },
  { label: "Loop Wave", variant: "wavy" },
  { label: "Text Rise Marquee", variant: "textRise" },
];

export default function AnimatedLinkPreview() {
  return (
    <div className="w-full max-w-4xl px-8 py-12 select-none">
      <div className="grid grid-cols-2 gap-x-12 gap-y-12 sm:grid-cols-3 md:grid-cols-4">
        {previewVariants.map((item) => (
          <div key={item.label} className="flex h-8 items-center">
            <AnimatedLink
              href="#"
              variant={item.variant}
              showArrow={item.showArrow}
              className="text-xl font-semibold transition-colors"
            >
              {item.label}
            </AnimatedLink>
          </div>
        ))}
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion motion
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
