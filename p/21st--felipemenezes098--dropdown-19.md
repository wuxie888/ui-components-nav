<!-- TanStack Form Dropdown · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/dropdown-19
     license: agpl-3.0 · category: form
     A visibility dropdown-menu radio group wired to TanStack Form with Zod validation, showing inline field errors on submit and a success toast. -->

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
components/ui/dropdown-19.tsx
'use client'

import { useForm } from '@tanstack/react-form'
import { toast } from 'sonner'
import * as z from 'zod'

import { Button } from '@/components/ui/button'
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuRadioGroup,
  DropdownMenuRadioItem,
  DropdownMenuTrigger,
} from '@/components/ui/dropdown-menu'
import {
  Field,
  FieldError,
  FieldGroup,
  FieldLabel,
} from '@/components/ui/field'
import { ChevronDownIcon } from 'lucide-react'

const visibilities = [
  { value: 'public', label: 'Public' },
  { value: 'private', label: 'Private' },
  { value: 'team', label: 'Team only' },
] as const

const formSchema = z.object({
  visibility: z.string().min(1, 'Choose a visibility.'),
})

export function Dropdown19() {
  const form = useForm({
    defaultValues: {
      visibility: '',
    },
    validators: {
      onSubmit: formSchema,
    },
    onSubmit: async ({ value }) => {
      const label = visibilities.find(
        (item) => item.value === value.visibility,
      )?.label
      toast.success('Submitted', {
        description: label ?? value.visibility,
      })
    },
  })

  return (
    <form
      onSubmit={(e) => {
        e.preventDefault()
        form.handleSubmit()
      }}
      className="flex w-full max-w-sm flex-col gap-4"
    >
      <FieldGroup>
        <form.Field name="visibility">
          {(field) => {
            const isInvalid =
              field.state.meta.isTouched && !field.state.meta.isValid
            const selected = visibilities.find(
              (item) => item.value === field.state.value,
            )

            return (
              <Field data-invalid={isInvalid}>
                <FieldLabel htmlFor={field.name}>Visibility</FieldLabel>
                <DropdownMenu>
                  <DropdownMenuTrigger
                    render={
                      <Button
                        id={field.name}
                        type="button"
                        variant="outline"
                        className="w-full justify-between font-normal"
                        aria-invalid={isInvalid}
                      >
                        <span
                          className={selected ? '' : 'text-muted-foreground'}
                        >
                          {selected?.label ?? 'Select visibility'}
                        </span>
                        <ChevronDownIcon className="text-muted-foreground" />
                      </Button>
                    }
                  />
                  <DropdownMenuContent
                    align="start"
                    className="w-(--anchor-width)"
                  >
                    <DropdownMenuRadioGroup
                      value={field.state.value}
                      onValueChange={(value) => {
                        field.handleChange(value)
                        field.handleBlur()
                      }}
                    >
                      {visibilities.map((item) => (
                        <DropdownMenuRadioItem
                          key={item.value}
                          value={item.value}
                        >
                          {item.label}
                        </DropdownMenuRadioItem>
                      ))}
                    </DropdownMenuRadioGroup>
                  </DropdownMenuContent>
                </DropdownMenu>
                {isInvalid && <FieldError errors={field.state.meta.errors} />}
              </Field>
            )
          }}
        </form.Field>
      </FieldGroup>
      <Button type="submit" size="sm">
        Submit
      </Button>
    </form>
  )
}

demo.tsx
import Dropdown19 from "@/components/ui/dropdown-19";
import { Toaster } from "sonner";

export default function DropdownFormDemo() {
  return (
    <div className="flex min-h-72 w-full items-center justify-center p-6">
      <Dropdown19 />
      <Toaster />
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
npx shadcn@latest add button dropdown-menu field label separator
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
