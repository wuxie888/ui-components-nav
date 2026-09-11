<!-- Avatar Group Empty State · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/empty-04
     license: no-license · category: team
     A bordered empty-state card with an avatar stack and an invite button, used to prompt inviting team members to an empty workspace. -->

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
components/ui/empty-04.tsx
import { UserPlusIcon } from 'lucide-react'

import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar'
import { Button } from '@/components/ui/button'
import {
  Empty,
  EmptyContent,
  EmptyDescription,
  EmptyHeader,
  EmptyTitle,
} from '@/components/ui/empty'

export function Empty04() {
  return (
    <Empty className="bg-muted/30 w-full max-w-md rounded-xl border border-dashed">
      <EmptyHeader>
        <div className="*:data-[slot=avatar]:ring-background mb-2 flex -space-x-2 *:data-[slot=avatar]:grayscale *:data-[slot=avatar]:ring-2">
          <Avatar>
            <AvatarImage src="https://github.com/shadcn.png" alt="@shadcn" />
            <AvatarFallback>CN</AvatarFallback>
          </Avatar>
          <Avatar>
            <AvatarImage
              src="https://github.com/maxleiter.png"
              alt="@maxleiter"
            />
            <AvatarFallback>LR</AvatarFallback>
          </Avatar>
          <Avatar>
            <AvatarImage
              src="https://github.com/evilrabbit.png"
              alt="@evilrabbit"
            />
            <AvatarFallback>ER</AvatarFallback>
          </Avatar>
        </div>
        <EmptyTitle>No team members</EmptyTitle>
        <EmptyDescription>
          Invite people to collaborate on this workspace.
        </EmptyDescription>
      </EmptyHeader>
      <EmptyContent>
        <Button size="sm">
          <UserPlusIcon data-icon="inline-start" />
          Invite people
        </Button>
      </EmptyContent>
    </Empty>
  )
}

demo.tsx
import Empty04 from "@/components/ui/empty-04";

export default function Default() {
  return (
    <div className="flex min-h-[440px] w-full items-center justify-center bg-background p-8">
      <Empty04 />
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
npx shadcn@latest add avatar button empty
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
