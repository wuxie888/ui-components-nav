<!-- Notification Channel Matrix · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/notifications-1
     license: agpl-3.0 · category: notification
     A settings card with a switch matrix table for toggling each notification type across email, push, and SMS channels. -->

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
components/ui/notifications-1.tsx
'use client'

import { useState } from 'react'

import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'
import { Switch } from '@/components/ui/switch'
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from '@/components/ui/table'

type Channel = 'email' | 'push' | 'sms'

type Row = {
  id: string
  label: string
  hint: string
  channels: Record<Channel, boolean>
}

const INITIAL: Row[] = [
  {
    id: 'security',
    label: 'Security alerts',
    hint: 'New sign-ins and password changes',
    channels: { email: true, push: true, sms: true },
  },
  {
    id: 'mentions',
    label: 'Mentions and replies',
    hint: 'When someone tags you',
    channels: { email: true, push: true, sms: false },
  },
  {
    id: 'billing',
    label: 'Billing and receipts',
    hint: 'Invoices and payment issues',
    channels: { email: true, push: false, sms: false },
  },
  {
    id: 'product',
    label: 'Product updates',
    hint: 'New features and announcements',
    channels: { email: false, push: false, sms: false },
  },
]

const CHANNELS: { key: Channel; label: string }[] = [
  { key: 'email', label: 'Email' },
  { key: 'push', label: 'Push' },
  { key: 'sms', label: 'SMS' },
]

export function Notifications1() {
  const [rows, setRows] = useState<Row[]>(INITIAL)

  const toggle = (id: string, channel: Channel) =>
    setRows((prev) =>
      prev.map((row) =>
        row.id === id
          ? {
              ...row,
              channels: {
                ...row.channels,
                [channel]: !row.channels[channel],
              },
            }
          : row,
      ),
    )

  return (
    <Card>
      <CardHeader>
        <CardTitle>Notification channels</CardTitle>
      </CardHeader>
      <CardContent>
        <Table>
          <TableHeader>
            <TableRow>
              <TableHead>Notification</TableHead>
              {CHANNELS.map((channel) => (
                <TableHead key={channel.key} className="text-center">
                  {channel.label}
                </TableHead>
              ))}
            </TableRow>
          </TableHeader>
          <TableBody>
            {rows.map((row) => (
              <TableRow key={row.id}>
                <TableCell>
                  <div className="flex flex-col">
                    <span className="font-medium">{row.label}</span>
                    <span className="text-muted-foreground text-xs">
                      {row.hint}
                    </span>
                  </div>
                </TableCell>
                {CHANNELS.map((channel) => (
                  <TableCell key={channel.key} className="text-center">
                    <Switch
                      checked={row.channels[channel.key]}
                      onCheckedChange={() => toggle(row.id, channel.key)}
                      aria-label={`${row.label} via ${channel.label}`}
                    />
                  </TableCell>
                ))}
              </TableRow>
            ))}
          </TableBody>
        </Table>
      </CardContent>
    </Card>
  )
}

demo.tsx
import { Notifications1 } from "@/components/ui/notifications-1";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center bg-background p-6">
      <div className="w-full max-w-2xl">
        <Notifications1 />
      </div>
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add card switch table
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
