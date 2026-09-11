<!-- Dialog · @ephraimduncan · https://21st.dev/@ephraimduncan/components/dialog-1
     license: unspecified · category: dialog
     Here is dialog component -->

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
components/ui/dialog-10.tsx
'use client';

import { format } from 'date-fns';
import { CalendarIcon } from 'lucide-react';
import { useMemo, useState } from 'react';

import { Button } from '@/components/ui/button';
import { Calendar } from '@/components/ui/calendar';
import {
  Dialog,
  DialogClose,
  DialogContent,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from '@/components/ui/dialog';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from '@/components/ui/popover';
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select';
import { Textarea } from '@/components/ui/textarea';
import { cn } from '@/lib/utils';

export default function Dialog10() {
  const [date, setDate] = useState<Date | undefined>(new Date());
  const [startTime, setStartTime] = useState('09:00');

  const timeOptions = useMemo(() => {
    const options = [];
    for (let hour = 0; hour <= 23; hour++) {
      for (let minute = 0; minute < 60; minute += 30) {
        const formattedHour = hour.toString().padStart(2, '0');
        const formattedMinute = minute.toString().padStart(2, '0');
        const value = `${formattedHour}:${formattedMinute}`;
        const tempDate = new Date(2000, 0, 1, hour, minute);
        const label = format(tempDate, 'h:mm a');
        options.push({ value, label });
      }
    }

    if (!options.find((opt) => opt.value === '23:59')) {
      const endOfDay = new Date(2000, 0, 1, 23, 59);
      options.push({ value: '23:59', label: format(endOfDay, 'h:mm a') });
    }

    return options;
  }, []);
  return (
    <Dialog defaultOpen>
      <DialogTrigger render={<Button />}>Show dialog</DialogTrigger>
      <DialogContent className="gap-0 p-0 sm:max-w-lg">
        <DialogHeader className="border-b px-6 py-4 pt-5">
          <DialogTitle>Schedule a Meeting</DialogTitle>
        </DialogHeader>

        <form action="#" method="POST">
          <div className="space-y-6 p-6">
            <div className="space-y-2">
              <Label htmlFor="title">Meeting Title</Label>
              <Input
                id="title"
                name="title"
                placeholder="e.g., Project Kickoff"
              />
            </div>

            <div className="space-y-2">
              <Label htmlFor="attendees">Attendees</Label>
              <Input
                id="attendees"
                name="attendees"
                placeholder="user1@example.com, user2@example.com"
              />
              <p className="text-pretty text-muted-foreground text-xs">
                Enter email addresses separated by commas.
              </p>
            </div>

            <div className="grid grid-cols-3 gap-4">
              <div className="col-span-2 space-y-2">
                <Label htmlFor="date">Date</Label>
                <Popover>
                  <PopoverTrigger
                    render={
                      <Button
                        className={cn(
                          'w-full justify-start text-left font-normal',
                          !date && 'text-muted-foreground'
                        )}
                        id="date"
                        variant={'outline'}
                      />
                    }
                  >
                    <CalendarIcon className="mr-1 h-4 w-4 shrink-0" />{' '}
                    {date ? format(date, 'PPP') : <span>Pick a date</span>}
                  </PopoverTrigger>
                  <PopoverContent align="start" className="w-auto p-0">
                    <Calendar
                      autoFocus
                      mode="single"
                      onSelect={setDate}
                      selected={date}
                    />
                  </PopoverContent>
                </Popover>
              </div>

              <div className="space-y-2">
                <Label htmlFor="time">Time</Label>
                <Select
                  items={timeOptions}
                  onValueChange={(value) => {
                    if (value) {
                      setStartTime(value);
                    }
                  }}
                  value={startTime}
                >
                  <SelectTrigger className="w-full" id="time">
                    <SelectValue placeholder="Select time" />
                  </SelectTrigger>
                  <SelectContent>
                    {timeOptions.map((option) => (
                      <SelectItem key={option.value} value={option.value}>
                        {option.label}
                      </SelectItem>
                    ))}
                  </SelectContent>
                </Select>
              </div>
            </div>

            <div className="space-y-2">
              <Label htmlFor="location">Location / Conference Link</Label>
              <Input
                id="location"
                name="location"
                placeholder="e.g., Conference Room B or https://meet.example.com/..."
              />
            </div>

            <div className="space-y-2">
              <Label htmlFor="description">Description</Label>
              <Textarea
                className="min-h-[100px]"
                id="description"
                name="description"
                placeholder="Optional: Add an agenda or notes..."
              />
            </div>
          </div>

          <div className="flex items-center justify-end space-x-2 border-t p-4">
            <DialogClose render={<Button type="button" variant="ghost" />}>
              Cancel
            </DialogClose>
            <Button size="sm" type="submit">
              Schedule
            </Button>
          </div>
        </form>
      </DialogContent>
    </Dialog>
  );
}

demo.tsx
// demo.tsx
"use client";

import Dialog03 from "@/components/ui/dialog-1";

export default function DemoPage() {
  return (
    <div className="flex justify-center items-center h-screen bg-gray-50 dark:bg-gray-900">
      <Dialog03 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-dialog @radix-ui/react-label @radix-ui/react-slot class-variance-authority clsx date-fns lucide-react tailwind-merge
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button calendar dialog input label popover select textarea
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
