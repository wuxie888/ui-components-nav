<!-- Key Value List · @corr · https://21st.dev/@corr/components/key-value-list
     license: no-license · category: list
     A definition-list component that displays label/value pairs with optional descriptions for metadata and payload details. -->

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
components/ui/key-value-list.tsx
import type { ReactNode } from "react"

import { cn } from "@/lib/utils"

export type KeyValueListItem = {
  label: ReactNode
  value: ReactNode
  description?: ReactNode
}

export function KeyValueList({
  items,
  variant = "default",
  className,
}: {
  items: KeyValueListItem[]
  variant?: "default" | "bordered"
  className?: string
}) {
  return (
    <dl
      className={cn(
        "grid text-sm",
        variant === "bordered" && "overflow-hidden rounded-md border",
        className
      )}
    >
      {items.map((item, index) => (
        <div
          key={index}
          className={cn(
            "grid gap-1 px-3 py-2 sm:grid-cols-[10rem_minmax(0,1fr)] sm:gap-4",
            variant === "bordered" && index > 0 && "border-t"
          )}
        >
          <dt className="text-muted-foreground">{item.label}</dt>
          <dd className="min-w-0 font-medium">
            <div className="truncate">{item.value}</div>
            {item.description ? (
              <div className="mt-1 text-xs font-normal text-muted-foreground">
                {item.description}
              </div>
            ) : null}
          </dd>
        </div>
      ))}
    </dl>
  )
}

demo.tsx
import { KeyValueList } from "@/components/ui/key-value-list";

export default function KeyValueListDemo() {
  return (
    <div className="mx-auto w-full max-w-md p-6">
      <KeyValueList
        variant="bordered"
        items={[
          { label: "Name", value: "key-value-list" },
          { label: "Type", value: "registry:component" },
          {
            label: "Author",
            value: "corr",
            description: "Maintained on ui.corr.sh",
          },
          { label: "Version", value: "1.0.0" },
          { label: "License", value: "MIT" },
        ]}
      />
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
npx shadcn@latest add context-menu filters.json
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
