<!-- Expandable Content Card · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-card-11
     license: MIT · category: card
     A billing usage card that expands and collapses its content with a smooth max-height transition, a fade-out gradient overlay when collapsed, and a floating chevron toggle button. -->

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
components/ui/v-card-11.tsx
"use client";

import { ChevronDownIcon } from "lucide-react";
import { useState } from "react";
import { cn } from "@/lib/utils";
import { Button } from "@/registry/default/ui/button";
import {
  Card,
  CardAction,
  CardContent,
  CardHeader,
  CardTitle,
} from "@/registry/default/ui/card";
import { Progress } from "@/registry/default/ui/progress";

export function Pattern() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <Card className="relative w-full max-w-md gap-6 overflow-visible pb-1">
      <CardHeader className="flex items-center justify-between">
        <CardTitle>3 days remaining in cycle</CardTitle>
        <CardAction>
          <Button size="sm" variant="outline">
            Billing
          </Button>
        </CardAction>
      </CardHeader>
      <CardContent
        className={cn(
          "relative space-y-5 overflow-hidden transition-all duration-500 ease-in-out",
          isOpen ? "max-h-125" : "max-h-48",
        )}
      >
        {/* Usage Details */}
        <div className="space-y-3 rounded-lg bg-muted/60 p-4">
          <div className="flex justify-between font-medium text-muted-foreground text-xs">
            <span>Included Credit</span>
            <span>On-Demand Charges</span>
          </div>
          <div className="flex justify-between font-bold text-lg">
            <span>$18.08 / $20</span>
            <span>$0</span>
          </div>
          <Progress className="h-2" value={90} />
        </div>

        {/* Additional Usage Details */}
        <div className="flex flex-col gap-4">
          <div className="flex justify-between text-sm">
            <span className="font-medium text-foreground">Requests</span>
            <span className="text-muted-foreground">$210.84</span>
          </div>
          <div className="flex justify-between text-sm">
            <span className="font-medium text-foreground">Active CPU</span>
            <span className="text-muted-foreground">$21.95</span>
          </div>
          <div className="flex justify-between text-sm">
            <span className="font-medium text-foreground">Events</span>
            <span className="text-muted-foreground">$21.20</span>
          </div>
          <div className="flex justify-between text-sm">
            <span className="font-medium text-foreground">Storage Usage</span>
            <span className="text-muted-foreground">$20.45</span>
          </div>
          <div className="flex justify-between text-sm">
            <span className="font-medium text-foreground">Bandwidth</span>
            <span className="text-muted-foreground">$0.00</span>
          </div>
        </div>

        {/* Faded background effect for collapsed state */}
        <div
          className={cn(
            "pointer-events-none absolute inset-x-0 bottom-0 h-20 rounded-b-lg bg-linear-to-t from-background to-transparent transition-opacity duration-300",
            isOpen ? "opacity-0" : "opacity-100",
          )}
        />
      </CardContent>

      {/* Toggle button */}
      <div className="absolute -bottom-4 left-1/2 -translate-x-1/2">
        <Button
          className="rounded-full bg-background shadow-sm hover:bg-background"
          onClick={() => setIsOpen(!isOpen)}
          size="icon-sm"
          variant="outline"
        >
          <ChevronDownIcon
            aria-hidden="true"
            className={cn(
              "transition-transform duration-300",
              isOpen && "rotate-180",
            )}
          />
          <span className="sr-only">Toggle card</span>
        </Button>
      </div>
    </Card>
  );
}

demo.tsx
import Component from "@/components/ui/v-card-11";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Component />
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
npx shadcn@latest add button card progress
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
