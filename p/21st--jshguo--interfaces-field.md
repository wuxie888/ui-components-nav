<!-- Field · @jshguo · https://21st.dev/@jshguo/components/interfaces-field
     license: MIT · category: form
     A comprehensive field system with support for labels, descriptions, errors, and complex form layouts. -->

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
"use client"

import { useMemo } from "react"
import { cva, type VariantProps } from "class-variance-authority"

import { cn } from "@/lib/utils"
import { Label } from "@/components/ui/label"
import { Separator } from "@/components/ui/separator"

function FieldSet({ className, ...props }: React.ComponentProps<"fieldset">) {
    return (
        <fieldset
            data-slot="field-set"
            className={cn(
                "flex flex-col gap-6",
                "has-[>[data-slot=checkbox-group]]:gap-3 has-[>[data-slot=radio-group]]:gap-3",
                className
            )}
            {...props}
        />
    )
}

function FieldLegend({
    className,
    variant = "legend",
    ...props
}: React.ComponentProps<"legend"> & { variant?: "legend" | "label" }) {
    return (
        <legend
            data-slot="field-legend"
            data-variant={variant}
            className={cn(
                "mb-3 font-medium",
                "data-[variant=legend]:text-base",
                "data-[variant=label]:text-sm",
                className
            )}
            {...props}
        />
    )
}

function FieldGroup({ className, ...props }: React.ComponentProps<"div">) {
    return (
        <div
            data-slot="field-group"
            className={cn(
                "group/field-group @container/field-group flex w-full flex-col gap-7 data-[slot=checkbox-group]:gap-3 *:data-[slot=field-group]:gap-4",
                className
            )}
            {...props}
        />
    )
}

const fieldVariants = cva(
    "group/field flex w-full gap-3 data-[invalid=true]:text-danger",
    {
        variants: {
            orientation: {
                vertical: ["flex-col [&>*]:w-full [&>.sr-only]:w-auto"],
                horizontal: [
                    "flex-row items-center",
                    "[&>[data-slot=field-label]]:flex-auto",
                    "has-[>[data-slot=field-content]]:items-start has-[>[data-slot=field-content]]:[&>[role=checkbox],[role=radio]]:mt-px",
                ],
                responsive: [
                    "flex-col [&>*]:w-full [&>.sr-only]:w-auto @md/field-group:flex-row @md/field-group:items-center @md/field-group:[&>*]:w-auto",
                    "@md/field-group:[&>[data-slot=field-label]]:flex-auto",
                    "@md/field-group:has-[>[data-slot=field-content]]:items-start @md/field-group:has-[>[data-slot=field-content]]:[&>[role=checkbox],[role=radio]]:mt-px",
                ],
            },
        },
        defaultVariants: {
            orientation: "vertical",
        },
    }
)

function Field({
    className,
    orientation = "vertical",
    ...props
}: React.ComponentProps<"div"> & VariantProps<typeof fieldVariants>) {
    return (
        <div
            role="group"
            data-slot="field"
            data-orientation={orientation}
            className={cn(fieldVariants({ orientation }), className)}
            {...props}
        />
    )
}

function FieldContent({ className, ...props }: React.ComponentProps<"div">) {
    return (
        <div
            data-slot="field-content"
            className={cn(
                "group/field-content flex flex-1 flex-col gap-1.5 leading-snug",
                className
            )}
            {...props}
        />
    )
}

function FieldLabel({
    className,
    ...props
}: React.ComponentProps<typeof Label>) {
    return (
        <Label
            data-slot="field-label"
            className={cn(
                "group/field-label peer/field-label flex w-fit gap-2 leading-snug group-data-[disabled=true]/field:opacity-50",
                "has-[>[data-slot=field]]:w-full has-[>[data-slot=field]]:flex-col has-[>[data-slot=field]]:rounded-md has-[>[data-slot=field]]:border *:data-[slot=field]:p-4",
                "has-data-[state=checked]:bg-primary/5 has-data-[state=checked]:border-primary dark:has-data-[state=checked]:bg-primary/10",
                className
            )}
            {...props}
        />
    )
}

function FieldTitle({ className, ...props }: React.ComponentProps<"div">) {
    return (
        <div
            data-slot="field-label"
            className={cn(
                "flex w-fit items-center gap-2 text-sm leading-snug font-medium group-data-[disabled=true]/field:opacity-50",
                className
            )}
            {...props}
        />
    )
}

function FieldDescription({ className, ...props }: React.ComponentProps<"p">) {
    return (
        <p
            data-slot="field-description"
            className={cn(
                "text-muted-foreground text-sm leading-normal font-normal group-has-data-[orientation=horizontal]/field:text-balance",
                "last:mt-0 nth-last-2:-mt-1 [[data-variant=legend]+&]:-mt-1.5",
                "[&>a:hover]:text-primary [&>a]:underline [&>a]:underline-offset-4",
                className
            )}
            {...props}
        />
    )
}

