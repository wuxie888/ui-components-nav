<!-- Slider Field (TanStack Form) · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/tsf-fields-04
     license: MIT · category: form
     A numeric slider form field wired with TanStack Form and validated against a Zod min/max range, showing the live value and inline error. -->

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
components/ui/tsf-fields-04.tsx
'use client'

import { useForm } from '@tanstack/react-form'
import { toast } from 'sonner'
import * as z from 'zod'

import { Button } from '@/components/ui/button'
import {
  Field,
  FieldContent,
  FieldDescription,
  FieldError,
  FieldGroup,
  FieldLabel,
} from '@/components/ui/field'
import { Slider } from '@/components/ui/slider'

const formSchema = z.object({
  budget: z
    .number()
    .min(10, 'Budget must be at least $10.')
    .max(100, 'Budget cannot exceed $100.'),
})

export function TsfFields04() {
  const form = useForm({
    defaultValues: { budget: 0 },
    validators: { onSubmit: formSchema },
    onSubmit: async ({ value }) => {
      toast.success('Budget set', { description: `$${value.budget} / month` })
    },
  })

  return (
    <form
      onSubmit={(e) => {
        e.preventDefault()
        form.handleSubmit()
      }}
      className="flex w-full max-w-sm flex-col gap-6"
    >
      <FieldGroup>
        <form.Field name="budget">
          {(field) => {
            const isInvalid =
              field.state.meta.isTouched && !field.state.meta.isValid
            return (
              <Field data-invalid={isInvalid}>
                <FieldContent>
                  <FieldLabel htmlFor={field.name}>Monthly budget</FieldLabel>
                  <FieldDescription>
                    ${field.state.value} / month
                  </FieldDescription>
                </FieldContent>
                <Slider
                  id={field.name}
                  min={0}
                  max={100}
                  step={5}
                  value={[field.state.value]}
                  onValueChange={(v) => field.handleChange((v as number[])[0])}
                  aria-invalid={isInvalid}
                />
                {isInvalid && <FieldError errors={field.state.meta.errors} />}
              </Field>
            )
          }}
        </form.Field>
      </FieldGroup>
      <Button type="submit" size="sm">
        Save
      </Button>
    </form>
  )
}

demo.tsx
import { TsfFields04 } from "@/components/ui/tsf-fields-04";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <TsfFields04 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @tanstack/react-form sonner zod
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button field slider
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
