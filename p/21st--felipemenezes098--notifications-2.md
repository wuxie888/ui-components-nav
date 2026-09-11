<!-- Grouped Notification Toggles · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/notifications-2
     license: MIT · category: notification
     A notification settings card that groups toggle switches by category like security, social, and billing. -->

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
components/ui/notifications-2.tsx
'use client'

import { useState } from 'react'
import { Bell, CreditCard, ShieldCheck, Users } from 'lucide-react'

import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'
import { Label } from '@/components/ui/label'
import { Separator } from '@/components/ui/separator'
import { Switch } from '@/components/ui/switch'

type Toggle = { id: string; label: string; enabled: boolean }

type Group = {
  id: string
  title: string
  icon: typeof Bell
  toggles: Toggle[]
}

const GROUPS: Group[] = [
  {
    id: 'security',
    title: 'Security',
    icon: ShieldCheck,
    toggles: [
      { id: 'sec-signin', label: 'New sign-in alerts', enabled: true },
      { id: 'sec-password', label: 'Password changes', enabled: true },
    ],
  },
  {
    id: 'social',
    title: 'Social',
    icon: Users,
    toggles: [
      { id: 'soc-mentions', label: 'Mentions and replies', enabled: true },
      { id: 'soc-follows', label: 'New followers', enabled: false },
    ],
  },
  {
    id: 'billing',
    title: 'Billing',
    icon: CreditCard,
    toggles: [
      { id: 'bil-invoices', label: 'Invoices and receipts', enabled: true },
      { id: 'bil-failures', label: 'Failed payments', enabled: true },
    ],
  },
]

export function Notifications2() {
  const [state, setState] = useState<Record<string, boolean>>(() =>
    Object.fromEntries(
      GROUPS.flatMap((group) =>
        group.toggles.map((toggle) => [toggle.id, toggle.enabled]),
      ),
    ),
  )

  return (
    <Card>
      <CardHeader>
        <CardTitle>Notifications</CardTitle>
      </CardHeader>
      <CardContent className="flex flex-col gap-6">
        {GROUPS.map((group) => {
          const Icon = group.icon
          return (
            <div key={group.id} className="flex flex-col gap-3">
              <div className="text-muted-foreground flex items-center gap-2 text-xs font-medium tracking-wide uppercase">
                <Icon className="size-4" />
                {group.title}
              </div>
              {group.toggles.map((toggle, index) => (
                <div key={toggle.id} className="flex flex-col gap-3">
                  {index > 0 && <Separator />}
                  <div className="flex items-center justify-between">
                    <Label htmlFor={toggle.id} className="font-normal">
                      {toggle.label}
                    </Label>
                    <Switch
                      id={toggle.id}
                      checked={state[toggle.id]}
                      onCheckedChange={(v) =>
                        setState((prev) => ({ ...prev, [toggle.id]: v }))
                      }
                    />
                  </div>
                </div>
              ))}
            </div>
          )
        })}
      </CardContent>
    </Card>
  )
}

demo.tsx
import { Notifications2 } from "@/components/ui/notifications-2";

export default function Demo() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center bg-background p-6">
      <div className="w-full max-w-sm">
        <Notifications2 />
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
npx shadcn@latest add card label separator switch
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
