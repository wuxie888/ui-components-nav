<!-- Nested Tree Checkbox · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/checkbox-07
     license: agpl-3.0 · category: file-tree
     A multi-level file-tree of checkboxes where each parent node derives its checked or indeterminate state from its children. -->

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
components/ui/checkbox-07.tsx
'use client'

import { Checkbox } from '@/components/ui/checkbox'
import { Label } from '@/components/ui/label'
import { FileIcon, FolderIcon } from 'lucide-react'
import { useState } from 'react'

type TreeNode = {
  id: string
  label: string
  children?: TreeNode[]
}

const tree: TreeNode[] = [
  {
    id: 'src',
    label: 'src',
    children: [
      {
        id: 'components',
        label: 'components',
        children: [
          { id: 'button', label: 'button.tsx' },
          { id: 'input', label: 'input.tsx' },
        ],
      },
      {
        id: 'lib',
        label: 'lib',
        children: [{ id: 'utils', label: 'utils.ts' }],
      },
    ],
  },
  {
    id: 'public',
    label: 'public',
    children: [{ id: 'logo', label: 'logo.svg' }],
  },
]

function leafIds(node: TreeNode): string[] {
  return node.children ? node.children.flatMap(leafIds) : [node.id]
}

function TreeItem({
  node,
  checked,
  onToggle,
  depth = 0,
}: Readonly<{
  node: TreeNode
  checked: Set<string>
  onToggle: (ids: string[], next: boolean) => void
  depth?: number
}>) {
  const leaves = leafIds(node)
  const checkedLeaves = leaves.filter((id) => checked.has(id))
  const allLeavesChecked =
    checkedLeaves.length > 0 && checkedLeaves.length === leaves.length
  const someLeavesChecked =
    checkedLeaves.length > 0 && checkedLeaves.length < leaves.length

  return (
    <div className="flex flex-col gap-2.5">
      <div
        className="flex items-center gap-2.5"
        style={{ paddingLeft: depth * 20 }}
      >
        <Checkbox
          id={`checkbox-07-${node.id}`}
          checked={allLeavesChecked}
          indeterminate={someLeavesChecked}
          onCheckedChange={(value) => onToggle(leaves, value === true)}
        />
        {node.children ? (
          <FolderIcon className="text-muted-foreground size-4" />
        ) : (
          <FileIcon className="text-muted-foreground size-4" />
        )}
        <Label htmlFor={`checkbox-07-${node.id}`} className="font-normal">
          {node.label}
        </Label>
      </div>
      {node.children?.map((child) => (
        <TreeItem
          key={child.id}
          node={child}
          checked={checked}
          onToggle={onToggle}
          depth={depth + 1}
        />
      ))}
    </div>
  )
}

export function Checkbox07() {
  const [checked, setChecked] = useState<Set<string>>(
    () => new Set(['button', 'utils']),
  )

  const toggle = (ids: string[], next: boolean) =>
    setChecked((prev) => {
      const draft = new Set(prev)
      ids.forEach((id) => (next ? draft.add(id) : draft.delete(id)))
      return draft
    })

  return (
    <div className="flex w-full max-w-xs flex-col gap-2.5">
      {tree.map((node) => (
        <TreeItem
          key={node.id}
          node={node}
          checked={checked}
          onToggle={toggle}
        />
      ))}
    </div>
  )
}

demo.tsx
import { Checkbox07 } from "@/components/ui/checkbox-07";

export default function Default() {
  return (
    <div className="flex min-h-72 w-full items-center justify-center p-6">
      <Checkbox07 />
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
