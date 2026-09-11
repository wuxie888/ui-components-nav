<!-- Skill Level Meters · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-meter-6
     license: MIT · category: stat
     A stacked list of labeled meter bars that visualize skill levels as animated percentage fills. -->

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
components/ui/v-meter-6.tsx
import {
  Meter,
  MeterIndicator,
  MeterLabel,
  MeterTrack,
  MeterValue,
} from "@/registry/default/ui/meter";

const skills = [
  { label: "TypeScript", value: 90 },
  { label: "React", value: 85 },
  { label: "Node.js", value: 72 },
  { label: "Python", value: 60 },
  { label: "Go", value: 38 },
];

export default function Particle() {
  return (
    <div className="w-full max-w-sm space-y-3">
      {skills.map(({ label, value }) => (
        <Meter key={label} value={value}>
          <div className="flex items-center justify-between gap-2">
            <MeterLabel>{label}</MeterLabel>
            <MeterValue />
          </div>
          <MeterTrack className="h-1.5">
            <MeterIndicator />
          </MeterTrack>
        </Meter>
      ))}
    </div>
  );
}

demo.tsx
import Component from "@/components/ui/v-meter-6";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center bg-background p-10 text-foreground">
      <Component />
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
