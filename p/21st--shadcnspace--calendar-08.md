<!-- Calendar With Event List · @shadcnspace · https://21st.dev/@shadcnspace/components/calendar-08
     license: MIT · category: date-picker
     A date picker calendar in a card with a color-coded daily event list showing meeting titles and time ranges. -->

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
components/shadcn-space/calendar/calendar-08.tsx
'use client'

import { useState } from 'react'
import { Button } from '@/components/ui/button'
import { Calendar } from '@/components/ui/calendar'
import { Card, CardContent, CardFooter } from '@/components/ui/card'
import { PlusIcon } from 'lucide-react'

const formatTimeRange = (from: Date, to: Date): string => {
  const timeFmt = new Intl.DateTimeFormat('en-US', { hour: 'numeric', minute: '2-digit', hour12: true })
  const dateFmt = new Intl.DateTimeFormat('en-US', { month: 'short', day: 'numeric' })
  if (from.toDateString() === to.toDateString()) {
    return `${timeFmt.format(from)} – ${timeFmt.format(to)}`
  }
  return `${dateFmt.format(from)} – ${dateFmt.format(to)}`
}

const colorMap = {
  blue: { dot: 'bg-blue-500', bg: 'bg-blue-500/10', time: 'text-blue-500' },
  teal: { dot: 'bg-teal-400', bg: 'bg-teal-400/10', time: 'text-teal-400' },
  orange: { dot: 'bg-orange-400', bg: 'bg-orange-400/10', time: 'text-orange-400' }
} as const

const events = [
  { title: 'Product Standup', from: '2026-05-28T08:30:00', to: '2026-05-28T09:00:00', color: 'blue' as const },
  { title: 'UX Walkthrough', from: '2026-05-28T11:00:00', to: '2026-05-28T12:00:00', color: 'teal' as const },
  { title: 'Stakeholder Demo', from: '2026-05-28T15:00:00', to: '2026-05-28T16:30:00', color: 'orange' as const }
]

const WithEventListDemo = () => {
  const [date, setDate] = useState<Date | undefined>(new Date())

  return (
    <>
      <Card className='min-w-2xs pt-4'>
        <CardContent className='px-4'>
          <Calendar mode='single' selected={date} onSelect={setDate} className='w-full bg-transparent p-0' required />
        </CardContent>
        <CardFooter className='flex flex-col items-start gap-3 border-t bg-card'>
          <div className='flex w-full items-center justify-between px-1'>
            <div className='text-sm font-medium'>
              {date?.toLocaleDateString('en-US', {
                day: 'numeric',
                month: 'long',
                year: 'numeric'
              })}
            </div>
            <Button variant='ghost' size='icon' className='size-6' title='Add Event'>
              <PlusIcon />
              <span className='sr-only'>Add Event</span>
            </Button>
          </div>
          <div className='flex w-full flex-col gap-2'>
            {events.map(event => {
              const c = colorMap[event.color]
              return (
                <div
                  key={event.title}
                  className={`flex items-center gap-3 rounded-lg px-3 py-2 transition-opacity hover:opacity-80 ${c.bg}`}
                >
                  <span className={`size-1.5 shrink-0 rounded-full ${c.dot}`} />
                  <span className='flex-1 truncate text-sm font-medium'>{event.title}</span>
                  <span className={`shrink-0 text-xs font-medium ${c.time}`}>
                    {formatTimeRange(new Date(event.from), new Date(event.to))}
                  </span>
                </div>
              )
            })}
          </div>
        </CardFooter>
      </Card>
    </>
  )
}

export default WithEventListDemo

demo.tsx
import Calendar08 from "@/components/ui/calendar-08";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Calendar08 />
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
npx shadcn@latest add button calendar card
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
