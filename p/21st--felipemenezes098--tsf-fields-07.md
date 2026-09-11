<!-- Select Field (TanStack Form) · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/tsf-fields-07
     license: MIT · category: form
     A grouped select field with labelled option groups, wired to TanStack Form with Zod validation and submit feedback. -->

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
components/ui/tsf-fields-07.tsx
'use client'

import { useForm } from '@tanstack/react-form'
import { toast } from 'sonner'
import * as z from 'zod'

import { Button } from '@/components/ui/button'
import {
  Field,
  FieldError,
  FieldGroup,
  FieldLabel,
} from '@/components/ui/field'
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
  framework: z.string().min(1, 'Select a framework.'),
})

export function TsfFields07() {
  const form = useForm({
    defaultValues: { framework: '' },
    validators: { onSubmit: formSchema },
    onSubmit: async ({ value }) => {
      toast.success('Framework selected', { description: value.framework })
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
        <form.Field name="framework">
          {(field) => {
            const isInvalid =
              field.state.meta.isTouched && !field.state.meta.isValid
            return (
              <Field data-invalid={isInvalid}>
                <FieldLabel htmlFor={field.name}>Framework</FieldLabel>
                <Select
                  value={field.state.value}
                  onValueChange={(value) => field.handleChange(value ?? '')}
                >
                  <SelectTrigger
                    id={field.name}
                    className="w-full"
                    aria-invalid={isInvalid}
                  >
                    <SelectValue placeholder="Select a framework" />
                  </SelectTrigger>
                  <SelectContent>
                    <SelectGroup>
                      <SelectLabel>Frontend</SelectLabel>
                      <SelectItem value="react">React</SelectItem>
                      <SelectItem value="vue">Vue</SelectItem>
                      <SelectItem value="svelte">Svelte</SelectItem>
                    </SelectGroup>
                    <SelectGroup>
                      <SelectLabel>Backend</SelectLabel>
                      <SelectItem value="express">Express</SelectItem>
                      <SelectItem value="fastify">Fastify</SelectItem>
                      <SelectItem value="nest">NestJS</SelectItem>
                    </SelectGroup>
                  </SelectContent>
                </Select>
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
import { TsfFields07 } from "@/components/ui/tsf-fields-07";

export default function DemoTsfFields07() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <TsfFields07 />
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
npx shadcn@latest add button field select
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
