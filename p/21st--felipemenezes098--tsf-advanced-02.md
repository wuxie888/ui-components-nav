<!-- Invoice Form with Line Item Dialog · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/tsf-advanced-02
     license: agpl-3.0 · category: form
     An invoice builder form that adds each billable line item through its own validating dialog before appending it to the list and computing the total. -->

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
components/ui/tsf-advanced-02.tsx
'use client'

import { useForm } from '@tanstack/react-form'
import { ReceiptTextIcon, Trash2Icon } from 'lucide-react'
import { toast } from 'sonner'
import * as z from 'zod'

import { Button } from '@/components/ui/button'
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'
import {
  Empty,
  EmptyDescription,
  EmptyHeader,
  EmptyMedia,
  EmptyTitle,
} from '@/components/ui/empty'
import {
  Field,
  FieldError,
  FieldLabel,
} from '@/components/ui/field'
import { Input } from '@/components/ui/input'

import {
  LineItemDialog,
  lineItemSchema,
  type LineItem,
} from './tsf-advanced-02-line-item-dialog'

const formSchema = z.object({
  client: z.string().min(1, 'Client name is required.'),
  items: z.array(lineItemSchema).min(1, 'Add at least one line item.'),
})

function lineTotal(item: LineItem) {
  return Number(item.quantity) * Number(item.unitPrice)
}

function money(value: number) {
  return `$${value.toFixed(2)}`
}

export function TsfAdvanced02() {
  const form = useForm({
    defaultValues: {
      client: '',
      items: [] as LineItem[],
    },
    validators: { onSubmit: formSchema },
    onSubmit: async ({ value }) => {
      const total = value.items.reduce((sum, i) => sum + lineTotal(i), 0)
      toast.success('Invoice created', {
        description: `${value.items.length} item(s) · ${money(total)}`,
      })
    },
  })

  return (
    <Card className="w-full max-w-lg">
      <CardHeader>
        <CardTitle>Create invoice</CardTitle>
        <CardDescription>
          Add a client, then build the line items one at a time.
        </CardDescription>
      </CardHeader>
      <form
        onSubmit={(e) => {
          e.preventDefault()
          form.handleSubmit()
        }}
        className="flex flex-col gap-3"
      >
        <CardContent className="flex flex-col gap-6">
          <form.Field name="client">
            {(field) => {
              const isInvalid =
                field.state.meta.isTouched && !field.state.meta.isValid
              return (
                <Field data-invalid={isInvalid}>
                  <FieldLabel htmlFor={field.name}>Client</FieldLabel>
                  <Input
                    id={field.name}
                    name={field.name}
                    placeholder="Acme Inc."
                    aria-invalid={isInvalid}
                    value={field.state.value}
                    onBlur={field.handleBlur}
                    onChange={(e) => field.handleChange(e.target.value)}
                  />
                  {isInvalid && <FieldError errors={field.state.meta.errors} />}
                </Field>
              )
            }}
          </form.Field>

          <form.Field name="items" mode="array">
            {(itemsField) => {
              const items = itemsField.state.value
              const total = items.reduce((sum, i) => sum + lineTotal(i), 0)
              return (
                <div className="flex flex-col gap-3">
                  <div className="flex items-center justify-between">
                    <span className="text-sm font-medium">Line items</span>
                    {items.length > 0 && (
                      <span className="text-muted-foreground text-sm tabular-nums">
                        Total {money(total)}
                      </span>
                    )}
                  </div>

                  {items.length === 0 ? (
                    <Empty className="border">
                      <EmptyHeader>
                        <EmptyMedia variant="icon">
                          <ReceiptTextIcon />
                        </EmptyMedia>
                        <EmptyTitle>No line items</EmptyTitle>
                        <EmptyDescription>
                          Add billable items to build out this invoice.
                        </EmptyDescription>
                      </EmptyHeader>
                    </Empty>
                  ) : (
                    <ul className="flex flex-col gap-2">
                      {items.map((item, index) => (
                        <li
                          key={index}
                          className="bg-muted/30 flex items-center justify-between gap-3 rounded-lg border px-3 py-2"
                        >
                          <div className="min-w-0">
                            <p className="truncate text-sm">
                              {item.description}
                            </p>
                            <p className="text-muted-foreground text-xs tabular-nums">
                              {item.quantity} × {money(Number(item.unitPrice))}
                            </p>
                          </div>
                          <div className="flex items-center gap-1">
                            <span className="text-sm tabular-nums">
                              {money(lineTotal(item))}
                            </span>
                            <Button
                              type="button"
                              variant="ghost"
                              size="icon-sm"
                              aria-label="Remove item"
                              onClick={() => itemsField.removeValue(index)}
                            >
                              <Trash2Icon className="size-4" />
                            </Button>
                          </div>
                        </li>
                      ))}
                    </ul>
                  )}

                  <LineItemDialog
                    onAdd={(item) => itemsField.pushValue(item)}
                  />
                </div>
              )
            }}
          </form.Field>
        </CardContent>
        <form.Subscribe selector={(state) => state.values.items.length}>
          {(count) => (
            <CardFooter className="justify-end">
              <Button type="submit" size="sm" disabled={count === 0}>
                Create invoice
              </Button>
            </CardFooter>
          )}
        </form.Subscribe>
      </form>
    </Card>
  )
}

