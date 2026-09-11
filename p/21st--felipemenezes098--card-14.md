<!-- Payment Form Card · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/card-14
     license: MIT · category: form
     A checkout payment card with fields for cardholder name, card number with an icon, expiry, CVC, and a country select plus a pay button. -->

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
components/ui/card-14.tsx
import { CreditCardIcon } from 'lucide-react'

import { Button } from '@/components/ui/button'
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'
import { Field, FieldGroup, FieldLabel } from '@/components/ui/field'
import { Input } from '@/components/ui/input'
import {
  InputGroup,
  InputGroupAddon,
  InputGroupInput,
} from '@/components/ui/input-group'
import {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select'

export function Card14() {
  return (
    <Card className="w-full max-w-sm">
      <CardHeader>
        <CardTitle>Payment details</CardTitle>
        <CardDescription>
          Enter your card information to complete checkout.
        </CardDescription>
      </CardHeader>
      <CardContent>
        <form>
          <FieldGroup>
            <Field>
              <FieldLabel htmlFor="card-14-name">Name on card</FieldLabel>
              <Input id="card-14-name" placeholder="John Doe" />
            </Field>
            <Field>
              <FieldLabel htmlFor="card-14-number">Card number</FieldLabel>
              <InputGroup>
                <InputGroupInput
                  id="card-14-number"
                  placeholder="1234 1234 1234 1234"
                  inputMode="numeric"
                />
                <InputGroupAddon>
                  <CreditCardIcon />
                </InputGroupAddon>
              </InputGroup>
            </Field>
            <div className="grid grid-cols-2 gap-4">
              <Field>
                <FieldLabel htmlFor="card-14-expiry">Expires</FieldLabel>
                <Input id="card-14-expiry" placeholder="MM / YY" />
              </Field>
              <Field>
                <FieldLabel htmlFor="card-14-cvc">CVC</FieldLabel>
                <Input id="card-14-cvc" placeholder="123" inputMode="numeric" />
              </Field>
            </div>
            <Field>
              <FieldLabel htmlFor="card-14-country">Country</FieldLabel>
              <Select defaultValue="us">
                <SelectTrigger id="card-14-country" className="w-full">
                  <SelectValue />
                </SelectTrigger>
                <SelectContent>
                  <SelectGroup>
                    <SelectItem value="us">United States</SelectItem>
                    <SelectItem value="br">Brazil</SelectItem>
                    <SelectItem value="pt">Portugal</SelectItem>
                    <SelectItem value="uk">United Kingdom</SelectItem>
                  </SelectGroup>
                </SelectContent>
              </Select>
            </Field>
          </FieldGroup>
        </form>
      </CardContent>
      <CardFooter>
        <Button type="submit" className="w-full">
          Pay $29.00
        </Button>
      </CardFooter>
    </Card>
  )
}

demo.tsx
import { Card14 } from "@/components/ui/card-14";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Card14 />
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
npx shadcn@latest add button card field input input-group select
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
