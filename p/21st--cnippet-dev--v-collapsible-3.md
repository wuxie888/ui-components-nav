<!-- Collapsible Animated Card · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-collapsible-3
     license: MIT · category: faq
     A card containing a collapsible FAQ item that smoothly expands and collapses its answer when the header is clicked. -->

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
components/ui/v-collapsible-3.tsx
import { ChevronDownIcon } from "lucide-react";
import { Card, CardContent } from "@/registry/default/ui/card";
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from "@/registry/default/ui/collapsible";

export function Pattern() {
  return (
    <div className="h-40 w-full max-w-xs">
      <Card className="py-0">
        <CardContent className="px-3">
          <Collapsible>
            <CollapsibleTrigger className="flex w-full cursor-pointer items-center justify-between gap-4 text-sm">
              <span>How do I reset my password?</span>
              <ChevronDownIcon
                aria-hidden="true"
                className="size-4 shrink-0 in-data-[state=open]:rotate-180 text-muted-foreground transition-transform"
              />
            </CollapsibleTrigger>
            <CollapsibleContent className="data-[state=closed]:animate-collapsible-up data-[state=open]:animate-collapsible-down">
              <div className="pt-3 text-muted-foreground text-sm">
                You can reset your password by clicking the &quot;Forgot
                Password&quot; link on the login page. We&apos;ll send you an
                email with instructions to create a new password.
              </div>
            </CollapsibleContent>
          </Collapsible>
        </CardContent>
      </Card>
    </div>
  );
}

demo.tsx
import { ChevronDownIcon } from "lucide-react";
import { Card, CardContent } from "@/components/ui/v-collapsible-3-utils/card";
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from "@/components/ui/v-collapsible-3-utils/collapsible";

export default function Default() {
  return (
    <div className="flex min-h-60 w-full items-center justify-center p-6">
      <div className="h-40 w-full max-w-xs">
        <Card className="py-0">
          <CardContent className="px-3">
            <Collapsible defaultOpen>
              <CollapsibleTrigger className="flex w-full cursor-pointer items-center justify-between gap-4 text-sm">
                <span>How do I reset my password?</span>
                <ChevronDownIcon
                  aria-hidden="true"
                  className="size-4 shrink-0 in-data-[state=open]:rotate-180 text-muted-foreground transition-transform"
                />
              </CollapsibleTrigger>
              <CollapsibleContent className="data-[state=closed]:animate-collapsible-up data-[state=open]:animate-collapsible-down">
                <div className="pt-3 text-muted-foreground text-sm">
                  You can reset your password by clicking the &quot;Forgot
                  Password&quot; link on the login page. We&apos;ll send you an
                  email with instructions to create a new password.
                </div>
              </CollapsibleContent>
            </Collapsible>
          </CardContent>
        </Card>
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
npx shadcn@latest add card collapsible
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
