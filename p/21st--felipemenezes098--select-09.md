<!-- Custom Border & Background Select · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/select-09
     license: MIT · category: form
     A select dropdown with a muted background and a custom subtle border on the trigger for a softer, filled input style. -->

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
components/ui/select-09.tsx
import {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select'

export function Select09() {
  return (
    <Select defaultValue="design">
      <SelectTrigger className="border-border/60 bg-muted hover:bg-muted/80 w-full max-w-56 border shadow-none">
        <SelectValue placeholder="Select a team" />
      </SelectTrigger>
      <SelectContent>
        <SelectGroup>
          <SelectItem value="design">Design</SelectItem>
          <SelectItem value="engineering">Engineering</SelectItem>
          <SelectItem value="marketing">Marketing</SelectItem>
          <SelectItem value="support">Support</SelectItem>
        </SelectGroup>
      </SelectContent>
    </Select>
  )
}

demo.tsx
import { Select09 } from "@/components/ui/select-09";

export default function DemoSelect09() {
  return (
    <div className="flex min-h-52 w-full items-center justify-center p-6">
      <Select09 />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add select
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
