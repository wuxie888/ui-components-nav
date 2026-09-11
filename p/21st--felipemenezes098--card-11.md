<!-- Notification Settings Card · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/card-11
     license: MIT · category: notification
     A settings card with horizontal field rows, each toggled by a switch, for managing notification preferences. -->

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
components/ui/card-11.tsx
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'
import {
  Field,
  FieldContent,
  FieldDescription,
  FieldGroup,
  FieldLabel,
} from '@/components/ui/field'
import { Switch } from '@/components/ui/switch'

const settings = [
  {
    id: 'card-11-product',
    label: 'Product updates',
    description: 'News about features and improvements.',
    defaultChecked: true,
  },
  {
    id: 'card-11-security',
    label: 'Security alerts',
    description: 'Get notified about suspicious activity.',
    defaultChecked: true,
  },
  {
    id: 'card-11-marketing',
    label: 'Marketing emails',
    description: 'Tips, offers, and occasional surveys.',
    defaultChecked: false,
  },
]

export function Card11() {
  return (
    <Card className="w-full max-w-sm">
      <CardHeader>
        <CardTitle>Notifications</CardTitle>
        <CardDescription>Choose what you want to hear about.</CardDescription>
      </CardHeader>
      <CardContent>
        <FieldGroup>
          {settings.map((setting) => (
            <Field key={setting.id} orientation="horizontal">
              <FieldContent>
                <FieldLabel htmlFor={setting.id}>{setting.label}</FieldLabel>
                <FieldDescription>{setting.description}</FieldDescription>
              </FieldContent>
              <Switch id={setting.id} defaultChecked={setting.defaultChecked} />
            </Field>
          ))}
        </FieldGroup>
      </CardContent>
    </Card>
  )
}

demo.tsx
import Card11 from "@/components/ui/card-11";

export default function Card11Demo() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Card11 />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add card field switch
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
