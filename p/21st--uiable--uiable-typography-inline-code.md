<!-- Inline Code Typography · @uiable · https://21st.dev/@uiable/components/uiable-typography-inline-code
     license: MIT · category: text
     Inline code snippet styled with a monospace font and muted background for use within body text. -->

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
components/uiable/typography/typography-inline-code.tsx
//  ------------------------------ | TYPOGRAPHY - INLINE CODE | ------------------------------  //

export function TypographyInlineCode() {
  return (
    <code className="relative rounded bg-muted px-[0.3rem] py-[0.2rem] font-mono text-sm font-semibold">
      @radix-ui/react-alert-dialog
    </code>
  )
}

demo.tsx
import { TypographyInlineCode } from "@/components/ui/uiable-typography-inline-code";

export default function Default() {
  return (
    <div className="flex min-h-[200px] w-full items-center justify-center bg-background p-8">
      <p className="max-w-md text-center text-base text-foreground">
        Install the package by running <TypographyInlineCode /> in your
        terminal.
      </p>
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
