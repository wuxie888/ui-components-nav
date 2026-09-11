<!-- Pricing Card · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/card-09
     license: MIT · category: pricing-section
     A pricing card that highlights a plan with a Popular badge, monthly price, and a feature checklist with a call-to-action button. -->

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
components/ui/card-09.tsx
import { CheckIcon } from 'lucide-react'

import { Badge } from '@/components/ui/badge'
import { Button } from '@/components/ui/button'
import {
  Card,
  CardAction,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'

const features = [
  'Unlimited projects',
  'Advanced analytics',
  'Priority support',
  '50 GB storage',
]

export function Card09() {
  return (
    <Card className="ring-primary w-full max-w-xs ring-2">
      <CardHeader>
        <CardTitle>Pro</CardTitle>
        <CardDescription>For growing teams that ship fast.</CardDescription>
        <CardAction>
          <Badge>Popular</Badge>
        </CardAction>
      </CardHeader>
      <CardContent className="flex flex-col gap-4">
        <div className="flex items-baseline gap-1">
          <span className="text-3xl font-semibold tabular-nums">$29</span>
          <span className="text-muted-foreground text-sm">/ month</span>
        </div>
        <ul className="flex flex-col gap-2 text-sm">
          {features.map((feature) => (
            <li key={feature} className="flex items-center gap-2">
              <CheckIcon className="text-primary size-4 shrink-0" />
              {feature}
            </li>
          ))}
        </ul>
      </CardContent>
      <CardFooter>
        <Button className="w-full">Get started</Button>
      </CardFooter>
    </Card>
  )
}

demo.tsx
import { Card09 } from "@/components/ui/card-09";

export default function Card09Demo() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Card09 />
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
npx shadcn@latest add badge button card
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
