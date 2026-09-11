<!-- Notification Settings Field · @shadcnspace · https://21st.dev/@shadcnspace/components/field-02
     license: MIT · category: form
     A notification preferences list that pairs a title, description and toggle switch for each setting, separated by dividers, for account or app settings pages. -->

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
components/shadcn-space/field/field-02.tsx
import {
  Field,
  FieldContent,
  FieldDescription,
  FieldGroup,
  FieldSeparator,
  FieldTitle,
} from "@/components/ui/field";
import { Switch } from "@/components/ui/switch";

type NotificationOption = {
  id: string;
  title: string;
  description: string;
  defaultChecked: boolean;
};

const notificationOptions: NotificationOption[] = [
  {
    id: "notif-comments",
    title: "Comments",
    description: "Get notified when someone comments on your post.",
    defaultChecked: true,
  },
  {
    id: "notif-mentions",
    title: "Mentions",
    description: "Get notified when someone mentions you.",
    defaultChecked: true,
  },
  {
    id: "notif-marketing",
    title: "Marketing emails",
    description: "Receive emails about new products and features.",
    defaultChecked: false,
  },
];

const NotificationSettingsDemo = () => {
  return (
    <div className="w-full max-w-md rounded-xl border border-border p-6">
      <FieldGroup className="gap-4">
        {notificationOptions.map((option, index) => (
          <div key={option.id} className="flex flex-col gap-4">
            <Field orientation="horizontal">
              <FieldContent>
                <FieldTitle>{option.title}</FieldTitle>
                <FieldDescription>{option.description}</FieldDescription>
              </FieldContent>
              <Switch id={option.id} defaultChecked={option.defaultChecked} />
            </Field>
            {index < notificationOptions.length - 1 && <FieldSeparator />}
          </div>
        ))}
      </FieldGroup>
    </div>
  );
};

export default NotificationSettingsDemo;

demo.tsx
import NotificationSettingsField from "@/components/ui/field-02";

export default function DemoPreview() {
  return (
    <div className="flex w-full items-center justify-center p-8">
      <NotificationSettingsField />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add field switch
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