function FieldSeparator({
    children,
    className,
    ...props
}: React.ComponentProps<"div"> & {
    children?: React.ReactNode
}) {
    return (
        <div
            data-slot="field-separator"
            data-content={!!children}
            className={cn(
                "relative -my-2 h-5 text-sm group-data-[variant=outline]/field-group:-mb-2",
                className
            )}
            {...props}
        >
            <Separator className="absolute inset-0 top-1/2" />
            {children && (
                <span
                    className="bg-background text-muted-foreground relative mx-auto block w-fit px-2"
                    data-slot="field-separator-content"
                >
                    {children}
                </span>
            )}
        </div>
    )
}

function FieldError({
    className,
    children,
    errors,
    ...props
}: React.ComponentProps<"div"> & {
    errors?: Array<{ message?: string } | undefined>
}) {
    const content = useMemo(() => {
        if (children) {
            return children
        }

        if (!errors?.length) {
            return null
        }

        const uniqueErrors = [
            ...new Map(errors.map((error) => [error?.message, error])).values(),
        ]

        if (uniqueErrors?.length == 1) {
            return uniqueErrors[0]?.message
        }

        return (
            <ul className="ml-4 flex list-disc flex-col gap-1">
                {uniqueErrors.map(
                    (error, index) =>
                        error?.message && <li key={index}>{error.message}</li>
                )}
            </ul>
        )
    }, [children, errors])

    if (!content) {
        return null
    }

    return (
        <div
            role="alert"
            data-slot="field-error"
            className={cn("text-danger text-sm font-normal", className)}
            {...props}
        >
            {content}
        </div>
    )
}

export {
    Field,
    FieldLabel,
    FieldDescription,
    FieldError,
    FieldGroup,
    FieldLegend,
    FieldSeparator,
    FieldSet,
    FieldContent,
    FieldTitle,
}

demo.tsx
"use client"

import { useState } from "react"

import {
  Field,
  FieldContent,
  FieldDescription,
  FieldGroup,
  FieldLabel,
  FieldLegend,
  FieldSeparator,
  FieldSet,
} from "@/components/ui/interfaces-field"
import { Checkbox } from "@/components/ui/checkbox"
import { Input } from "@/components/ui/input"
import { Textarea } from "@/components/ui/textarea"

export default function ComplexFormDemo() {
  const [sameAsShipping, setSameAsShipping] = useState(true)

  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background p-6 overflow-hidden">
      <div className="w-full max-w-3xl rounded-3xl border bg-background p-6 shadow-sm md:p-8">
        <FieldGroup>
          <FieldSet>
            <div>
              <FieldLegend>Payment Method</FieldLegend>
              <FieldDescription>All transactions are secure and encrypted</FieldDescription>
            </div>

            <Field>
              <FieldLabel htmlFor="card-name">Name on Card</FieldLabel>
              <FieldContent>
                <Input id="card-name" placeholder="Name on Card" defaultValue="David Larsen" />
              </FieldContent>
            </Field>

            <Field>
              <FieldLabel htmlFor="card-number">Card Number</FieldLabel>
              <FieldContent>
                <Input id="card-number" placeholder="Card Number" defaultValue="4242 4242 4242 4242" />
                <FieldDescription>Enter your 16-digit card number</FieldDescription>
              </FieldContent>
            </Field>

            <div className="grid gap-4 md:grid-cols-3">
              <Field>
                <FieldLabel htmlFor="card-month">Month</FieldLabel>
                <FieldContent>
                  <Input id="card-month" placeholder="Month" defaultValue="12" />
                </FieldContent>
              </Field>
              <Field>
                <FieldLabel htmlFor="card-year">Year</FieldLabel>
                <FieldContent>
                  <Input id="card-year" placeholder="Year" defaultValue="2028" />
                </FieldContent>
              </Field>
              <Field>
                <FieldLabel htmlFor="card-cvv">CVV</FieldLabel>
                <FieldContent>
                  <Input id="card-cvv" placeholder="CVV" defaultValue="123" />
                </FieldContent>
              </Field>
            </div>
          </FieldSet>

          <FieldSeparator />

          <FieldSet>
            <div>
              <FieldLegend>Billing Address</FieldLegend>
              <FieldDescription>
                The billing address associated with your payment method
              </FieldDescription>
            </div>

            <Field orientation="horizontal">
              <Checkbox
                id="same-as-shipping"
                checked={sameAsShipping}
                onCheckedChange={(checked) => setSameAsShipping(checked === true)}
              />
              <FieldLabel htmlFor="same-as-shipping">Same as shipping address</FieldLabel>
            </Field>

            <Field>
              <FieldLabel htmlFor="comments">Comments</FieldLabel>
              <FieldContent>
                <Textarea
                  id="comments"
                  placeholder="Comments"
                  defaultValue="Leave delivery instructions for the concierge."
                  className="min-h-28 resize-none"
                />
              </FieldContent>
            </Field>

            <div className="flex flex-col gap-3 sm:flex-row">
              <button
                type="button"
                className="inline-flex h-10 items-center justify-center rounded-md bg-primary px-4 py-2 text-sm font-medium text-primary-foreground shadow-sm transition-colors hover:bg-primary/90"
              >
                Submit
              </button>
              <button
                type="button"
                className="inline-flex h-10 items-center justify-center rounded-md border bg-background px-4 py-2 text-sm font-medium shadow-sm transition-colors hover:bg-accent hover:text-accent-foreground"
              >
                Reset
              </button>
            </div>
          </FieldSet>
        </FieldGroup>
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add label separator styles utils
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
