<!-- API Usage Meter Card · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-meter-8
     license: no-license · category: team
     A card that shows per-member API usage as labeled meter bars against a shared plan limit. -->

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
components/ui/v-meter-8.tsx
"use client";

import {
  Meter,
  MeterIndicator,
  MeterLabel,
  MeterTrack,
  MeterValue,
} from "@/registry/default/ui/meter";

const PLAN_LIMIT = 10000;

const seats = [
  { label: "Alice Chen", used: 8240 },
  { label: "Bob Kim", used: 3100 },
  { label: "Carol Day", used: 9870 },
  { label: "Dave Osei", used: 1500 },
];

function fmt(n: number) {
  return n >= 1000 ? `${(n / 1000).toFixed(1)}k` : String(n);
}

export default function Particle() {
  return (
    <div className="w-full max-w-sm space-y-4">
      <p className="font-medium text-sm">API usage by member</p>
      {seats.map(({ label, used }) => (
        <Meter key={label} max={PLAN_LIMIT} value={used}>
          <div className="flex items-center justify-between gap-2">
            <MeterLabel>{label}</MeterLabel>
            <MeterValue>{() => `${fmt(used)} / ${fmt(PLAN_LIMIT)}`}</MeterValue>
          </div>
          <MeterTrack>
            <MeterIndicator />
          </MeterTrack>
        </Meter>
      ))}
    </div>
  );
}

demo.tsx
import Particle from "@/components/ui/v-meter-8";

export default function Default() {
  return (
    <div className="flex min-h-[350px] w-full items-center justify-center p-8">
      <Particle />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add meter
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
