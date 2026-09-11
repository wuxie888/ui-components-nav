<!-- Right Side Navigation Calendar · @shadcnspace · https://21st.dev/@shadcnspace/components/calendar-10
     license: MIT · category: navigation-menu
     A single-date calendar with month navigation arrows positioned on the right side and left-aligned caption. -->

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
components/shadcn-space/calendar/calendar-10.tsx
'use client'

import { useState } from 'react'
import { type ChevronProps } from 'react-day-picker'
import { Calendar } from '@/components/ui/calendar'
import { ArrowLeftIcon, ArrowRightIcon } from 'lucide-react'

const RightSideNavigationDemo = () => {
  const [date, setDate] = useState<Date | undefined>(new Date())

  return (
    <>
      <Calendar
        mode='single'
        selected={date}
        defaultMonth={date}
        onSelect={setDate}
        className='rounded-md border'
        classNames={{
          month_caption: 'flex items-center h-8 justify-start',
          nav: 'flex justify-end absolute w-full items-center'
        }}
        components={{
          Chevron: ({ orientation }: ChevronProps) => {
            if (orientation === 'left') return <ArrowLeftIcon className='size-4' />
            if (orientation === 'right') return <ArrowRightIcon className='size-4' />
            return <></>
          }
        }}
      />
    </>
  )
}

export default RightSideNavigationDemo

demo.tsx
import Calendar10 from "@/components/ui/calendar-10";

export default function Default() {
  return (
    <div className="flex justify-center p-4">
      <Calendar10 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react react-day-picker
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add calendar
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
