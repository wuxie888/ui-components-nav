<!-- Textarea With Helper Text · @shadcnspace · https://21st.dev/@shadcnspace/components/textarea-08
     license: MIT · category: form
     A textarea input paired with a label and a right-aligned helper text hint below it. -->

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
components/shadcn-space/textarea/textarea-08.tsx
import { Label } from '@/components/ui/label'
import { Textarea } from '@/components/ui/textarea'

const TextareaWithHelperTextRightDemo = () => {
  return (
    <div className='w-full max-w-xs space-y-2'>
      <Label htmlFor="message">Message</Label>
      <Textarea placeholder='Drop your message here !' id="message" />
      <p className='text-muted-foreground text-end text-xs'>Your feedback means a lot to us.</p>
    </div>
  )
}

export default TextareaWithHelperTextRightDemo

demo.tsx
import TextareaWithHelperText from "@/components/ui/textarea-08";

export default function Demo() {
  return (
    <div className="flex min-h-[200px] w-full items-center justify-center p-6">
      <TextareaWithHelperText />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add label textarea
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
