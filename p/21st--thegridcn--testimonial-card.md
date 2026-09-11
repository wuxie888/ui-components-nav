<!-- Testimonial Card · @thegridcn · https://21st.dev/@thegridcn/components/testimonial-card
     license: MIT · category: testimonials
     A sci-fi Tron-styled testimonial card with a quote, author, avatar or initials, star rating, scanline overlay and corner brackets. -->

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
components/thegridcn/testimonial-card.tsx
"use client";

import type * as React from "react";
import { cn } from "@/lib/utils";

interface TestimonialCardProps extends React.HTMLAttributes<HTMLDivElement> {
  author: string;
  avatar?: string;
  quote: string;
  rating?: number;
  role?: string;
}

export function TestimonialCard({
  quote,
  author,
  role,
  avatar,
  rating,
  className,
  ...props
}: TestimonialCardProps) {
  const initials = author
    .split(" ")
    .map((w) => w[0])
    .join("")
    .toUpperCase()
    .slice(0, 2);

  return (
    <div
      data-slot="tron-testimonial-card"
      className={cn(
        "relative overflow-hidden rounded border border-primary/20 bg-card/80 p-5 backdrop-blur-sm",
        className
      )}
      {...props}
    >
      {/* Scanline overlay */}
      <div className="pointer-events-none absolute inset-0 bg-[repeating-linear-gradient(0deg,transparent,transparent_2px,rgba(0,0,0,0.03)_2px,rgba(0,0,0,0.03)_4px)]" />

      {/* Quote mark */}
      <div className="mb-3 font-display text-2xl text-primary/30 leading-none">
        &ldquo;
      </div>

      {/* Quote text */}
      <p className="text-foreground/80 text-sm leading-relaxed">{quote}</p>

      {/* Rating */}
      {rating !== undefined && (
        <div className="mt-3 flex gap-0.5">
          {Array.from({ length: 5 }, (_, i) => (
            <span
              key={i}
              className={cn(
                "text-xs",
                i < rating ? "text-primary" : "text-foreground/15"
              )}
            >
              ◆
            </span>
          ))}
        </div>
      )}

      {/* Author */}
      <div className="mt-4 flex items-center gap-3 border-border/30 border-t pt-4">
        <div className="flex h-9 w-9 shrink-0 items-center justify-center overflow-hidden rounded-full border border-primary/30 bg-primary/10">
          {avatar ? (
            <img
              src={avatar}
              alt={author}
              className="h-full w-full object-cover"
            />
          ) : (
            <span className="font-bold font-mono text-[10px] text-primary">
              {initials}
            </span>
          )}
        </div>
        <div>
          <div className="font-bold text-foreground text-xs uppercase tracking-wider">
            {author}
          </div>
          {role ? (
            <div className="text-[10px] text-foreground/40 uppercase tracking-widest">
              {role}
            </div>
          ) : null}
        </div>
      </div>

      {/* Corner decorations */}
      <div className="pointer-events-none absolute top-0 left-0 h-3 w-3 border-primary/30 border-t-2 border-l-2" />
      <div className="pointer-events-none absolute top-0 right-0 h-3 w-3 border-primary/30 border-t-2 border-r-2" />
      <div className="pointer-events-none absolute bottom-0 left-0 h-3 w-3 border-primary/30 border-b-2 border-l-2" />
      <div className="pointer-events-none absolute right-0 bottom-0 h-3 w-3 border-primary/30 border-r-2 border-b-2" />
    </div>
  );
}

demo.tsx
import { TestimonialCard } from "@/components/ui/testimonial-card";

export default function Default() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center bg-background p-8">
      <TestimonialCard
        className="max-w-sm"
        quote="The GridCN turned our dashboard into a command center. Every interaction feels like piloting a lightcycle across the grid."
        author="Kevin Flynn"
        role="System Architect"
        rating={5}
      />
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
