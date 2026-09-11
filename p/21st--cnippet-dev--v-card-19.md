<!-- Event RSVP Card · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-card-19
     license: no-license · category: calendar
     An event card showing the title, category badge, date, time, location and attendee count with a reserve/RSVP button. -->

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
components/ui/v-card-19.tsx
import { CalendarIcon, ClockIcon, MapPinIcon, UsersIcon } from "lucide-react";
import { Badge } from "@/registry/default/ui/badge";
import { Button } from "@/registry/default/ui/button";
import { Card, CardContent } from "@/registry/default/ui/card";

const event = {
  attendees: 42,
  category: "Workshop",
  date: "Jun 18, 2026",
  location: "Design Hub, Floor 3",
  spots: 10,
  time: "2:00 PM – 4:30 PM",
  title: "Design Systems in Practice",
};

export function Pattern() {
  return (
    <Card className="w-full max-w-xs">
      <CardContent className="flex flex-col gap-4">
        <div className="flex items-start justify-between gap-2">
          <h3 className="font-semibold text-sm leading-snug">{event.title}</h3>
          <Badge size="sm" variant="secondary">
            {event.category}
          </Badge>
        </div>
        <div className="flex flex-col gap-2 text-muted-foreground text-xs">
          <span className="flex items-center gap-2">
            <CalendarIcon className="size-3.5 shrink-0" />
            {event.date}
          </span>
          <span className="flex items-center gap-2">
            <ClockIcon className="size-3.5 shrink-0" />
            {event.time}
          </span>
          <span className="flex items-center gap-2">
            <MapPinIcon className="size-3.5 shrink-0" />
            {event.location}
          </span>
          <span className="flex items-center gap-2">
            <UsersIcon className="size-3.5 shrink-0" />
            {event.attendees} attending ·{" "}
            <span className="text-amber-600 dark:text-amber-400">
              {event.spots} spots left
            </span>
          </span>
        </div>
        <Button className="w-full" size="sm">
          Reserve a Spot
        </Button>
      </CardContent>
    </Card>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-card-19";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Pattern />
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
npx shadcn@latest add badge button card
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
