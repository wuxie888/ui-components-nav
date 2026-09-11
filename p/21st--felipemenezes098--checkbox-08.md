<!-- Choice Cards · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/checkbox-08
     license: MIT · category: pricing-section
     Bordered add-on selection cards with a checkbox, title, description and price that highlight when checked. -->

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
components/ui/checkbox-08.tsx
import { Checkbox } from '@/components/ui/checkbox'
import {
  Field,
  FieldContent,
  FieldDescription,
  FieldGroup,
  FieldLabel,
  FieldTitle,
} from '@/components/ui/field'

const addons = [
  {
    id: 'storage',
    title: 'Extra storage',
    description: '+100 GB of cloud storage.',
    price: '$4/mo',
    defaultChecked: true,
  },
  {
    id: 'support',
    title: 'Priority support',
    description: '24/7 chat with a 1h response SLA.',
    price: '$9/mo',
    defaultChecked: false,
  },
  {
    id: 'analytics',
    title: 'Advanced analytics',
    description: 'Funnels, retention, and custom reports.',
    price: '$12/mo',
    defaultChecked: false,
  },
]

export function Checkbox08() {
  return (
    <FieldGroup className="w-full max-w-sm gap-3">
      {addons.map((addon) => (
        <FieldLabel key={addon.id} htmlFor={`checkbox-08-${addon.id}`}>
          <Field orientation="horizontal">
            <FieldContent>
              <FieldTitle>{addon.title}</FieldTitle>
              <FieldDescription>{addon.description}</FieldDescription>
            </FieldContent>
            <span className="text-sm font-medium tabular-nums">
              {addon.price}
            </span>
            <Checkbox
              id={`checkbox-08-${addon.id}`}
              defaultChecked={addon.defaultChecked}
            />
          </Field>
        </FieldLabel>
      ))}
    </FieldGroup>
  )
}

demo.tsx
import { Checkbox08 } from "@/components/ui/checkbox-08";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Checkbox08 />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add checkbox field
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
