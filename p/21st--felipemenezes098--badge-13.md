<!-- Tag List · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/badge-13
     license: MIT · category: badge
     A wrapping group of outline badges used to display a list of tags or keywords. -->

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
components/ui/badge-13.tsx
import { Badge } from '@/components/ui/badge'

const tags = [
  'react',
  'typescript',
  'tailwind',
  'next.js',
  'shadcn/ui',
  'motion',
  'radix',
]

export function Badge13() {
  return (
    <div className="flex max-w-xs flex-wrap gap-1.5">
      {tags.map((tag) => (
        <Badge key={tag} variant="outline" className="font-normal">
          {tag}
        </Badge>
      ))}
    </div>
  )
}

demo.tsx
import { Badge13 } from "@/components/ui/badge-13";

export default function Demo() {
  return (
    <div className="flex min-h-52 w-full items-center justify-center p-6">
      <Badge13 />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge
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
