<!-- Form Fields with Tooltip Hints · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-tooltip-13
     license: MIT · category: form
     A vertical form with labeled inputs where each field label has an info icon that reveals validation hints in a tooltip on hover. -->

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
components/ui/v-tooltip-13.tsx
import { InfoIcon } from "lucide-react";
import { Input } from "@/registry/default/ui/input";
import { Label } from "@/registry/default/ui/label";
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/registry/default/ui/tooltip";

type Field = {
  id: string;
  label: string;
  placeholder: string;
  hint: string;
};

const fields: Field[] = [
  {
    hint: "3–20 characters, letters, numbers, and underscores only.",
    id: "username",
    label: "Username",
    placeholder: "john_doe",
  },
  {
    hint: "Must be 8+ chars with at least one uppercase letter and one number.",
    id: "password",
    label: "Password",
    placeholder: "••••••••",
  },
  {
    hint: "We only use this for account recovery — no marketing emails.",
    id: "email",
    label: "Email",
    placeholder: "you@example.com",
  },
];

export function Pattern() {
  return (
    <div className="w-full max-w-sm space-y-4">
      <TooltipProvider>
        {fields.map(({ id, label, placeholder, hint }) => (
          <div className="space-y-1.5" key={id}>
            <div className="flex items-center gap-1.5">
              <Label htmlFor={id}>{label}</Label>
              <Tooltip>
                <TooltipTrigger
                  aria-label={`${label} format requirements`}
                  className="inline-flex cursor-default text-muted-foreground hover:text-foreground"
                >
                  <InfoIcon className="size-3.5" />
                </TooltipTrigger>
                <TooltipContent className="max-w-52">{hint}</TooltipContent>
              </Tooltip>
            </div>
            <Input id={id} placeholder={placeholder} />
          </div>
        ))}
      </TooltipProvider>
    </div>
  );
}

demo.tsx
import { InfoIcon } from "lucide-react";
import { Input } from "@/components/ui/v-tooltip-13-utils/input";
import { Label } from "@/components/ui/v-tooltip-13-utils/label";
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/components/ui/v-tooltip-13-utils/tooltip";

type Field = {
  id: string;
  label: string;
  placeholder: string;
  hint: string;
  defaultOpen?: boolean;
};

const fields: Field[] = [
  {
    hint: "3–20 characters, letters, numbers, and underscores only.",
    id: "username",
    label: "Username",
    placeholder: "john_doe",
    defaultOpen: true,
  },
  {
    hint: "Must be 8+ chars with at least one uppercase letter and one number.",
    id: "password",
    label: "Password",
    placeholder: "••••••••",
  },
  {
    hint: "We only use this for account recovery — no marketing emails.",
    id: "email",
    label: "Email",
    placeholder: "you@example.com",
  },
];

function Pattern() {
  return (
    <div className="w-full max-w-sm space-y-4">
      <TooltipProvider>
        {fields.map(({ id, label, placeholder, hint, defaultOpen }) => (
          <div className="space-y-1.5" key={id}>
            <div className="flex items-center gap-1.5">
              <Label htmlFor={id}>{label}</Label>
              <Tooltip defaultOpen={defaultOpen}>
                <TooltipTrigger
                  aria-label={`${label} format requirements`}
                  className="inline-flex cursor-default text-muted-foreground hover:text-foreground"
                >
                  <InfoIcon className="size-3.5" />
                </TooltipTrigger>
                <TooltipContent className="max-w-52">{hint}</TooltipContent>
              </Tooltip>
            </div>
            <Input id={id} placeholder={placeholder} />
          </div>
        ))}
      </TooltipProvider>
    </div>
  );
}

export default function Default() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center p-6">
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
npx shadcn@latest add input label tooltip
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
