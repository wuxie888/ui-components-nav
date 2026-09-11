<!-- Animated Link · @ruixen.ui · https://21st.dev/@ruixen.ui/components/animated-link
     license: unspecified · category: text
     React animated link: Hover reveals an underline that wipes in from the left, the right, or grows from the center, while a diagonal arrow lifts into view. -->

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
components/ui/animated-link.tsx
import * as React from "react";

import { cn } from "@/lib/utils";

type AnimatedLinkVariant = "left" | "right" | "center";

const underlineVariants: Record<AnimatedLinkVariant, string> = {
  // underline wipes in from the left edge
  left: "before:origin-right before:scale-x-0 hover:before:origin-left hover:before:scale-x-100",
  // underline wipes in from the right edge
  right:
    "before:origin-left before:scale-x-0 hover:before:origin-right hover:before:scale-x-100",
  // underline grows outward from the center
  center: "before:origin-center before:scale-x-0 hover:before:scale-x-100",
};

export interface AnimatedLinkProps extends React.ComponentPropsWithoutRef<"a"> {
  /** Direction the underline reveals from on hover. */
  variant?: AnimatedLinkVariant;
  /** Show the diagonal arrow that lifts in on hover. */
  showArrow?: boolean;
}

const AnimatedLink = ({
  variant = "left",
  showArrow = true,
  className,
  children,
  ...props
}: AnimatedLinkProps) => {
  return (
    <a
      className={cn(
        "group relative inline-flex w-fit items-center text-foreground",
        "before:pointer-events-none before:absolute before:left-0 before:top-[1.5em] before:h-[0.05em] before:w-full before:bg-current before:content-['']",
        "before:transition-transform before:duration-300 before:ease-[cubic-bezier(0.4,0,0.2,1)] motion-reduce:before:transition-none",
        underlineVariants[variant],
        className,
      )}
      {...props}
    >
      {children}
      {showArrow && (
        <svg
          className="ml-[0.3em] size-[0.55em] transition-none"
          fill="none"
          viewBox="0 0 10 10"
          xmlns="http://www.w3.org/2000/svg"
          aria-hidden="true"
        >
          <path
            d="M1.004 9.166 9.337.833m0 0v8.333m0-8.333H1.004"
            stroke="currentColor"
            strokeWidth="1.25"
            strokeLinecap="round"
            strokeLinejoin="round"
            className="[stroke-dasharray:32] [stroke-dashoffset:32] transition-[stroke-dashoffset] duration-300 ease-[cubic-bezier(0.4,0,0.2,1)] group-hover:[stroke-dashoffset:0] motion-reduce:transition-none"
          />
        </svg>
      )}
    </a>
  );
};

export { AnimatedLink };
export default AnimatedLink;

demo.tsx
import { AnimatedLink } from "@/components/ui/animated-link";

export default function DemoOne() {
return (
    <div className="flex h-full w-full flex-col items-center justify-center gap-8 py-16 text-lg">
      {/* underline reveals from the left */}
      <AnimatedLink href="#" variant="left">
        Hover over me
      </AnimatedLink>
 
      {/* underline reveals from the right */}
      <AnimatedLink href="#" variant="right">
        Hover over me
      </AnimatedLink>
 
      {/* underline grows from the center */}
      <AnimatedLink href="#" variant="center">
        Hover over me
      </AnimatedLink>
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
