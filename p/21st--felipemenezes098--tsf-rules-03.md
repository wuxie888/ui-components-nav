<!-- Number Range Form · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/tsf-rules-03
     license: MIT · category: form
     A TanStack Form number input field with Zod validation that keeps a quantity within a minimum and maximum range. -->

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
components/ui/tsf-rules-03.tsx
'use client'

import { useForm } from '@tanstack/react-form'
import { Hash } from 'lucide-react'
import { toast } from 'sonner'
import * as z from 'zod'

import { Button } from '@/components/ui/button'
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'
import {
  Field,
  FieldDescription,
  FieldError,
  FieldGroup,
  FieldLabel,
} from '@/components/ui/field'
import { Input } from '@/components/ui/input'

const MIN = 1
const MAX = 99

const formSchema = z.object({
  quantity: z
    .number({ message: 'Enter a number.' })
    .int('Whole numbers only.')
    .min(MIN, `Minimum is ${MIN}.`)
    .max(MAX, `Maximum is ${MAX}.`),
})

export function TsfRules03() {
  const form = useForm({
    defaultValues: { quantity: 1 },
    validators: { onSubmit: formSchema },
    onSubmit: async ({ value }) => {
      toast.success('Quantity confirmed', {
        description: `${value.quantity} item(s)`,
      })
    },
  })

  return (
    <Card className="w-full max-w-sm">
      <CardHeader>
        <CardTitle className="flex items-center gap-2">
          <Hash className="text-muted-foreground size-4" />
          Number range
        </CardTitle>
        <CardDescription>
          Tell us how many you&apos;d like to order.
        </CardDescription>
      </CardHeader>
      <CardContent>
        <form
          onSubmit={(e) => {
            e.preventDefault()
            form.handleSubmit()
          }}
        >
          <FieldGroup>
            <form.Field name="quantity">
              {(field) => {
                const isInvalid =
                  field.state.meta.isTouched && !field.state.meta.isValid
                return (
                  <Field data-invalid={isInvalid}>
                    <FieldLabel htmlFor={field.name}>Quantity</FieldLabel>
                    <Input
                      id={field.name}
                      name={field.name}
                      type="number"
                      inputMode="numeric"
                      min={MIN}
                      max={MAX}
                      aria-invalid={isInvalid}
                      value={field.state.value}
                      onBlur={field.handleBlur}
                      onChange={(e) =>
                        field.handleChange(e.target.valueAsNumber)
                      }
                    />
                    {isInvalid ? (
                      <FieldError errors={field.state.meta.errors} />
                    ) : (
                      <FieldDescription>
                        Pick a whole number between {MIN} and {MAX}.
                      </FieldDescription>
                    )}
                  </Field>
                )
              }}
            </form.Field>
            <Button type="submit" size="sm">
              Add to cart
            </Button>
          </FieldGroup>
        </form>
      </CardContent>
    </Card>
  )
}

demo.tsx
import TsfRules03 from "@/components/ui/tsf-rules-03";

export default function Demo() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <TsfRules03 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @tanstack/react-form lucide-react sonner zod
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button card field input
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
