<!-- Notification Checkbox Group · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-checkbox-group-8
     license: MIT · category: form
     A vertical checkbox group for notification preferences, with each option showing a bold label and a helper description on divided rows. -->

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
components/ui/v-checkbox-group-8.tsx
import { Checkbox } from "@/registry/default/ui/checkbox";
import { CheckboxGroup } from "@/registry/default/ui/checkbox-group";

const notifications = [
  {
    description: "Get notified when someone @mentions you.",
    id: "mentions",
    label: "Mentions",
  },
  {
    description: "Receive a summary of your team's activity.",
    id: "digest",
    label: "Weekly digest",
  },
  {
    description: "Security alerts and login notifications.",
    id: "security",
    label: "Security alerts",
  },
  {
    description: "Product updates and new feature announcements.",
    id: "product",
    label: "Product updates",
  },
];

export default function Component() {
  return (
    <CheckboxGroup
      aria-label="Notification preferences"
      className="gap-0 divide-y"
      defaultValue={["mentions", "security"]}
    >
      {notifications.map((item) => (
        <label
          className="flex cursor-pointer items-start gap-3 py-3 first:pt-0 last:pb-0"
          htmlFor={item.id}
          key={item.id}
        >
          <Checkbox className="mt-0.5" id={item.id} value={item.id} />
          <div className="flex flex-col gap-0.5">
            <span className="font-medium text-sm">{item.label}</span>
            <span className="text-muted-foreground text-xs">
              {item.description}
            </span>
          </div>
        </label>
      ))}
    </CheckboxGroup>
  );
}

demo.tsx
import Component from "@/components/ui/v-checkbox-group-8";

export default function Default() {
  return (
    <div className="mx-auto w-full max-w-sm p-6">
      <Component />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add checkbox checkbox-group
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
