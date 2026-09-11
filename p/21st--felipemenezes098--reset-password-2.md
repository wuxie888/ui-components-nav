<!-- Reset Password OTP Code · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/reset-password-2
     license: MIT · category: card
     A card with a six-digit OTP input for verifying a password-reset code sent by email. -->

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
components/ui/reset-password-2.tsx
'use client'

import { useState } from 'react'
import { ShieldCheckIcon } from 'lucide-react'

import { Button } from '@/components/ui/button'
import {
  Card,
  CardContent,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'
import {
  InputOTP,
  InputOTPGroup,
  InputOTPSlot,
} from '@/components/ui/input-otp'

export function ResetPassword2() {
  const [code, setCode] = useState('')

  return (
    <Card size="sm">
      <CardHeader>
        <div className="bg-primary/10 mb-1 flex size-10 items-center justify-center rounded-full">
          <ShieldCheckIcon className="text-primary size-5" />
        </div>
        <CardTitle>Enter your code</CardTitle>
        <p className="text-muted-foreground text-xs">
          We sent a 6-digit code to{' '}
          <span className="text-foreground font-medium">j•••@acme.com</span>.
          Enter it below to continue.
        </p>
      </CardHeader>

      <CardContent className="flex justify-center">
        <InputOTP maxLength={6} value={code} onChange={setCode}>
          <InputOTPGroup>
            <InputOTPSlot index={0} />
            <InputOTPSlot index={1} />
            <InputOTPSlot index={2} />
            <InputOTPSlot index={3} />
            <InputOTPSlot index={4} />
            <InputOTPSlot index={5} />
          </InputOTPGroup>
        </InputOTP>
      </CardContent>

      <CardFooter className="flex-col items-stretch gap-4">
        <Button className="w-full" disabled={code.length < 6}>
          Verify code
        </Button>
        <p className="text-muted-foreground text-center text-[11px]">
          Didn&apos;t get it?{' '}
          <a href="#" className="text-foreground font-medium hover:underline">
            Resend code
          </a>{' '}
          · expires in 5:00
        </p>
      </CardFooter>
    </Card>
  )
}

demo.tsx
import { ResetPassword2 } from "@/components/ui/reset-password-2";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <ResetPassword2 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install input-otp lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button card input-otp
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
