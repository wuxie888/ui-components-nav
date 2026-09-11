<!-- Select with Disabled Options · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-select-11
     license: no-license · category: form
     A select dropdown that groups active options and shows disabled, non-selectable items marked as coming soon, separated by a divider. -->

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
components/ui/v-select-11.tsx
import {
  Select,
  SelectItem,
  SelectPopup,
  SelectSeparator,
  SelectTrigger,
  SelectValue,
} from "@/registry/default/ui/select";

const activeRoles = [
  { label: "Owner", value: "owner" },
  { label: "Admin", value: "admin" },
  { label: "Editor", value: "editor" },
  { label: "Viewer", value: "viewer" },
];

const comingSoon = [
  { label: "Guest", value: "guest" },
  { label: "Contractor", value: "contractor" },
];

const placeholder = { label: "Select a role", value: null };
const allItems = [placeholder, ...activeRoles, ...comingSoon];

export default function Particle() {
  return (
    <Select defaultValue={activeRoles[2]} items={allItems}>
      <SelectTrigger className="w-52">
        <SelectValue />
      </SelectTrigger>
      <SelectPopup>
        {activeRoles.map((item) => (
          <SelectItem key={item.value} value={item}>
            {item.label}
          </SelectItem>
        ))}
        <SelectSeparator />
        {comingSoon.map((item) => (
          <SelectItem disabled key={item.value} value={item}>
            {item.label}
            <span className="ms-1.5 text-muted-foreground text-xs">
              (coming soon)
            </span>
          </SelectItem>
        ))}
      </SelectPopup>
    </Select>
  );
}

demo.tsx
import Component from "@/components/ui/v-select-11";

export default function Default() {
  return (
    <div className="flex min-h-52 w-full items-center justify-center p-6">
      <Component />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react class-variance-authority lucide-react
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
