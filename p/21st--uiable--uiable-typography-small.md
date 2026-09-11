<!-- Small Typography · @uiable · https://21st.dev/@uiable/components/uiable-typography-small
     license: no-license · category: form
     A small typography element for rendering muted helper text, form field labels, and fine print. -->

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
components/uiable/typography/typography-small.tsx
//  ------------------------------ | TYPOGRAPHY - SMALL | ------------------------------  //

export function TypographySmall() {
  return <small className="text-sm">Email address</small>
}

demo.tsx
import { TypographySmall } from "@/components/ui/uiable-typography-small";

export default function Default() {
  return (
    <div className="flex min-h-[350px] w-full items-center justify-center bg-background p-8">
      <div className="w-full max-w-sm space-y-2">
        <TypographySmall />
        <input
          type="email"
          placeholder="name@example.com"
          className="w-full rounded-md border border-input bg-background px-3 py-2 text-sm text-foreground shadow-sm outline-none focus:ring-2 focus:ring-ring"
        />
        <p className="text-xs text-muted-foreground">
          We&apos;ll never share your email with anyone else.
        </p>
      </div>
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
