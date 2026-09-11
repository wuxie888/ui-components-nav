<!-- Feature Gate Tooltip · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-tooltip-12
     license: MIT · category: tooltip
     A tooltip toolbar of icon buttons where locked Pro features show a rich tooltip with a Pro badge, description, and upgrade link. -->

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
components/ui/v-tooltip-12.tsx
import {
  ArrowRightIcon,
  CrownIcon,
  ImageIcon,
  PaletteIcon,
  PlugIcon,
  SparklesIcon,
  ZapIcon,
} from "lucide-react";
import { Badge } from "@/registry/default/ui/badge";
import { Button } from "@/registry/default/ui/button";
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/registry/default/ui/tooltip";

type Feature = {
  icon: React.ElementType;
  label: string;
  description: string;
  pro: boolean;
};

const features: Feature[] = [
  {
    description: "Automate repetitive tasks",
    icon: ZapIcon,
    label: "Automations",
    pro: false,
  },
  {
    description: "Browse and insert media",
    icon: ImageIcon,
    label: "Media Library",
    pro: false,
  },
  {
    description: "Build custom color themes",
    icon: PaletteIcon,
    label: "Custom Themes",
    pro: true,
  },
  {
    description: "Connect third-party tools",
    icon: PlugIcon,
    label: "Integrations",
    pro: true,
  },
  {
    description: "Generate content with AI",
    icon: SparklesIcon,
    label: "AI Assist",
    pro: true,
  },
];

export function Pattern() {
  return (
    <div className="flex min-h-25 items-center justify-center">
      <TooltipProvider>
        <div className="flex items-center gap-1 rounded-lg border bg-background p-1 shadow-xs">
          {features.map(({ icon: Icon, label, description, pro }) => (
            <Tooltip key={label}>
              <TooltipTrigger
                render={
                  <Button
                    aria-label={label}
                    className={pro ? "opacity-40" : ""}
                    size="icon"
                    variant="ghost"
                  />
                }
              >
                <Icon aria-hidden="true" className="size-4" />
              </TooltipTrigger>
              <TooltipContent className={pro ? "p-3" : undefined}>
                {pro ? (
                  <div className="space-y-1.5">
                    <div className="flex items-center gap-2">
                      <span className="font-medium">{label}</span>
                      <Badge className="gap-1" size="sm" variant="outline">
                        <CrownIcon aria-hidden="true" className="size-2.5" />
                        Pro
                      </Badge>
                    </div>
                    <p className="text-muted-foreground">{description}.</p>
                    <a
                      className="flex items-center gap-1 font-medium text-primary underline-offset-2 hover:underline"
                      href="#"
                    >
                      Upgrade to Pro
                      <ArrowRightIcon aria-hidden="true" className="size-3" />
                    </a>
                  </div>
                ) : (
                  label
                )}
              </TooltipContent>
            </Tooltip>
          ))}
        </div>
      </TooltipProvider>
    </div>
  );
}

demo.tsx
import {
  ArrowRightIcon,
  CrownIcon,
  ImageIcon,
  PaletteIcon,
  PlugIcon,
  SparklesIcon,
  ZapIcon,
} from "lucide-react";
import { Badge } from "@/components/ui/v-tooltip-12-utils/badge";
import { Button } from "@/components/ui/v-tooltip-12-utils/button";
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/components/ui/v-tooltip-12-utils/tooltip";

type Feature = {
  icon: React.ElementType;
  label: string;
  description: string;
  pro: boolean;
};

const features: Feature[] = [
  {
    description: "Automate repetitive tasks",
    icon: ZapIcon,
    label: "Automations",
    pro: false,
  },
  {
    description: "Browse and insert media",
    icon: ImageIcon,
    label: "Media Library",
    pro: false,
  },
  {
    description: "Build custom color themes",
    icon: PaletteIcon,
    label: "Custom Themes",
    pro: true,
  },
  {
    description: "Connect third-party tools",
    icon: PlugIcon,
    label: "Integrations",
    pro: true,
  },
  {
    description: "Generate content with AI",
    icon: SparklesIcon,
    label: "AI Assist",
    pro: true,
  },
];

export default function FeatureGateTooltipDemo() {
  return (
    <div className="flex min-h-40 items-center justify-center">
      <TooltipProvider>
        <div className="flex items-center gap-1 rounded-lg border bg-background p-1 shadow-xs">
          {features.map(({ icon: Icon, label, description, pro }) => (
            <Tooltip key={label} defaultOpen={label === "Custom Themes"}>
              <TooltipTrigger
                render={
                  <Button
                    aria-label={label}
                    className={pro ? "opacity-40" : ""}
                    size="icon"
                    variant="ghost"
                  />
                }
              >
                <Icon aria-hidden="true" className="size-4" />
              </TooltipTrigger>
              <TooltipContent className={pro ? "p-3" : undefined}>
                {pro ? (
                  <div className="space-y-1.5">
                    <div className="flex items-center gap-2">
                      <span className="font-medium">{label}</span>
                      <Badge className="gap-1" size="sm" variant="outline">
                        <CrownIcon aria-hidden="true" className="size-2.5" />
                        Pro
                      </Badge>
                    </div>
                    <p className="text-muted-foreground">{description}.</p>
                    <a
                      className="flex items-center gap-1 font-medium text-primary underline-offset-2 hover:underline"
                      href="#"
                    >
                      Upgrade to Pro
                      <ArrowRightIcon aria-hidden="true" className="size-3" />
                    </a>
                  </div>
                ) : (
                  label
                )}
              </TooltipContent>
            </Tooltip>
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
npx shadcn@latest add badge button tooltip
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
