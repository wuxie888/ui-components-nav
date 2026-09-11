<!-- Select with Disabled Items · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/select-07
     license: agpl-3.0 · category: form
     A select dropdown where individual options can be marked as disabled so users cannot choose them. -->

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
components/ui/select-07.tsx
import {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select'

export function Select07() {
  return (
    <Select defaultValue="pro">
      <SelectTrigger className="w-full max-w-56">
        <SelectValue placeholder="Select a plan" />
      </SelectTrigger>
      <SelectContent>
        <SelectGroup>
          <SelectItem value="free">Free</SelectItem>
          <SelectItem value="pro">Pro</SelectItem>
          <SelectItem value="enterprise" disabled>
            Enterprise
          </SelectItem>
          <SelectItem value="custom" disabled>
            Custom (contact sales)
          </SelectItem>
        </SelectGroup>
      </SelectContent>
    </Select>
  )
}

demo.tsx
import { Select07 } from "@/components/ui/select-07";

export default function Default() {
  return (
    <div className="flex min-h-52 w-full items-center justify-center p-6">
      <Select07 />
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
