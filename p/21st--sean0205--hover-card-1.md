<!-- Hover Card · @sean0205 · https://21st.dev/@sean0205/components/hover-card-1
     license: MIT · category: profile
     For sighted users to preview content available behind a link. -->

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
components/ui/c-hover-card-1.tsx
import { Button } from "@/components/ui/button"
import {
  HoverCard,
  HoverCardContent,
  HoverCardTrigger,
} from "@/components/ui/hover-card"

export function Pattern() {
  return (
    <div className="flex min-h-[100px] items-center justify-center">
      <HoverCard>
        <HoverCardTrigger
          delay={100}
          closeDelay={100}
          render={<Button variant="outline">Hover Me</Button>}
        />
        <HoverCardContent>
          <div className="flex flex-col gap-1">
            <h4 className="leading-none font-medium">Hover Card</h4>
            <p className="text-muted-foreground">
              A basic hover card that appears when you hover over the trigger.
            </p>
          </div>
        </HoverCardContent>
      </HoverCard>
    </div>
  )
}

demo.tsx
import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar';
import { Button } from '@/components/ui/button-1';
import { HoverCard, HoverCardContent, HoverCardTrigger } from '@/components/ui/hover-card-1';

export default function Component() {
  return (
    <HoverCard>
      <HoverCardTrigger asChild>
        <Button mode="link">@reui_io</Button>
      </HoverCardTrigger>
      <HoverCardContent className="w-80">
        <div className="flex justify-between gap-4">
          <Avatar>
            <AvatarImage src="https://cdn.21st.dev/assets/mirror/82/827b9c7667e33f2b0012dfa2d4cc3851eec5aba531c436e83f67b602e5ca512b.png" />
            <AvatarFallback>RE</AvatarFallback>
          </Avatar>
          <div className="space-y-1">
            <h4 className="text-sm font-semibold">@reui_io</h4>
            <p className="text-sm">
              Open-source collection of UI components and animated effects built with React, Typescript, Tailwind CSS,
              and Motion.
            </p>
          </div>
        </div>
      </HoverCardContent>
    </HoverCard>
  );
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-hover-card
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar button button-1 hover-card
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
