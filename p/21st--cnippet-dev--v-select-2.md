<!-- Basic Select · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-select-2
     license: MIT · category: form
     A dropdown select built on Base UI, shown in small, default, and large trigger sizes for choosing a plan. -->

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
components/ui/v-select-2.tsx
import {
  Select,
  SelectItem,
  SelectPopup,
  SelectTrigger,
  SelectValue,
} from "@/registry/default/ui/select";

const sizes = [
  { label: "Select a plan", value: null },
  { label: "Free", value: "free" },
  { label: "Pro", value: "pro" },
  { label: "Team", value: "team" },
  { label: "Enterprise", value: "enterprise" },
];

export default function Component() {
  return (
    <div className="flex flex-col items-start gap-4">
      <div className="flex flex-col gap-1.5">
        <span className="text-muted-foreground text-xs">Small</span>
        <Select items={sizes}>
          <SelectTrigger className="w-48" size="sm">
            <SelectValue />
          </SelectTrigger>
          <SelectPopup>
            {sizes.slice(1).map((item) => (
              <SelectItem key={item.value} value={item}>
                {item.label}
              </SelectItem>
            ))}
          </SelectPopup>
        </Select>
      </div>
      <div className="flex flex-col gap-1.5">
        <span className="text-muted-foreground text-xs">Default</span>
        <Select items={sizes}>
          <SelectTrigger className="w-48">
            <SelectValue />
          </SelectTrigger>
          <SelectPopup>
            {sizes.slice(1).map((item) => (
              <SelectItem key={item.value} value={item}>
                {item.label}
              </SelectItem>
            ))}
          </SelectPopup>
        </Select>
      </div>
      <div className="flex flex-col gap-1.5">
        <span className="text-muted-foreground text-xs">Large</span>
        <Select items={sizes}>
          <SelectTrigger className="w-48" size="lg">
            <SelectValue />
          </SelectTrigger>
          <SelectPopup>
            {sizes.slice(1).map((item) => (
              <SelectItem key={item.value} value={item}>
                {item.label}
              </SelectItem>
            ))}
          </SelectPopup>
        </Select>
      </div>
    </div>
  );
}

demo.tsx
import Component from "@/components/ui/v-select-2";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-8">
      <Component />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react class-variance-authority clsx lucide-react tailwind-merge
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add select
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
