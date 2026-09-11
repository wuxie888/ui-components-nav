<!-- Spotlight Card · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/spotlight-card
     license: unspecified · category: features
     Create a spotlight effect on hover on a card component. -->

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
components/ui/card.tsx
import * as React from "react";

import { cn } from "@/lib/utils";

const Card = React.forwardRef<HTMLDivElement, React.ComponentProps<"div">>(
  function Card({ className, ...props }, ref) {
    return (
      <div
        className={cn(
          "flex touch-manipulation flex-col gap-6 rounded-xl border bg-card py-6 text-card-foreground shadow-sm",
          className
        )}
        data-slot="card"
        ref={ref}
        {...props}
      />
    );
  }
);

const CardHeader = React.forwardRef<
  HTMLDivElement,
  React.ComponentProps<"div">
>(function CardHeader({ className, ...props }, ref) {
  return (
    <div
      className={cn(
        "@container/card-header grid auto-rows-min grid-rows-[auto_auto] items-start gap-2 px-6 has-data-[slot=card-action]:grid-cols-[1fr_auto] [.border-b]:pb-6",
        className
      )}
      data-slot="card-header"
      ref={ref}
      {...props}
    />
  );
});

const CardTitle = React.forwardRef<HTMLDivElement, React.ComponentProps<"div">>(
  function CardTitle({ className, ...props }, ref) {
    return (
      <div
        className={cn(
          "font-semibold text-xl leading-none tracking-tight",
          className
        )}
        data-slot="card-title"
        ref={ref}
        {...props}
      />
    );
  }
);

const CardDescription = React.forwardRef<
  HTMLDivElement,
  React.ComponentProps<"div">
>(function CardDescription({ className, ...props }, ref) {
  return (
    <div
      className={cn("text-muted-foreground text-sm", className)}
      data-slot="card-description"
      ref={ref}
      {...props}
    />
  );
});

const CardAction = React.forwardRef<
  HTMLDivElement,
  React.ComponentProps<"div">
>(function CardAction({ className, ...props }, ref) {
  return (
    <div
      className={cn(
        "col-start-2 row-span-2 row-start-1 self-start justify-self-end",
        className
      )}
      data-slot="card-action"
      ref={ref}
      {...props}
    />
  );
});

const CardContent = React.forwardRef<
  HTMLDivElement,
  React.ComponentProps<"div">
>(function CardContent({ className, ...props }, ref) {
  return (
    <div
      className={cn("px-6", className)}
      data-slot="card-content"
      ref={ref}
      {...props}
    />
  );
});

const CardFooter = React.forwardRef<
  HTMLDivElement,
  React.ComponentProps<"div">
>(function CardFooter({ className, ...props }, ref) {
  return (
    <div
      className={cn("flex items-center px-6 [.border-t]:pt-6", className)}
      data-slot="card-footer"
      ref={ref}
      {...props}
    />
  );
});

export {
  Card,
  CardHeader,
  CardFooter,
  CardTitle,
  CardAction,
  CardDescription,
  CardContent,
};

demo.tsx
import { FaBolt } from "react-icons/fa";
import { SpotlightCard } from "@/components/ui/spotlight-card";

const DemoOne = () => {
  return (
    <div className="flex w-full h-screen justify-center items-center">
      <SpotlightCard
        className="magic-card flex flex-col gap-4 max-w-[30rem] rounded-4xl bg-white   border border-primary/10 shadow-2xl/10"
        spotlightColor="#ff006630"
      >
        <div className="text-2xl font-bold flex items-center gap-2">
          <FaBolt className="text-yellow-500" />
          <span>Lighting Fast</span>
        </div>
        <div className="text-muted-foreground">
          Optimized for performance with minimal bundle size. Build fast, responsive
          websites without compromise.
        </div>
      </SpotlightCard>
    </div>
  );
};

export { DemoOne };
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
