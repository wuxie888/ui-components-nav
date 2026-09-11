<!-- Push Approval Card · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/verify-identity-3
     license: no-license · category: sign-in
     A push-notification approval card that prompts the user to confirm a sign-in request from their trusted device while waiting for approval. -->

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
components/ui/verify-identity-3.tsx
import { Loader2, Smartphone } from 'lucide-react'

import { Button } from '@/components/ui/button'
import {
  Card,
  CardContent,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'

export function VerifyIdentity3() {
  return (
    <Card size="sm">
      <CardHeader className="justify-items-center text-center">
        <div className="border-primary/20 relative mb-2 flex size-16 items-center justify-center rounded-2xl border">
          <Smartphone className="text-primary size-7" />
          <span className="border-background bg-primary absolute -right-1 -bottom-1 flex size-5 items-center justify-center rounded-full border-2">
            <Loader2 className="text-primary-foreground size-3 animate-spin" />
          </span>
        </div>
        <CardTitle>Check your phone</CardTitle>
        <p className="text-muted-foreground text-xs text-balance">
          We sent an approval request to{' '}
          <span className="text-foreground">iPhone 15 Pro</span>. Tap{' '}
          <span className="text-foreground">Approve</span> to continue.
        </p>
      </CardHeader>

      <CardContent className="flex flex-col gap-3">
        <div className="bg-muted/50 flex items-center justify-center gap-2 rounded-lg border p-3">
          <span className="bg-primary size-1.5 animate-pulse rounded-full" />
          <span className="text-muted-foreground text-xs">
            Waiting for approval…
          </span>
        </div>
        <Button variant="outline" className="w-full">
          Resend request
        </Button>
      </CardContent>

      <CardFooter className="justify-center">
        <a
          href="#"
          className="text-muted-foreground hover:text-foreground text-[11px]"
        >
          Verify another way
        </a>
      </CardFooter>
    </Card>
  )
}

demo.tsx
import { VerifyIdentity3 } from "@/components/ui/verify-identity-3";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center bg-background p-6">
      <VerifyIdentity3 />
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
npx shadcn@latest add button card
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
