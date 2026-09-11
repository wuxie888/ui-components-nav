<!-- Warning Border Spinner · @uiable · https://21st.dev/@uiable/components/uiable-spinner-border-warning
     license: MIT · category: spinner
     A circular loading spinner with a yellow warning-colored border that rotates to indicate a pending state. -->

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
components/uiable/spinner/spinner-border-warning.tsx
//  ------------------------------ | SPINNER - BORDER WARNING | ------------------------------  //

export default function SpinnerBorderWarning() {
  return (
    <div
      className="inline-block size-8 animate-spin rounded-full border-[3.5px] border-yellow-500 border-l-transparent dark:border-l-transparent"
      role="status"
    >
      <span className="sr-only">Loading...</span>
    </div>
  )
}

demo.tsx
import SpinnerBorderWarning from "@/components/ui/uiable-spinner-border-warning";

export default function Default() {
  return (
    <div className="flex min-h-[240px] w-full items-center justify-center bg-background text-foreground">
      <SpinnerBorderWarning />
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
