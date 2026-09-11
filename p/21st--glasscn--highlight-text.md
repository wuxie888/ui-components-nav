<!-- Highlight Text · @glasscn · https://21st.dev/@glasscn/components/highlight-text
     license: MIT · category: text
     Inline text highlight that draws a skewed colored background behind words, with preset color variants and custom background support. -->

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
components/ui/highlight-text.tsx
import { cn } from "@/lib/utils";

interface HighlightTextProps {
  children: React.ReactNode;
  className?: string;
  variant?: "lime" | "yellow" | "pink" | "cyan" | "orange";
}

const highlightVariants: Record<NonNullable<HighlightTextProps["variant"]>, string> = {
  lime: "bg-accent-green",
  yellow: "bg-yellow-300",
  pink: "bg-pink-300",
  cyan: "bg-cyan-300",
  orange: "bg-orange-300",
};

export function HighlightText({
  children,
  className,
  variant = "lime",
}: HighlightTextProps) {
  return (
    <span className="relative inline-block">
      <span
        className={cn(
          "absolute inset-0 scale-x-110 scale-y-90 -skew-y-1",
          highlightVariants[variant],
          className
        )}
        aria-hidden="true"
      />
      <span className="relative">{children}</span>
    </span>
  );
}

demo.tsx
import { HighlightText } from "@/components/ui/highlight-text";

export default function HighlightTextDemo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center p-8">
      <p className="max-w-md text-center text-2xl font-semibold leading-relaxed text-foreground">
        Build interfaces that feel{" "}
        <HighlightText>effortless</HighlightText> and stay{" "}
        <HighlightText variant="yellow">delightful</HighlightText> for every{" "}
        <HighlightText variant="pink">user</HighlightText>.
      </p>
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add utils
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
