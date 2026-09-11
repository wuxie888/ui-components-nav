<!-- Sidebar Nav Group · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/collapsible-05
     license: agpl-3.0 · category: sidebar
     A collapsible sidebar navigation with grouped sections, sub-item links, and a chevron that rotates when a group expands. -->

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
components/ui/collapsible-05.tsx
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from '@/components/ui/collapsible'
import {
  ChevronRightIcon,
  LayoutDashboardIcon,
  SettingsIcon,
} from 'lucide-react'

const groups = [
  {
    label: 'Platform',
    icon: LayoutDashboardIcon,
    defaultOpen: true,
    items: ['Overview', 'Projects', 'Deployments', 'Analytics'],
  },
  {
    label: 'Settings',
    icon: SettingsIcon,
    defaultOpen: false,
    items: ['General', 'Members', 'Billing', 'Integrations'],
  },
]

export function Collapsible05() {
  return (
    <nav className="w-full max-w-60 space-y-1 rounded-lg border p-2">
      {groups.map((group) => (
        <Collapsible key={group.label} defaultOpen={group.defaultOpen}>
          <CollapsibleTrigger className="group/nav hover:bg-accent flex w-full items-center gap-2 rounded-md px-2 py-1.5 text-sm font-medium">
            <group.icon className="text-muted-foreground size-4 shrink-0" />
            {group.label}
            <ChevronRightIcon className="text-muted-foreground ml-auto size-4 shrink-0 transition-transform group-data-panel-open/nav:rotate-90" />
          </CollapsibleTrigger>
          <CollapsibleContent>
            <ul className="mt-1 ml-4 space-y-0.5 border-l pl-4">
              {group.items.map((item) => (
                <li key={item}>
                  <a
                    href="#"
                    className="hover:bg-accent text-muted-foreground hover:text-foreground block rounded-md px-2 py-1.5 text-sm"
                  >
                    {item}
                  </a>
                </li>
              ))}
            </ul>
          </CollapsibleContent>
        </Collapsible>
      ))}
    </nav>
  )
}

demo.tsx
import { Collapsible05 } from "@/components/ui/collapsible-05";

export default function Default() {
  return (
    <div className="flex min-h-80 w-full items-center justify-center p-6">
      <Collapsible05 />
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
npx shadcn@latest add collapsible
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
