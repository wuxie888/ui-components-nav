<!-- Checkbox List Group · @shadcnspace · https://21st.dev/@shadcnspace/components/checkbox-05
     license: MIT · category: form
     A bordered list of options each pairing a labeled row with icon and a checkbox, useful for selecting multiple preferences or skills. -->

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
components/shadcn-space/checkbox/checkbox-05.tsx
import { Gamepad2, MapPinned, Music2 } from 'lucide-react'
import { Checkbox } from '@/components/ui/checkbox'
import { Label } from '@/components/ui/label'

const skills = [
  { label: 'Music & Singing', icon: Music2  },
  { label: 'Gaming', icon: Gamepad2 },
  { label: 'Traveling', icon: MapPinned },
]

const CheckboxListGroupDemo = () => {
  return (
    <ul className='flex w-full flex-col divide-y rounded-md border'>
      {skills.map(({ label, icon: Icon }) => (
        <li key={label} className='flex items-center justify-between gap-2 px-5 py-3'>
          <Label htmlFor={label}>
            <span className='flex items-center gap-2'>
              <Icon className='size-4' /> {label}
            </span>
          </Label>
          <Checkbox id={label} className='cursor-pointer' />
        </li>
      ))}
    </ul>
  )
}

export default CheckboxListGroupDemo

demo.tsx
import { Gamepad2, MapPinned, Music2 } from "lucide-react";
import { Checkbox } from "@/components/ui/checkbox";
import { Label } from "@/components/ui/label";

const skills = [
  { label: "Music & Singing", icon: Music2, checked: true },
  { label: "Gaming", icon: Gamepad2, checked: false },
  { label: "Traveling", icon: MapPinned, checked: true },
];

export default function Default() {
  return (
    <div className="flex w-full max-w-sm items-center justify-center p-6">
      <ul className="flex w-full flex-col divide-y rounded-md border">
        {skills.map(({ label, icon: Icon, checked }) => (
          <li
            key={label}
            className="flex items-center justify-between gap-2 px-5 py-3"
          >
            <Label htmlFor={label}>
              <span className="flex items-center gap-2">
                <Icon className="size-4" /> {label}
              </span>
            </Label>
            <Checkbox
              id={label}
              className="cursor-pointer"
              defaultChecked={checked}
            />
          </li>
        ))}
      </ul>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
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
