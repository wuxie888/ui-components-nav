<!-- Reset Password Email Link · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/reset-password-1
     license: no-license · category: form
     A password reset card with an email input and a button that sends a reset link, plus a link back to sign in. -->

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
components/ui/reset-password-1.tsx
import { KeyRoundIcon } from 'lucide-react'

import { Button } from '@/components/ui/button'
import {
  Card,
  CardContent,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'
import { Input } from '@/components/ui/input'
import { Label } from '@/components/ui/label'

export function ResetPassword1() {
  return (
    <Card size="sm">
      <CardHeader>
        <div className="bg-primary/10 mb-1 flex size-10 items-center justify-center rounded-full">
          <KeyRoundIcon className="text-primary size-5" />
        </div>
        <CardTitle>Reset your password</CardTitle>
        <p className="text-muted-foreground text-xs">
          Enter your account email and we&apos;ll send you a link to choose a
          new password.
        </p>
      </CardHeader>

      <CardContent className="flex flex-col gap-1.5">
        <Label htmlFor="rp-email" className="text-xs">
          Email
        </Label>
        <Input
          id="rp-email"
          type="email"
          placeholder="you@acme.com"
          autoComplete="email"
        />
      </CardContent>

      <CardFooter className="flex-col items-stretch gap-5">
        <Button className="w-full">Send reset link</Button>
        <p className="text-muted-foreground text-center text-xs">
          Remembered it?{' '}
          <a href="#" className="text-foreground font-medium hover:underline">
            Back to sign in
          </a>
        </p>
      </CardFooter>
    </Card>
  )
}

demo.tsx
import { ResetPassword1 } from "@/components/ui/reset-password-1";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <ResetPassword1 />
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
npx shadcn@latest add button card input label
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
