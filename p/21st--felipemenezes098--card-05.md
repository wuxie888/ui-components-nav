<!-- Stat Card · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/card-05
     license: MIT · category: stat
     A compact KPI card showing a metric value with an icon and a tinted trend badge comparing to a previous period. -->

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
components/ui/card-05.tsx
import { DollarSignIcon, TrendingUpIcon } from 'lucide-react'

import { Badge } from '@/components/ui/badge'
import {
  Card,
  CardAction,
  CardDescription,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'

export function Card05() {
  return (
    <Card className="w-full max-w-xs">
      <CardHeader>
        <CardDescription>Total revenue</CardDescription>
        <CardTitle className="text-2xl tabular-nums">$48,231.89</CardTitle>
        <CardAction>
          <div className="bg-muted flex size-9 items-center justify-center rounded-lg">
            <DollarSignIcon className="text-muted-foreground size-4" />
          </div>
        </CardAction>
      </CardHeader>
      <CardDescription className="flex items-center gap-2 px-6">
        <Badge variant="secondary" className="text-emerald-600 dark:text-emerald-400">
          <TrendingUpIcon className="size-3" />
          +20.1%
        </Badge>
        vs. last month
      </CardDescription>
    </Card>
  )
}

demo.tsx
import Card05 from "@/components/ui/card-05";

export default function DemoCard05() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center p-6">
      <Card05 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge card
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
