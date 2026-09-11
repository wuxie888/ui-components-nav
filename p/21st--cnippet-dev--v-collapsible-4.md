<!-- Collapsible Billing Card · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-collapsible-4
     license: no-license · category: progress
     A billing usage card with a progress bar that expands via a floating bottom-center trigger to reveal a detailed cost breakdown. -->

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
components/ui/v-collapsible-4.tsx
import { ChevronDownIcon } from "lucide-react";
import { Button } from "@/registry/default/ui/button";
import {
  Card,
  CardAction,
  CardContent,
  CardHeader,
  CardTitle,
} from "@/registry/default/ui/card";
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from "@/registry/default/ui/collapsible";
import { Progress } from "@/registry/default/ui/progress";

export function Pattern() {
  return (
    <div className="h-72 w-full max-w-xs">
      <Collapsible className="relative">
        <Card>
          <CardHeader className="flex items-center justify-between">
            <CardTitle className="text-sm">3 days remaining in cycle</CardTitle>
            <CardAction>
              <Button size="sm" variant="outline">
                Billing
              </Button>
            </CardAction>
          </CardHeader>
          <CardContent className="space-y-4">
            <div className="space-y-2 rounded-lg border border-border bg-muted/60 p-3">
              <div className="flex justify-between font-medium text-sm">
                <span>$18.08 / $20</span>
                <span>$200</span>
              </div>
              <Progress
                className="**:data-[slot=progress-track]:h-1.5 **:data-[slot=progress-track]:bg-primary/20"
                value={90}
              />
            </div>

            <CollapsibleContent className="data-[state=closed]:animate-collapsible-up data-[state=open]:animate-collapsible-down">
              <div className="flex flex-col gap-2.5 pt-2">
                {[
                  { label: "Requests", value: "$210.84" },
                  { label: "Active CPU", value: "$21.95" },
                  { label: "Events", value: "$21.20" },
                  { label: "Storage Usage", value: "$20.45" },
                ].map((item) => (
                  <div
                    className="flex justify-between text-xs"
                    key={item.label}
                  >
                    <span className="font-medium text-muted-foreground">
                      {item.label}
                    </span>
                    <span className="font-medium">{item.value}</span>
                  </div>
                ))}
              </div>
            </CollapsibleContent>
          </CardContent>
        </Card>

        <div className="absolute -bottom-3.5 left-1/2 -translate-x-1/2">
          <CollapsibleTrigger
            render={
              <Button
                className="rounded-full bg-background! shadow-sm"
                size="icon-sm"
                variant="outline"
              />
            }
          >
            <ChevronDownIcon
              aria-hidden="true"
              className="size-3.5 in-data-panel-open:rotate-180 transition-transform"
            />
            <span className="sr-only">Toggle details</span>
          </CollapsibleTrigger>
        </div>
      </Collapsible>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-collapsible-4";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center bg-background p-10 text-foreground">
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
npx shadcn@latest add button card collapsible progress
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
