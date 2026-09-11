<!-- Testimonial 2 · @ncdai · https://21st.dev/@ncdai/components/testimonial-2
     license: MIT · category: testimonials
     A testimonial quote block with a serif pull-quote, author name, tagline, and a link to the source. -->

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
components/testimonial-2/testimonial-2.tsx
import { cn } from "@/lib/utils"

export type Testimonial2Props = {
  /** Full display name of the person giving the testimonial. */
  authorName: string
  /** Short tagline, title, or description of the person. */
  authorTagline: string
  /** Link to the person's profile, website, or social media page. */
  url: string
  /** The testimonial text content or recommendation message. */
  quote: string
  /** Additional CSS classes to apply to the testimonial container. */
  className?: string
}

export function Testimonial2({
  className,
  authorName,
  authorTagline,
  url,
  quote,
}: Testimonial2Props) {
  return (
    <figure className={cn("relative flex flex-col gap-6 pl-3", className)}>
      <blockquote className="relative block w-full font-serif text-xl text-foreground md:w-lg md:text-2xl">
        <span
          className="absolute -left-3 text-muted-foreground select-none"
          aria-hidden="true"
        >
          “
        </span>
        <p className="inline text-pretty">{quote}</p>
        <span
          className="absolute translate-x-0.5 text-muted-foreground select-none"
          aria-hidden="true"
        >
          ”
        </span>
      </blockquote>

      <figcaption className="ml-auto flex w-full items-center gap-2 md:w-1/2">
        <div className="hidden h-px grow translate-y-px bg-border md:block" />

        <div className="flex flex-col md:ml-auto md:text-right">
          <span className="text-sm leading-none font-medium">
            <a href={url} target="_blank" rel="noopener">
              <span className="absolute inset-0" aria-hidden />
              {authorName}
            </a>
            <span className="mt-1 block text-muted-foreground md:mt-0 md:inline">
              <span className="hidden text-foreground md:inline">, </span>
              {authorTagline}
            </span>
          </span>
        </div>
      </figcaption>
    </figure>
  )
}

demo.tsx
import { Testimonial2 } from "@/components/ui/testimonial-2";

export default function Testimonial2Demo() {
  return (
    <div className="flex min-h-100 w-full items-center justify-center bg-background p-8">
      <Testimonial2
        authorName="Guillermo Rauch"
        authorTagline="CEO @Vercel"
        url="https://x.com/rauchg/status/1978913158514237669"
        quote="awesome. Love the components, especially slide-to-unlock. Great job"
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
