<!-- React Hook Form Dropdown Field · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/rhf-fields-09
     license: agpl-3.0 · category: form
     A single-choice form field that uses a dropdown menu radio group bound to React Hook Form via Controller, with Zod validation and inline error messages. -->

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
components/ui/rhf-fields-09.tsx
'use client'

import { Controller, useForm } from 'react-hook-form'
import { zodResolver } from '@hookform/resolvers/zod'
import { ChevronDownIcon } from 'lucide-react'
import { toast } from 'sonner'
import * as z from 'zod'

import { Button } from '@/components/ui/button'
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuGroup,
  DropdownMenuLabel,
  DropdownMenuRadioGroup,
  DropdownMenuRadioItem,
  DropdownMenuSeparator,
  DropdownMenuTrigger,
} from '@/components/ui/dropdown-menu'
import {
  Field,
  FieldError,
  FieldGroup,
  FieldLabel,
} from '@/components/ui/field'

const VISIBILITY = [
  { value: 'public', label: 'Public' },
  { value: 'private', label: 'Private' },
  { value: 'internal', label: 'Internal' },
] as const

const formSchema = z.object({
  visibility: z.string().min(1, 'Select a visibility.'),
})

type FormValues = z.infer<typeof formSchema>

export function RhfFields09() {
  const form = useForm<FormValues>({
    resolver: zodResolver(formSchema),
    defaultValues: { visibility: '' },
  })

  function onSubmit(data: FormValues) {
    toast.success('Visibility updated', { description: data.visibility })
  }

  return (
    <form
      onSubmit={form.handleSubmit(onSubmit)}
      className="flex w-full max-w-sm flex-col gap-6"
    >
      <FieldGroup>
        <Controller
          control={form.control}
          name="visibility"
          render={({ field, fieldState }) => {
            const selected = VISIBILITY.find((v) => v.value === field.value)
            return (
              <Field data-invalid={fieldState.invalid}>
                <FieldLabel htmlFor="rhf-fields-09-visibility">
                  Visibility
                </FieldLabel>
                <DropdownMenu>
                  <DropdownMenuTrigger
                    render={
                      <Button
                        id="rhf-fields-09-visibility"
                        type="button"
                        variant="outline"
                        aria-invalid={fieldState.invalid}
                        className="w-full justify-between font-normal"
                      >
                        {selected ? selected.label : 'Select visibility'}
                        <ChevronDownIcon className="text-muted-foreground" />
                      </Button>
                    }
                  />
                  <DropdownMenuContent
                    align="start"
                    className="w-(--anchor-width)"
                  >
                    <DropdownMenuGroup>
                      <DropdownMenuLabel>
                        Repository visibility
                      </DropdownMenuLabel>
                    </DropdownMenuGroup>
                    <DropdownMenuSeparator />
                    <DropdownMenuRadioGroup
                      value={field.value}
                      onValueChange={field.onChange}
                    >
                      {VISIBILITY.map((item) => (
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
                {fieldState.invalid && (
                  <FieldError errors={[fieldState.error]} />
                )}
              </Field>
            )
          }}
        />
      </FieldGroup>
      <Button type="submit" size="sm">
        Save
      </Button>
    </form>
  )
}

demo.tsx
"use client";

import RhfFields09 from "@/components/ui/rhf-fields-09";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <RhfFields09 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @hookform/resolvers lucide-react react-hook-form sonner zod
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
