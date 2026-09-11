<!-- Card Style Checkbox · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-checkbox-4
     license: no-license · category: notification
     A checkbox wrapped in a bordered card with a title and description, ideal for settings toggles and opt-in options that highlight when selected. -->

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
components/ui/v-checkbox-4.tsx
import { Checkbox } from "@/registry/default/ui/checkbox";
import { Label } from "@/registry/default/ui/label";

export default function Particle() {
  return (
    <Label className="flex items-start gap-2 rounded-lg border p-3 hover:bg-accent/50 has-data-checked:border-primary/48 has-data-checked:bg-accent/50">
      <Checkbox defaultChecked />
      <div className="flex flex-col gap-1">
        <p>Enable notifications</p>
        <p className="text-muted-foreground text-xs">
          You can enable or disable notifications at any time.
        </p>
      </div>
    </Label>
  );
}

demo.tsx
import Particle from "@/components/ui/v-checkbox-4";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-6">
      <div className="w-full max-w-sm">
        <Particle />
      </div>
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add checkbox label
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
