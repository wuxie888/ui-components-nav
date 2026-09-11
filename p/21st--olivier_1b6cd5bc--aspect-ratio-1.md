<!-- Aspect Ratio · @olivier_1b6cd5bc · https://21st.dev/@olivier_1b6cd5bc/components/aspect-ratio-1
     license: unspecified · category: image
     Aspect Ratio -->

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
components/ui/aspect-ratio.tsx
import { cn } from "@/lib/utils";

interface AspectRatioProps extends React.ComponentProps<"div"> {
  ratio: number;
}

function AspectRatio({
  ratio,
  className,
  children,
  ...props
}: AspectRatioProps) {
  return (
    <div
      data-slot="aspect-ratio-wrapper"
      className="relative w-full"
      style={{ paddingBottom: `${(1 / ratio) * 100}%` }}
    >
      <div
        data-slot="aspect-ratio"
        className={cn("absolute inset-0", className)}
        {...props}
      >
        {children}
      </div>
    </div>
  );
}

export { AspectRatio };

demo.tsx
import AspectRatioDemo from "@/components/ui/aspect-ratio-1";

export default function DemoOne() {
  return <AspectRatioDemo />;
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add aspect-ratio
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
