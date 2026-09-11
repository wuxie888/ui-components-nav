<!-- Disabled Select · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/select-03
     license: agpl-3.0 · category: form
     A select dropdown rendered in a non-interactive disabled state, useful for showing a locked or read-only status field in a form. -->

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
components/ui/select-03.tsx
import {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select'

export function Select03() {
  return (
    <Select disabled>
      <SelectTrigger className="w-full max-w-56" disabled>
        <SelectValue placeholder="Select a status" />
      </SelectTrigger>
      <SelectContent>
        <SelectGroup>
          <SelectItem value="active">Active</SelectItem>
          <SelectItem value="draft">Draft</SelectItem>
          <SelectItem value="pending">Pending</SelectItem>
          <SelectItem value="archived">Archived</SelectItem>
          <SelectItem value="deleted">Deleted</SelectItem>
        </SelectGroup>
      </SelectContent>
    </Select>
  )
}

demo.tsx
import { Select03 } from "@/components/ui/select-03";

export default function DemoSelect03() {
  return (
    <div className="flex min-h-72 w-full items-center justify-center p-6">
      <Select03 />
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
