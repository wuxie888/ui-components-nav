<!-- Changelog Accordion · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-accordion-10
     license: no-license · category: accordion
     A changelog block that lists versioned software releases in a collapsible accordion, each entry tagging its changes as new, improved, fixed, or breaking. -->

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
components/ui/v-accordion-10.tsx
//biome-ignore-all lint/suspicious/noArrayIndexKey: <>

import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/registry/default/ui/accordion";
import { Badge } from "@/registry/default/ui/badge";

type ChangeType = "new" | "improved" | "fixed" | "breaking";

const changeVariant: Record<
  ChangeType,
  "default" | "info" | "success" | "destructive"
> = {
  breaking: "destructive",
  fixed: "success",
  improved: "info",
  new: "default",
};

const releases = [
  {
    changes: [
      {
        label: "new",
        text: "Introduced real-time collaboration with live cursors",
      },
      { label: "new", text: "Added webhook support for all resource events" },
      {
        label: "improved",
        text: "Dashboard load time reduced by 60% with incremental rendering",
      },
      {
        label: "fixed",
        text: "Resolved token refresh race condition on simultaneous requests",
      },
    ],
    date: "May 22, 2025",
    isLatest: true,
    value: "v3-2-0",
    version: "v3.2.0",
  },
  {
    changes: [
      {
        label: "improved",
        text: "Overhauled settings UI for better discoverability",
      },
      {
        label: "improved",
        text: "CSV export now supports custom column ordering",
      },
      {
        label: "fixed",
        text: "Fixed pagination offset bug on filtered table views",
      },
      {
        label: "fixed",
        text: "Corrected timezone handling in scheduled reports",
      },
    ],
    date: "Apr 10, 2025",
    isLatest: false,
    value: "v3-1-2",
    version: "v3.1.2",
  },
  {
    changes: [
      {
        label: "breaking",
        text: "Removed legacy /v1/users endpoint — migrate to /v2/users",
      },
      {
        label: "new",
        text: "Role-based access control with custom permission sets",
      },
      {
        label: "new",
        text: "Two-factor authentication via TOTP and hardware keys",
      },
      {
        label: "improved",
        text: "API rate limit headers now returned on every response",
      },
    ],
    date: "Feb 28, 2025",
    isLatest: false,
    value: "v3-0-0",
    version: "v3.0.0",
  },
  {
    changes: [
      {
        label: "fixed",
        text: "File uploads no longer fail silently on network timeout",
      },
      {
        label: "fixed",
        text: "Resolved mobile layout overflow on narrow viewports",
      },
    ],
    date: "Jan 15, 2025",
    isLatest: false,
    value: "v2-9-4",
    version: "v2.9.4",
  },
];

export function Pattern() {
  return (
    <div className="mx-auto w-full max-w-lg">
      <div className="mb-4 flex items-center justify-between">
        <h2 className="font-semibold text-sm">Changelog</h2>
        <span className="text-muted-foreground text-xs">
          {releases.length} releases
        </span>
      </div>

      <Accordion defaultValue={["v3-2-0"]} multiple>
        {releases.map((release) => (
          <AccordionItem key={release.value} value={release.value}>
            <AccordionTrigger className="py-3 hover:no-underline">
              <div className="flex items-center gap-2.5">
                <span className="font-mono font-semibold text-sm">
                  {release.version}
                </span>
                {release.isLatest && (
                  <Badge size="sm" variant="success">
                    Latest
                  </Badge>
                )}
                {release.changes.some((c) => c.label === "breaking") && (
                  <Badge size="sm" variant="destructive">
                    Breaking
                  </Badge>
                )}
                <span className="font-normal text-muted-foreground text-xs">
                  {release.date}
                </span>
              </div>
            </AccordionTrigger>
            <AccordionContent className="pb-4">
              <ul className="space-y-2">
                {release.changes.map((change, i) => (
                  <li className="flex items-start gap-2.5" key={i}>
                    <Badge
                      className="mt-px shrink-0 capitalize"
                      size="sm"
                      variant={changeVariant[change.label as ChangeType]}
                    >
                      {change.label}
                    </Badge>
                    <span className="text-sm leading-snug">{change.text}</span>
                  </li>
                ))}
              </ul>
            </AccordionContent>
          </AccordionItem>
        ))}
      </Accordion>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-accordion-10";

export default function Default() {
  return (
    <div className="flex min-h-[520px] w-full items-center justify-center p-6">
      <Pattern />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add accordion badge
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
