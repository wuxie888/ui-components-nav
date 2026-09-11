<!-- Payment Card Form · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/payment-1
     license: no-license · category: form
     A payment details card form with name, card number, expiry and CVC fields plus a secure-checkout note, for collecting credit card information. -->

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
components/ui/payment-1.tsx
'use client'

import { useState } from 'react'
import { CreditCard, Lock } from 'lucide-react'

import { Button } from '@/components/ui/button'
import {
  Card,
  CardContent,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'
import { Input } from '@/components/ui/input'
import {
  InputGroup,
  InputGroupAddon,
  InputGroupInput,
} from '@/components/ui/input-group'
import { Label } from '@/components/ui/label'

export function Payment1() {
  const [number, setNumber] = useState('')

  const formatNumber = (raw: string) =>
    raw
      .replace(/\D/g, '')
      .slice(0, 16)
      .replace(/(.{4})/g, '$1 ')
      .trim()

  return (
    <Card>
      <CardHeader>
        <CardTitle>Payment details</CardTitle>
      </CardHeader>
      <CardContent className="flex flex-col gap-4">
        <div className="flex flex-col gap-2">
          <Label htmlFor="cf-name">Name on card</Label>
          <Input
            id="cf-name"
            placeholder="Ada Lovelace"
            autoComplete="cc-name"
          />
        </div>
        <div className="flex flex-col gap-2">
          <Label htmlFor="cf-number">Card number</Label>
          <InputGroup>
            <InputGroupAddon>
              <CreditCard />
            </InputGroupAddon>
            <InputGroupInput
              id="cf-number"
              inputMode="numeric"
              placeholder="4242 4242 4242 4242"
              value={number}
              onChange={(e) => setNumber(formatNumber(e.target.value))}
            />
          </InputGroup>
        </div>
        <div className="flex gap-3">
          <div className="flex flex-1 flex-col gap-2">
            <Label htmlFor="cf-exp">Expiry</Label>
            <Input id="cf-exp" placeholder="MM / YY" inputMode="numeric" />
          </div>
          <div className="flex flex-1 flex-col gap-2">
            <Label htmlFor="cf-cvc">CVC</Label>
            <Input id="cf-cvc" placeholder="123" inputMode="numeric" />
          </div>
        </div>
      </CardContent>
      <CardFooter className="flex flex-col gap-3">
        <Button className="w-full">Save card</Button>
        <p className="text-muted-foreground flex items-center gap-1.5 text-xs">
          <Lock className="size-3" />
          Encrypted and processed securely.
        </p>
      </CardFooter>
    </Card>
  )
}

demo.tsx
import { Payment1 } from "@/components/ui/payment-1";

export default function DefaultPaymentDemo() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <div className="w-full max-w-sm">
        <Payment1 />
      </div>
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
npx shadcn@latest add button card input input-group label
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
