<!-- Sign In Form · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/tsf-recipes-03
     license: no-license · category: sign-in
     A sign in card with email and password fields validated on submit using TanStack Form and Zod. -->

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
components/ui/tsf-recipes-03.tsx
'use client'

import { useForm } from '@tanstack/react-form'
import { LogIn } from 'lucide-react'
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

const formSchema = z.object({
  email: z.email('Enter a valid email.'),
  password: z.string().min(1, 'Enter your password.'),
})

export function TsfRecipes03() {
  const form = useForm({
    defaultValues: { email: '', password: '' },
    validators: { onSubmit: formSchema },
    onSubmit: async ({ value }) => {
      toast.success('Welcome back', { description: value.email })
    },
  })

  return (
    <Card className="w-full max-w-sm">
      <CardHeader>
        <CardTitle className="flex items-center gap-2">
          <LogIn className="text-muted-foreground size-4" />
          Sign in
        </CardTitle>
        <CardDescription>Use your email and password to continue.</CardDescription>
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
            <form.Field name="password">
              {(field) => {
                const isInvalid =
                  field.state.meta.isTouched && !field.state.meta.isValid
                return (
                  <Field data-invalid={isInvalid}>
                    <FieldLabel htmlFor={field.name}>Password</FieldLabel>
                    <Input
                      id={field.name}
                      name={field.name}
                      type="password"
                      placeholder="••••••••"
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
            <Button type="submit" size="sm">
              Sign in
            </Button>
          </FieldGroup>
        </form>
      </CardContent>
    </Card>
  )
}

demo.tsx
import { TsfRecipes03 } from "@/components/ui/tsf-recipes-03";

export default function DemoTsfRecipes03() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <TsfRecipes03 />
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