components/ui/tsf-advanced-02-line-item-dialog.tsx
'use client'

import { useState } from 'react'
import { useForm } from '@tanstack/react-form'
import { PlusIcon } from 'lucide-react'
import * as z from 'zod'

import { Button } from '@/components/ui/button'
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from '@/components/ui/dialog'
import {
  Field,
  FieldError,
  FieldGroup,
  FieldLabel,
} from '@/components/ui/field'
import { Input } from '@/components/ui/input'

export const lineItemSchema = z.object({
  description: z.string().min(1, 'Describe the item.'),
  quantity: z
    .string()
    .min(1, 'Required.')
    .refine(
      (v) => Number.isInteger(Number(v)) && Number(v) >= 1,
      'Use a whole number, 1 or more.',
    ),
  unitPrice: z
    .string()
    .min(1, 'Required.')
    .refine((v) => Number(v) > 0, 'Must be greater than 0.'),
})

export type LineItem = z.infer<typeof lineItemSchema>

const emptyItem: LineItem = { description: '', quantity: '1', unitPrice: '' }

export function LineItemDialog({
  onAdd,
}: Readonly<{ onAdd: (item: LineItem) => void }>) {
  const [open, setOpen] = useState(false)
  const form = useForm({
    defaultValues: emptyItem,
    validators: { onSubmit: lineItemSchema },
    onSubmit: async ({ value }) => {
      onAdd(value)
      form.reset()
      setOpen(false)
    },
  })

  return (
    <Dialog
      open={open}
      onOpenChange={(next) => {
        setOpen(next)
        if (!next) form.reset()
      }}
    >
      <DialogTrigger
        render={
          <Button type="button" variant="outline" size="sm" className="w-full">
            <PlusIcon className="size-4" />
            Add line item
          </Button>
        }
      />
      <DialogContent>
        <DialogHeader>
          <DialogTitle>Add line item</DialogTitle>
          <DialogDescription>
            This item is validated on its own before it joins the invoice.
          </DialogDescription>
        </DialogHeader>
        <form
          onSubmit={(e) => {
            e.preventDefault()
            e.stopPropagation()
            form.handleSubmit()
          }}
        >
          <FieldGroup>
            <form.Field name="description">
              {(field) => {
                const isInvalid =
                  field.state.meta.isTouched && !field.state.meta.isValid
                return (
                  <Field data-invalid={isInvalid}>
                    <FieldLabel htmlFor={field.name}>Description</FieldLabel>
                    <Input
                      id={field.name}
                      name={field.name}
                      placeholder="Design retainer"
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
            <div className="flex gap-3">
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
                        min="1"
                        step="1"
                        placeholder="1"
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
              <form.Field name="unitPrice">
                {(field) => {
                  const isInvalid =
                    field.state.meta.isTouched && !field.state.meta.isValid
                  return (
                    <Field data-invalid={isInvalid}>
                      <FieldLabel htmlFor={field.name}>
                        Unit price (USD)
                      </FieldLabel>
                      <Input
                        id={field.name}
                        name={field.name}
                        type="number"
                        min="0"
                        step="0.01"
                        placeholder="0.00"
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
            </div>
          </FieldGroup>
          <DialogFooter className="mt-6">
            <Button
              type="button"
              variant="outline"
              size="sm"
              onClick={() => setOpen(false)}
            >
              Cancel
            </Button>
            <Button type="submit" size="sm">
              Add item
            </Button>
          </DialogFooter>
        </form>
      </DialogContent>
    </Dialog>
  )
}

demo.tsx
import { TsfAdvanced02 } from "@/components/ui/tsf-advanced-02";
import { Toaster } from "sonner";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <TsfAdvanced02 />
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
npx shadcn@latest add button card dialog empty field input
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
