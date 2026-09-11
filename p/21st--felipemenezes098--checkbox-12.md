<!-- Checkbox Group Form · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/checkbox-12
     license: MIT · category: form
     A checkbox group bound to React Hook Form with Zod validation requiring at least one selection, showing an error and a success toast on submit. -->

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
components/ui/checkbox-12.tsx
'use client'

import { zodResolver } from '@hookform/resolvers/zod'
import { Controller, useForm } from 'react-hook-form'
import { toast } from 'sonner'
import * as z from 'zod'

import { Button } from '@/components/ui/button'
import { Checkbox } from '@/components/ui/checkbox'
import {
  Field,
  FieldError,
  FieldLabel,
  FieldLegend,
  FieldSet,
} from '@/components/ui/field'

const items = [
  { id: 'recents', label: 'Recents' },
  { id: 'home', label: 'Home' },
  { id: 'apps', label: 'Applications' },
  { id: 'desktop', label: 'Desktop' },
] as const

const formSchema = z.object({
  items: z.array(z.string()).min(1, 'Select at least one item.'),
})

type FormValues = z.infer<typeof formSchema>

export function Checkbox12() {
  const form = useForm<FormValues>({
    resolver: zodResolver(formSchema),
    defaultValues: {
      items: [],
    },
  })

  function onSubmit(data: FormValues) {
    toast.success('Submitted', { description: data.items.join(', ') })
  }

  return (
    <form
      onSubmit={form.handleSubmit(onSubmit)}
      className="flex w-full max-w-sm flex-col gap-4"
    >
      <Controller
        name="items"
        control={form.control}
        render={({ field, fieldState }) => (
          <FieldSet data-invalid={fieldState.invalid}>
            <FieldLegend variant="label">Sidebar</FieldLegend>
            <div className="flex flex-col gap-3">
              {items.map((item) => (
                <Field key={item.id} orientation="horizontal">
                  <Checkbox
                    id={`checkbox-12-${item.id}`}
                    checked={field.value.includes(item.id)}
                    onCheckedChange={(value) =>
                      field.onChange(
                        value === true
                          ? [...field.value, item.id]
                          : field.value.filter((id) => id !== item.id),
                      )
                    }
                  />
                  <FieldLabel htmlFor={`checkbox-12-${item.id}`}>
                    {item.label}
                  </FieldLabel>
                </Field>
              ))}
            </div>
            {fieldState.invalid && <FieldError errors={[fieldState.error]} />}
          </FieldSet>
        )}
      />
      <Button type="submit" size="sm" className="self-start">
        Submit
      </Button>
    </form>
  )
}

demo.tsx
import { Checkbox12 } from "@/components/ui/checkbox-12";
import { Toaster } from "sonner";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Checkbox12 />
      <Toaster />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @hookform/resolvers react-hook-form sonner zod
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button checkbox field
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
