<!-- Pricing Plan Card · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-card-18
     license: no-license · category: pricing-section
     A pricing plan card with a feature checklist, a highlighted ring, a "Most Popular" badge, and a call-to-action button. -->

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
components/ui/v-card-18.tsx
import { CheckIcon } from "lucide-react";
import { Badge } from "@/registry/default/ui/badge";
import { Button } from "@/registry/default/ui/button";
import { Card, CardContent } from "@/registry/default/ui/card";
import { Separator } from "@/registry/default/ui/separator";

const plan = {
  badge: "Most Popular",
  cta: "Start free trial",
  features: [
    "Unlimited components",
    "Team collaboration",
    "Priority support",
    "Custom themes",
    "CLI access",
  ],
  name: "Pro",
  period: "/ month",
  price: "$12",
};

export function Pattern() {
  return (
    <Card className="w-full max-w-xs ring-2 ring-primary">
      <CardContent className="flex flex-col gap-4">
        <div className="flex items-center justify-between">
          <span className="font-semibold text-sm">{plan.name}</span>
          <Badge size="sm">{plan.badge}</Badge>
        </div>
        <div className="flex items-baseline gap-1">
          <span className="font-bold text-3xl">{plan.price}</span>
          <span className="text-muted-foreground text-sm">{plan.period}</span>
        </div>
        <Separator />
        <ul className="flex flex-col gap-2">
          {plan.features.map((f) => (
            <li className="flex items-center gap-2 text-sm" key={f}>
              <CheckIcon className="size-4 shrink-0 text-emerald-500" />
              {f}
            </li>
          ))}
        </ul>
        <Button className="w-full">{plan.cta}</Button>
      </CardContent>
    </Card>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-card-18";

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
npx shadcn@latest add badge button card separator
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
