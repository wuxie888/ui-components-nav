<!-- SMS Code Two-Factor Setup · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/two-factor-setup-2
     license: agpl-3.0 · category: form
     A compact card for enabling SMS-based two-factor authentication by collecting a phone number to send verification codes to. -->

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
components/ui/two-factor-setup-2.tsx
import { SmartphoneIcon } from 'lucide-react'

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

export function TwoFactorSetup2() {
  return (
    <Card size="sm">
      <CardHeader>
        <div className="bg-primary/10 mb-1 flex size-10 items-center justify-center rounded-full">
          <SmartphoneIcon className="text-primary size-5" />
        </div>
        <CardTitle>Verify by SMS</CardTitle>
        <p className="text-muted-foreground text-xs">
          We&apos;ll text a code to your phone each time you sign in. Add the
          number to protect.
        </p>
      </CardHeader>

      <CardContent className="flex flex-col gap-1.5">
        <Label htmlFor="tf-phone" className="text-xs">
          Phone number
        </Label>
        <Input
          id="tf-phone"
          type="tel"
          placeholder="+1 (555) 000-0000"
          autoComplete="tel"
        />
      </CardContent>

      <CardFooter className="flex-col items-stretch gap-4">
        <Button className="w-full">Send code</Button>
        <p className="text-muted-foreground/80 text-center text-[11px]">
          Carrier rates may apply. SMS is less secure than an app.
        </p>
      </CardFooter>
    </Card>
  )
}

demo.tsx
import { TwoFactorSetup2 } from '@/components/ui/two-factor-setup-2'

export default function Default() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center bg-background p-10">
      <TwoFactorSetup2 />
    </div>
  )
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
