<!-- Field · @intentui · https://21st.dev/@intentui/components/field
     license: MIT · category: form
     Composable, accessible form field primitives — label, description, error message, fieldset, and legend — for building consistent form layouts. -->

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
components/ui/field.tsx
'use client'

import {
  FieldError as FieldErrorPrimitive,
  type FieldErrorProps,
} from 'react-aria-components/FieldError'
import { LabelContext, Label as LabelPrimitive, type LabelProps } from 'react-aria-components/Label'
import { Text, type TextProps } from 'react-aria-components/Text'
import { cn } from 'cn'
import { tv } from 'tailwind-variants'
import { cx } from '@/lib/primitive'

export const labelStyles = tv({
  base: [
    "select-none text-base/6 text-fg in-data-required:not-data-[slot='control-label']:after:ml-1.5 sm:text-sm/6",
    "in-data-required:not-data-[slot='control-label']:after:text-danger-subtle-fg in-data-required:not-data-[slot='control-label']:after:content-['*']",
    'in-disabled:pointer-events-none in-disabled:opacity-50 group-disabled:opacity-50',
  ],
})

export const descriptionStyles = tv({
  base: 'block text-muted-fg text-sm/6 in-disabled:opacity-50 group-disabled:opacity-50',
})

export const fieldErrorStyles = tv({
  base: 'block text-danger-subtle-fg text-sm/6 in-disabled:opacity-50 group-disabled:opacity-50 forced-colors:text-[Mark]',
})

export const fieldStyles = tv({
  base: [
    'w-full',
    '[&>[data-slot=control]+[data-slot=control]]:mt-2',
    '[&>[data-slot=label]+[data-slot=control]]:mt-2',
    "[&>[data-slot=label]+[slot='description']]:mt-1",
    '[&>[slot=description]+[data-slot=control]]:mt-2',
    '[&>[data-slot=control]+[slot=description]]:mt-2',
    '[&>[data-slot=control]+[slot=errorMessage]]:mt-2',
    '*:data-[slot=label]:font-medium',
    'in-disabled:opacity-50 disabled:opacity-50',
  ],
})

export function Label({ className, htmlFor, slot, ...props }: LabelProps) {
  const label = (
    <LabelPrimitive
      data-slot="label"
      htmlFor={htmlFor}
      slot={slot}
      {...props}
      className={labelStyles({ className })}
    />
  )

  if (htmlFor && slot === undefined) {
    return <LabelContext.Provider value={null}>{label}</LabelContext.Provider>
  }

  return label
}

export function Description({ className, ...props }: TextProps) {
  return <Text {...props} slot="description" className={descriptionStyles({ className })} />
}

export function Fieldset({ className, ...props }: React.ComponentProps<'fieldset'>) {
  return (
    <fieldset
      className={cn('*:data-[slot=text]:mt-1 [&>*+[data-slot=control]]:mt-6', className)}
      {...props}
    />
  )
}

export function FieldGroup({ className, ...props }: React.ComponentPropsWithoutRef<'div'>) {
  return <div data-slot="control" className={cn('space-y-6', className)} {...props} />
}

export function FieldError({ className, ...props }: FieldErrorProps) {
  return <FieldErrorPrimitive {...props} className={cx(fieldErrorStyles(), className)} />
}

export function Legend({ className, ...props }: React.ComponentProps<'legend'>) {
  return (
    <legend
      data-slot="legend"
      {...props}
      className={cn('font-semibold text-base/6 data-disabled:opacity-50', className)}
    />
  )
}

demo.tsx
"use client";

import { Description, Fieldset, Label, Legend } from "@/components/ui/field";

const inputClass =
  "mt-2 block w-full rounded-lg border border-input bg-bg px-3 py-2 text-fg text-sm shadow-xs outline-hidden placeholder:text-muted-fg focus:border-ring focus:ring-2 focus:ring-ring/20";

function RequiredLabel({
  htmlFor,
  children,
}: {
  htmlFor: string;
  children: React.ReactNode;
}) {
  return (
    <Label htmlFor={htmlFor}>
      {children} <span className="text-danger-subtle-fg">*</span>
    </Label>
  );
}

export default function FieldDemo() {
  return (
    <div className="w-full max-w-md p-6">
      <Fieldset>
        <Legend className="font-semibold text-fg text-base/6">
          Profile information
        </Legend>
        <Description>
          Update your account's profile information and email address.
        </Description>

        <div data-slot="control" className="space-y-6">
          <div>
            <RequiredLabel htmlFor="name">Name</RequiredLabel>
            <input id="name" className={inputClass} />
            <Description className="mt-2">
              This is your public display name.
            </Description>
          </div>

          <div>
            <RequiredLabel htmlFor="email">Email</RequiredLabel>
            <Description className="mt-1">
              This is your public display name.
            </Description>
            <input id="email" type="email" className={inputClass} />
          </div>

          <div>
            <RequiredLabel htmlFor="password">Password</RequiredLabel>
            <input id="password" type="password" className={inputClass} />
          </div>

          <button
            type="submit"
            className="inline-flex items-center justify-center rounded-lg bg-primary px-4 py-2 font-medium text-primary-fg text-sm shadow-xs outline-hidden hover:opacity-90 focus:ring-2 focus:ring-ring/30"
          >
            Register
          </button>
        </div>
      </Fieldset>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install cn react-aria-components tailwind-merge tailwind-variants
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add primitive
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
