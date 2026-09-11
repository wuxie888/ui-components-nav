<!-- Saved Payment Methods · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/payment-2
     license: no-license · category: card
     A card listing saved credit cards with selectable default, remove buttons, and an empty state for managing billing payment methods. -->

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
components/ui/payment-2.tsx
'use client'

import { useState } from 'react'
import { Check, CreditCard, Plus, Trash2 } from 'lucide-react'

import { Badge } from '@/components/ui/badge'
import { Button } from '@/components/ui/button'
import {
  Card,
  CardContent,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'
import {
  Empty,
  EmptyDescription,
  EmptyMedia,
  EmptyTitle,
} from '@/components/ui/empty'

type Method = {
  id: string
  brand: string
  last4: string
  exp: string
}

const INITIAL: Method[] = [
  { id: 'visa', brand: 'Visa', last4: '4242', exp: '08 / 27' },
  { id: 'mc', brand: 'Mastercard', last4: '8210', exp: '11 / 26' },
  { id: 'amex', brand: 'Amex', last4: '0005', exp: '03 / 28' },
]

export function Payment2() {
  const [methods, setMethods] = useState<Method[]>(INITIAL)
  const [selected, setSelected] = useState('visa')

  const remove = (id: string) => {
    setMethods((prev) => {
      const next = prev.filter((m) => m.id !== id)
      if (id === selected && next.length > 0) setSelected(next[0].id)
      return next
    })
  }

  return (
    <Card>
      <CardHeader>
        <CardTitle>Payment methods</CardTitle>
      </CardHeader>
      <CardContent className="flex flex-col gap-3">
        {methods.length === 0 ? (
          <Empty className="py-8">
            <EmptyMedia variant="icon">
              <CreditCard className="size-5" />
            </EmptyMedia>
            <EmptyTitle>No payment methods</EmptyTitle>
            <EmptyDescription>
              Add a card to start your subscription.
            </EmptyDescription>
          </Empty>
        ) : (
          methods.map((method) => {
            const active = method.id === selected
            return (
              <div
                key={method.id}
                role="button"
                tabIndex={0}
                onClick={() => setSelected(method.id)}
                onKeyDown={(e) => {
                  if (e.key === 'Enter' || e.key === ' ') {
                    e.preventDefault()
                    setSelected(method.id)
                  }
                }}
                className={`flex cursor-pointer items-center gap-3 rounded-lg border p-3 text-left transition-colors ${
                  active ? 'border-primary bg-primary/5' : 'hover:bg-muted/50'
                }`}
              >
                <CreditCard className="text-muted-foreground size-5 shrink-0" />
                <div className="flex min-w-0 flex-1 flex-col">
                  <span className="truncate text-sm font-medium">
                    {method.brand} ending in {method.last4}
                  </span>
                  <span className="text-muted-foreground truncate text-xs">
                    Expires {method.exp}
                  </span>
                </div>
                {active && (
                  <Badge variant="secondary" className="shrink-0">
                    Default
                  </Badge>
                )}
                <Button
                  variant="ghost"
                  size="icon"
                  className="text-muted-foreground hover:text-destructive size-8 shrink-0"
                  aria-label={`Remove ${method.brand} ending in ${method.last4}`}
                  onClick={(e) => {
                    e.stopPropagation()
                    remove(method.id)
                  }}
                >
                  <Trash2 className="size-4" />
                </Button>
              </div>
            )
          })
        )}
      </CardContent>
      <CardFooter>
        <Button variant="outline" className="w-full">
          <Plus className="size-4" />
          Add payment method
        </Button>
      </CardFooter>
    </Card>
  )
}

demo.tsx
import { Payment2 } from "@/components/ui/payment-2";

export default function Demo() {
  return (
    <div className="flex w-full items-center justify-center p-6">
      <div className="w-full max-w-md">
        <Payment2 />
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge button card empty
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
