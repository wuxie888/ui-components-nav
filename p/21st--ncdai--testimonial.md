<!-- Testimonial · @ncdai · https://21st.dev/@ncdai/components/testimonial
     license: MIT · category: testimonials
     A composable testimonial card for displaying user feedback with a quote, author name, avatar with ring, tagline, and verified badge. -->

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
components/testimonial/testimonial.tsx
import type { ComponentProps } from "react"

import { cn } from "@/lib/utils"

export function Testimonial({ className, ...props }: ComponentProps<"figure">) {
  return (
    <figure
      data-slot="testimonial"
      className={cn("flex h-full flex-col", className)}
      {...props}
    />
  )
}

export function TestimonialQuote({
  className,
  ...props
}: ComponentProps<"blockquote">) {
  return (
    <blockquote
      data-slot="testimonial-quote"
      className={cn(
        "grow px-4 py-3 text-base text-pretty text-foreground",
        className
      )}
      {...props}
    />
  )
}

export function TestimonialAuthor({
  className,
  ...props
}: ComponentProps<"figcaption">) {
  return (
    <figcaption
      data-slot="testimonial-author"
      className={cn(
        "grid grid-cols-[auto_1fr] grid-rows-2 items-center gap-x-3.5 px-4 pt-1 pb-3",
        className
      )}
      {...props}
    />
  )
}

export function TestimonialAvatar({
  className,
  ...props
}: ComponentProps<"div">) {
  return (
    <div
      data-slot="testimonial-avatar"
      className={cn("relative row-span-2 size-8 shrink-0", className)}
      {...props}
    />
  )
}

export function TestimonialAvatarImg({
  className,
  src,
  alt,
  ...props
}: ComponentProps<"img">) {
  return (
    <img
      data-slot="testimonial-avatar-img"
      className={cn("size-8 rounded-full select-none", className)}
      src={src}
      alt={alt}
      {...props}
    />
  )
}

export function TestimonialAvatarRing({
  className,
  ...props
}: ComponentProps<"div">) {
  return (
    <div
      data-slot="testimonial-avatar-ring"
      className={cn(
        "pointer-events-none absolute inset-0 rounded-full inset-ring-1 inset-ring-black/10 dark:inset-ring-white/15",
        className
      )}
      {...props}
    />
  )
}

export function TestimonialAuthorName({
  className,
  ...props
}: ComponentProps<"div">) {
  return (
    <div
      data-slot="testimonial-author-name"
      className={cn(
        "flex items-center gap-1.5 text-sm leading-4.5 font-semibold text-foreground",
        className
      )}
      {...props}
    />
  )
}

export function TestimonialAuthorTagline({
  className,
  ...props
}: ComponentProps<"div">) {
  return (
    <div
      data-slot="testimonial-author-tagline"
      className={cn(
        "text-xs leading-4 text-balance text-muted-foreground",
        className
      )}
      {...props}
    />
  )
}

export function TestimonialVerifiedBadge({
  className,
  ...props
}: ComponentProps<"span">) {
  return (
    <span
      data-slot="testimonial-verified-badge"
      className={cn(
        "flex [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-3.5",
        className
      )}
      aria-hidden
      {...props}
    />
  )
}

demo.tsx
import {
  Testimonial,
  TestimonialAuthor,
  TestimonialAuthorName,
  TestimonialAuthorTagline,
  TestimonialAvatar,
  TestimonialAvatarImg,
  TestimonialAvatarRing,
  TestimonialQuote,
  TestimonialVerifiedBadge,
} from "@/components/ui/testimonial";

export default function TestimonialDemo() {
  return (
    <a
      className="block w-80 max-w-full rounded-xl inset-ring-1 inset-ring-foreground/10 transition-[background-color] ease-out hover:bg-accent/50"
      href="https://x.com/rauchg/status/1978913158514237669"
      target="_blank"
      rel="noopener noreferrer"
    >
      <Testimonial>
        <TestimonialQuote className="font-serif">
          <p>
            awesome. Love the components, especially slide-to-unlock. Great job
          </p>
        </TestimonialQuote>

        <TestimonialAuthor>
          <TestimonialAvatar>
            <TestimonialAvatarImg
              src="https://unavatar.io/x/rauchg"
              alt="Guillermo Rauch"
            />
            <TestimonialAvatarRing />
          </TestimonialAvatar>

          <TestimonialAuthorName>
            Guillermo Rauch
            <TestimonialVerifiedBadge className="text-info">
              <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
                <path
                  fill="currentColor"
                  d="M24 12a4.454 4.454 0 0 0-2.564-3.91 4.437 4.437 0 0 0-.948-4.578 4.436 4.436 0 0 0-4.577-.948A4.44 4.44 0 0 0 12 0a4.423 4.423 0 0 0-3.9 2.564 4.434 4.434 0 0 0-2.43-.178 4.425 4.425 0 0 0-2.158 1.126 4.42 4.42 0 0 0-1.12 2.156 4.42 4.42 0 0 0 .183 2.421A4.456 4.456 0 0 0 0 12a4.465 4.465 0 0 0 2.576 3.91 4.433 4.433 0 0 0 .936 4.577 4.459 4.459 0 0 0 4.577.95A4.454 4.454 0 0 0 12 24a4.439 4.439 0 0 0 3.91-2.563 4.26 4.26 0 0 0 5.526-5.526A4.453 4.453 0 0 0 24 12Zm-13.709 4.917-4.38-4.378 1.652-1.663 2.646 2.646L15.83 7.4l1.72 1.591-7.258 7.926Z"
                />
              </svg>
            </TestimonialVerifiedBadge>
          </TestimonialAuthorName>
          <TestimonialAuthorTagline>CEO @Vercel</TestimonialAuthorTagline>
        </TestimonialAuthor>
      </Testimonial>
    </a>
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
