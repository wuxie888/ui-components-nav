<!-- Newsletter Subscription Form · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/tsf-recipes-04
     license: MIT · category: form
     A newsletter signup form card with email and delivery-frequency fields, built with TanStack Form and Zod validation. -->

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
components/ui/tsf-recipes-04.tsx
'use client'

import { useForm } from '@tanstack/react-form'
import { Mail } from 'lucide-react'
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
  FieldError,
  FieldGroup,
  FieldLabel,
} from '@/components/ui/field'
import { Input } from '@/components/ui/input'
import {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectLabel,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select'

const formSchema = z.object({
  email: z.email('Enter a valid email.'),
  frequency: z.string().min(1, 'Pick how often to hear from us.'),
})

export function TsfRecipes04() {
  const form = useForm({
    defaultValues: { email: '', frequency: 'weekly' },
    validators: { onSubmit: formSchema },
    onSubmit: async ({ value }) => {
      toast.success('Subscribed', { description: value.email })
    },
  })

  return (
    <Card className="w-full max-w-sm">
      <CardHeader>
        <CardTitle className="flex items-center gap-2">
          <Mail className="text-muted-foreground size-4" />
          Newsletter
        </CardTitle>
        <CardDescription>Get product news in your inbox.</CardDescription>
      </CardHeader>
      <CardContent>
        <form
          onSubmit={(e) => {
            e.preventDefault()
            form.handleSubmit()
          }}
        >
          <FieldGroup>
            <form.Field name="email">
              {(field) => {
                const isInvalid =
                  field.state.meta.isTouched && !field.state.meta.isValid
                return (
                  <Field data-invalid={isInvalid}>
                    <FieldLabel htmlFor={field.name}>Email</FieldLabel>
                    <Input
                      id={field.name}
                      name={field.name}
                      type="email"
                      placeholder="ada@example.com"
                      aria-invalid={isInvalid}
                      value={field.state.value}
                      onBlur={field.handleBlur}
                      onChange={(e) => field.handleChange(e.target.value)}
                    />
                    {isInvalid && (
                      <FieldError errors={field.state.meta.errors} />
                    )}
                  </Field>
                )
              }}
            </form.Field>
            <form.Field name="frequency">
              {(field) => {
                const isInvalid =
                  field.state.meta.isTouched && !field.state.meta.isValid
                return (
                  <Field data-invalid={isInvalid}>
                    <FieldLabel htmlFor={field.name}>Frequency</FieldLabel>
                    <Select
                      value={field.state.value}
                      onValueChange={(value) => field.handleChange(value ?? '')}
                    >
                      <SelectTrigger
                        id={field.name}
                        className="w-full"
                        aria-invalid={isInvalid}
                      >
                        <SelectValue placeholder="How often?" />
                      </SelectTrigger>
                      <SelectContent>
                        <SelectGroup>
                          <SelectLabel>Delivery cadence</SelectLabel>
                          <SelectItem value="daily">Daily</SelectItem>
                          <SelectItem value="weekly">Weekly</SelectItem>
                          <SelectItem value="monthly">Monthly</SelectItem>
                        </SelectGroup>
                      </SelectContent>
                    </Select>
                    {isInvalid && (
                      <FieldError errors={field.state.meta.errors} />
                    )}
                  </Field>
                )
              }}
            </form.Field>
            <Button type="submit" size="sm">
              Subscribe
            </Button>
          </FieldGroup>
        </form>
      </CardContent>
    </Card>
  )
}

demo.tsx
import { TsfRecipes04 } from "@/components/ui/tsf-recipes-04";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <TsfRecipes04 />
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
npx shadcn@latest add button card field input select
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
