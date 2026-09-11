<!-- Textarea · @sean0205 · https://21st.dev/@sean0205/components/textarea
     license: MIT · category: textarea
     A multi-line text input field with support for custom styling and states. -->

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
components/ui/c-textarea-1.tsx
import { Textarea } from "@/components/ui/textarea"

export function Pattern() {
  return (
    <div className="mx-auto w-full max-w-xs">
      <Textarea placeholder="Type your message here…" className="w-full" />
    </div>
  )
}

demo.tsx
import { Textarea } from '@/components/ui/textarea';

export default function TextareaDemo() {
  return (
    <div className="w-96">
      <Textarea placeholder="Type your message here." />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add textarea
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
