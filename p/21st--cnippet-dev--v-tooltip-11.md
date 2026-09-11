<!-- Copy to Clipboard Tooltip · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-tooltip-11
     license: MIT · category: tooltip
     A compact credentials card that copies API keys, endpoints, and webhook URLs to the clipboard, each row using a tooltip that confirms when the value was copied. -->

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
components/ui/v-tooltip-11.tsx
"use client";

import {
  CheckIcon,
  CopyIcon,
  KeyIcon,
  LinkIcon,
  WebhookIcon,
} from "lucide-react";
import { useState } from "react";
import { Button } from "@/registry/default/ui/button";
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/registry/default/ui/tooltip";

const fields = [
  { icon: KeyIcon, label: "API Key", value: "sk_live_9xKqP2mN...8rTv" },
  { icon: LinkIcon, label: "Endpoint", value: "https://api.cnippet.ui/v2" },
  {
    icon: WebhookIcon,
    label: "Webhook",
    value: "https://hooks.cnippet.ui/events",
  },
] as const;

export function Pattern() {
  const [copiedId, setCopiedId] = useState<string | null>(null);

  const copy = (value: string, id: string) => {
    navigator.clipboard.writeText(value).catch(() => null);
    setCopiedId(id);
    setTimeout(() => setCopiedId(null), 2000);
  };

  return (
    <div className="flex min-h-25 items-center justify-center">
      <TooltipProvider>
        <div className="w-72 space-y-1.5 rounded-xl border bg-background p-3 shadow-xs">
          {fields.map(({ icon: Icon, label, value }) => (
            <div
              className="flex items-center gap-2.5 rounded-lg bg-muted/50 px-3 py-2"
              key={label}
            >
              <Icon
                aria-hidden="true"
                className="size-3.5 shrink-0 text-muted-foreground"
              />
              <div className="min-w-0 flex-1">
                <p className="font-semibold text-[10px] text-muted-foreground uppercase tracking-wide">
                  {label}
                </p>
                <p className="truncate font-mono text-xs">{value}</p>
              </div>
              <Tooltip>
                <TooltipTrigger
                  className="shrink-0"
                  onClick={() => copy(value, label)}
                  render={
                    <Button
                      aria-label={
                        copiedId === label ? "Copied" : `Copy ${label}`
                      }
                      className="size-6"
                      size="icon"
                      variant="ghost"
                    />
                  }
                >
                  {copiedId === label ? (
                    <CheckIcon
                      aria-hidden="true"
                      className="size-3.5 text-emerald-500"
                    />
                  ) : (
                    <CopyIcon aria-hidden="true" className="size-3.5" />
                  )}
                </TooltipTrigger>
                <TooltipContent>
                  {copiedId === label ? "Copied!" : "Copy to clipboard"}
                </TooltipContent>
              </Tooltip>
            </div>
          ))}
        </div>
      </TooltipProvider>
    </div>
  );
}

demo.tsx
"use client";

import {
  CheckIcon,
  CopyIcon,
  KeyIcon,
  LinkIcon,
  WebhookIcon,
} from "lucide-react";
import { useState } from "react";
import { Button } from "@/components/ui/v-tooltip-11-utils/button";
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/components/ui/v-tooltip-11-utils/tooltip";

const fields = [
  { icon: KeyIcon, label: "API Key", value: "sk_live_9xKqP2mN...8rTv" },
  { icon: LinkIcon, label: "Endpoint", value: "https://api.cnippet.ui/v2" },
  {
    icon: WebhookIcon,
    label: "Webhook",
    value: "https://hooks.cnippet.ui/events",
  },
] as const;

export default function Default() {
  const [copiedId, setCopiedId] = useState<string | null>(null);

  const copy = (value: string, id: string) => {
    navigator.clipboard.writeText(value).catch(() => null);
    setCopiedId(id);
    setTimeout(() => setCopiedId(null), 2000);
  };

  return (
    <div className="flex min-h-25 items-center justify-center">
      <TooltipProvider>
        <div className="w-72 space-y-1.5 rounded-xl border bg-background p-3 shadow-xs">
          {fields.map(({ icon: Icon, label, value }, index) => (
            <div
              className="flex items-center gap-2.5 rounded-lg bg-muted/50 px-3 py-2"
              key={label}
            >
              <Icon
                aria-hidden="true"
                className="size-3.5 shrink-0 text-muted-foreground"
              />
              <div className="min-w-0 flex-1">
                <p className="font-semibold text-[10px] text-muted-foreground uppercase tracking-wide">
                  {label}
                </p>
                <p className="truncate font-mono text-xs">{value}</p>
              </div>
              <Tooltip defaultOpen={index === 0}>
                <TooltipTrigger
                  className="shrink-0"
                  onClick={() => copy(value, label)}
                  render={
                    <Button
                      aria-label={
                        copiedId === label ? "Copied" : `Copy ${label}`
                      }
                      className="size-6"
                      size="icon"
                      variant="ghost"
                    />
                  }
                >
                  {copiedId === label ? (
                    <CheckIcon
                      aria-hidden="true"
                      className="size-3.5 text-emerald-500"
                    />
                  ) : (
                    <CopyIcon aria-hidden="true" className="size-3.5" />
                  )}
                </TooltipTrigger>
                <TooltipContent>
                  {copiedId === label ? "Copied!" : "Copy to clipboard"}
                </TooltipContent>
              </Tooltip>
            </div>
          ))}
        </div>
      </TooltipProvider>
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
npx shadcn@latest add button tooltip
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
