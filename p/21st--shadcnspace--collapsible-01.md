<!-- Collapsible Changelog · @shadcnspace · https://21st.dev/@shadcnspace/components/collapsible-01
     license: MIT · category: accordion
     An expandable release changelog with per-version collapsible panels, typed badge labels (feature, fix, breaking), and smooth open/close animations. -->

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
components/shadcn-space/collapsible/collapsible-01.tsx
"use client";

import { useState } from "react";
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from "@/components/ui/collapsible";
import { Badge } from "@/components/ui/badge";
import { ChevronDown, Rocket, Wrench, Zap, Bug } from "lucide-react";
import { cn } from "@/lib/utils";

export const title = "Collapsible Changelog";

type ChangeType = "feature" | "improvement" | "fix" | "breaking";

type Release = {
  version: string;
  date: string;
  changes: { type: ChangeType; description: string }[];
};

const releases: Release[] = [
  {
    version: "v3.2.0",
    date: "April 2025",
    changes: [
      {
        type: "feature",
        description: "Added dark mode support across all components.",
      },
      {
        type: "feature",
        description: "Introduced new Chart component with 6 chart types.",
      },
      {
        type: "improvement",
        description: "Improved keyboard navigation for Dialog and Popover.",
      },
    ],
  },
  {
    version: "v3.1.0",
    date: "March 2025",
    changes: [
      {
        type: "feature",
        description: "Released Collapsible and Accordion components.",
      },
      {
        type: "improvement",
        description: "Reduced bundle size by 18% via tree-shaking.",
      },
      {
        type: "fix",
        description: "Fixed scroll position reset on modal close.",
      },
    ],
  },
  {
    version: "v3.0.0",
    date: "February 2025",
    changes: [
      {
        type: "breaking",
        description: "Migrated to Tailwind CSS v4 — config file removed.",
      },
      {
        type: "feature",
        description: "New design system with CSS custom properties.",
      },
      {
        type: "fix",
        description: "Resolved hydration mismatch on Select component.",
      },
    ],
  },
];

const changeConfig: Record<
  ChangeType,
  { label: string; icon: React.ComponentType<{ className?: string }>; className: string }
> = {
  feature: {
    label: "Feature",
    icon: Rocket,
    className: "bg-blue-500/10 text-blue-500 border-blue-500/20",
  },
  improvement: {
    label: "Improvement",
    icon: Zap,
    className: "bg-teal-400/10 text-teal-400 border-teal-400/20",
  },
  fix: {
    label: "Fix",
    icon: Bug,
    className: "bg-orange-400/10 text-orange-400 border-orange-400/20",
  },
  breaking: {
    label: "Breaking",
    icon: Wrench,
    className: "bg-red-500/10 text-red-500 border-red-500/20",
  },
};

export default function CollapsibleChangelog() {
  const [openVersions, setOpenVersions] = useState<Set<string>>(
    new Set([releases[0].version]),
  );

  const toggle = (version: string) => {
    setOpenVersions((prev) => {
      const next = new Set(prev);
      next.has(version) ? next.delete(version) : next.add(version);
      return next;
    });
  };

  return (
    <div className="w-full max-w-lg space-y-2">
      {releases.map((release) => {
        const isOpen = openVersions.has(release.version);
        return (
          <Collapsible
            key={release.version}
            open={isOpen}
            onOpenChange={() => toggle(release.version)}
          >
            <div className="rounded-xl outline bg-background overflow-hidden">
              <CollapsibleTrigger className="w-full flex justify-between px-4 py-3 h-auto rounded-none hover:bg-muted/50">
                <div className="flex items-center gap-3">
                  <span className="font-semibold text-foreground">
                    {release.version}
                  </span>
                  <span className="text-xs text-muted-foreground">
                    {release.date}
                  </span>
                </div>
                <ChevronDown
                  className={cn(
                    "size-4 text-muted-foreground transition-transform duration-200",
                    isOpen && "rotate-180",
                  )}
                />
              </CollapsibleTrigger>
              <CollapsibleContent className="overflow-hidden data-[state=closed]:animate-collapsible-up data-[state=open]:animate-collapsible-down">
                <div className="border-t px-4 py-3 space-y-2.5">
                  {release.changes.map((change, idx) => {
                    const config = changeConfig[change.type];
                    const Icon = config.icon;
                    return (
                      <div
                        key={idx}
                        className="flex flex-col sm:flex-row items-start gap-1 sm:gap-3"
                      >
                        <Badge
                          variant="outline"
                          className={cn(
                            "mt-0.5 shrink-0 gap-1 text-xs px-2 py-0.5",
                            config.className,
                          )}
                        >
                          <Icon className="size-3" />
                          {config.label}
                        </Badge>
                        <p className="text-sm text-muted-foreground leading-relaxed">
                          {change.description}
                        </p>
                      </div>
                    );
                  })}
                </div>
              </CollapsibleContent>
            </div>
          </Collapsible>
        );
      })}
    </div>
  );
}

demo.tsx
import CollapsibleChangelog from "@/components/ui/collapsible-01";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <CollapsibleChangelog />
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
npx shadcn@latest add badge collapsible
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
