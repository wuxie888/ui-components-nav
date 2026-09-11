<!-- Payment Method Card Checkbox · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-checkbox-6
     license: no-license · category: checkbox
     A payment method selection card that pairs a saved card (brand icon, last digits, expiry) with a rounded checkbox to mark it as selected. -->

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
components/ui/v-checkbox-6.tsx
"use client";

import type { SVGProps } from "react";

import { Checkbox } from "@/registry/default/ui/checkbox";
import { Frame, FramePanel } from "@/registry/default/ui/frame";

const MastercardIcon = (props: SVGProps<SVGSVGElement>) => (
  <svg
    fill="none"
    viewBox="0 0 24 24"
    xmlns="http://www.w3.org/2000/svg"
    {...props}
  >
    <circle cx="7" cy="12" fill="#EB001B" r="7" />
    <circle cx="17" cy="12" fill="#F79E1B" r="7" />
    <path
      d="M12 17.5C13.5 16.2 14.5 14.2 14.5 12C14.5 9.8 13.5 7.8 12 6.5C10.5 7.8 9.5 9.8 9.5 12C9.5 14.2 10.5 16.2 12 17.5Z"
      fill="#FF5F00"
    />
  </svg>
);

export function Pattern() {
  return (
    <div className="mx-auto w-full max-w-xs">
      <Frame className="w-full">
        <FramePanel>
          <div className="flex size-10 items-center justify-center rounded-lg border border-border bg-background p-1.5 shadow-black/5 shadow-xs">
            <MastercardIcon className="size-full" />
          </div>
          <div className="flex flex-col items-start gap-0.5">
            <span className="font-medium text-sm">
              Mastercard ending in 8888
            </span>
            <span className="text-muted-foreground text-xs">Expires 09/25</span>
          </div>
        </FramePanel>
        <Checkbox
          className="absolute top-5 right-5 size-5 rounded-full"
          defaultChecked
          id="mastercard"
        />
      </Frame>
    </div>
  );
}

demo.tsx
import { Pattern } from "@/components/ui/v-checkbox-6";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-8">
      <Pattern />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add checkbox
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
