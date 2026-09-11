<!-- React Hook Form Select · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/select-12
     license: MIT · category: form
     A select dropdown bound to React Hook Form with a Zod resolver that validates on submit and shows an inline field error until a department is chosen. -->

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
components/ui/select-12.tsx
'use client'

import { zodResolver } from '@hookform/resolvers/zod'
import { Controller, useForm } from 'react-hook-form'
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
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select'

const departments = [
  { value: 'design', label: 'Design' },
  { value: 'engineering', label: 'Engineering' },
  { value: 'marketing', label: 'Marketing' },
  { value: 'support', label: 'Support' },
] as const

const formSchema = z.object({
  department: z.string().min(1, 'Select a department.'),
})

type FormValues = z.infer<typeof formSchema>

export function Select12() {
  const form = useForm<FormValues>({
    resolver: zodResolver(formSchema),
    defaultValues: {
      department: '',
    },
  })

  function onSubmit(data: FormValues) {
    const label = departments.find(
      (item) => item.value === data.department,
    )?.label
    toast.success('Submitted', {
      description: label ?? data.department,
    })
  }

  return (
    <form
      id="select-rhf-form"
      onSubmit={form.handleSubmit(onSubmit)}
      className="flex w-full max-w-sm flex-col gap-4"
    >
      <FieldGroup>
        <Controller
          name="department"
          control={form.control}
          render={({ field, fieldState }) => (
            <Field data-invalid={fieldState.invalid}>
              <FieldLabel htmlFor="select-rhf-department">
                Department
              </FieldLabel>
              <Select value={field.value} onValueChange={field.onChange}>
                <SelectTrigger
                  id="select-rhf-department"
                  className="w-full"
                  aria-invalid={fieldState.invalid}
                >
                  <SelectValue placeholder="Select department" />
                </SelectTrigger>
                <SelectContent>
                  <SelectGroup>
                    {departments.map((item) => (
                      <SelectItem key={item.value} value={item.value}>
                        {item.label}
                      </SelectItem>
                    ))}
                  </SelectGroup>
                </SelectContent>
              </Select>
              {fieldState.invalid && <FieldError errors={[fieldState.error]} />}
            </Field>
          )}
        />
      </FieldGroup>
      <Button type="submit" size="sm">
        Submit
      </Button>
    </form>
  )
}

demo.tsx
import { Select12 } from "@/components/ui/select-12";

export default function Default() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center p-6">
      <Select12 />
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
