<!-- Testimonial Card · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/testimonial-card
     license: MIT · category: testimonials
     A responsive testimonial card component with rating stars and author information. -->

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
import { Testimonial } from "@/components/ui/testimonial-card"

const testimonials = [
  {
    name: "Sarah Johnson",
    role: "Product Manager",
    company: "Amazun",
    rating: 5,
    image: "https://cdn.21st.dev/assets/mirror/a3/a37358a09c532474e3246a94e5e5104b1b6b6907f83b44259509f8c776506120.jpg",
    testimonial: "This library has completely transformed how we build our UI components. The attention to detail and smooth animations make our application stand out. Highly recommended!"
  },
  {
    name: "John Doe",
    role: "Software Engineer",
    company: "Goggle",
    rating: 4,
    image: "https://cdn.21st.dev/assets/mirror/c9/c9038553755a8c1c3753ccd0a14af4854745b83bd3a97609f81d2c0d48f35517.jpg",
    testimonial: "The components are well documented and easy to customize. The code quality is top-notch and the support is excellent. I'm very happy with my purchase."
  },
  {
    name: "Emily Chen",
    role: "UX Designer",
    company: "Microsift",
    rating: 5,
    image: "https://cdn.21st.dev/assets/mirror/db/db395a118244043772f21d6cd644901eaf89eab5a3dfcedcc73c255a4d6417e0.jpg",
    testimonial: "The accessibility features and design system consistency are impressive. It's saved us countless hours in development time."
  }
]

function TestimonialDemo() {
  return (
    <div className="container py-10">
      <div className="grid gap-4 grid-cols-1 md:grid-cols-2 lg:grid-cols-3">
        {testimonials.map((testimonial) => (
          <Testimonial key={testimonial.name} {...testimonial} />
        ))}
      </div>
    </div>
  )
}

export { TestimonialDemo }
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar
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
