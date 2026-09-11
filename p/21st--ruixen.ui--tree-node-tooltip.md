<!-- Tree Node Tooltip · @ruixen.ui · https://21st.dev/@ruixen.ui/components/tree-node-tooltip
     license: unspecified · category: file-tree
     This TreeView component is a modern, lightweight file explorer built with React, Framer Motion, and shadcn/ui. It displays a hierarchical structure of folders and files, where users can expand or collapse folders by clicking on either the folder icon or the name. Each item shows a tooltip on hover, making it easy to identify long or truncated file names. The design follows shadcn UI styling principles with clean hover effects, accent colors, and rounded corners, while Framer Motion adds smooth expand/collapse animations. This makes the component both visually appealing and intuitive to use, ideal for dashboards, code editors, or any application that needs a structured tree navigation system. -->

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
components/ui/tree-node-tooltip.tsx
"use client";

import React, { useState } from "react";
import { motion, AnimatePresence } from "motion/react";
import { cn } from "@/lib/utils";
import { Folder, File } from "lucide-react";
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/components/ui/tooltip";

export type TreeNode = {
  id: string;
  name: string;
  tooltip?: string;
  type: "folder" | "file";
  children?: TreeNode[];
};

const demoData: TreeNode[] = [
  {
    id: "1",
    name: "src",
    tooltip: "src",
    type: "folder",
    children: [
      {
        id: "2",
        name: "components",
        tooltip: "components",
        type: "folder",
        children: [
          {
            id: "3",
            name: "Button.tsx",
            tooltip: "Button's tooltip",
            type: "file",
          },
          {
            id: "4",
            name: "Card.tsx",
            tooltip: "Card's tooltip",
            type: "file",
          },
        ],
      },
      {
        id: "5",
        name: "lib",
        tooltip: "lib",
        type: "folder",
        children: [
          {
            id: "6",
            name: "utils.ts",
            tooltip: "utils's tooltip",
            type: "file",
          },
        ],
      },
    ],
  },
];

export default function TreeNodeTooltip({ node }: { node: TreeNode }) {
  const [expanded, setExpanded] = useState(false);

  const isFolder = node.type === "folder";

  const toggle = () => {
    if (isFolder) setExpanded((prev) => !prev);
  };

  return (
    <div>
      <TooltipProvider>
        <Tooltip>
          <TooltipTrigger asChild>
            <button
              onClick={toggle}
              className={cn(
                "flex items-center gap-2 px-2 py-1 rounded-md w-full text-left",
                "hover:bg-accent hover:text-accent-foreground transition-colors",
              )}
            >
              {isFolder ? (
                <Folder
                  size={16}
                  className={cn(
                    "text-muted-foreground",
                    expanded && "text-blue-500",
                  )}
                />
              ) : (
                <File size={16} className="text-muted-foreground" />
              )}
              <span className="truncate">{node.name}</span>
            </button>
          </TooltipTrigger>
          <TooltipContent side="right">{node.tooltip}</TooltipContent>
        </Tooltip>
      </TooltipProvider>

      {/* Animate children */}
      {isFolder && (
        <AnimatePresence>
          {expanded && (
            <motion.div
              initial={{ height: 0, opacity: 0 }}
              animate={{ height: "auto", opacity: 1 }}
              exit={{ height: 0, opacity: 0 }}
              transition={{ duration: 0.2 }}
              className="ml-4 border-l pl-2 space-y-1"
            >
              {node.children?.map((child) => (
                <TreeNodeTooltip key={child.id} node={child} />
              ))}
            </motion.div>
          )}
        </AnimatePresence>
      )}
    </div>
  );
}

demo.tsx
import TreeNodeTooltip from "@/components/ui/tree-node-tooltip";

  const demoData = [
  {
    id: "1",
    name: "src",
    tooltip: "src",
    type: "folder",
    children: [
      {
        id: "2",
        name: "components",
        tooltip: "components",
        type: "folder",
        children: [
          { id: "3", name: "Button.tsx", tooltip: "Button's tooltip", type: "file" },
          { id: "4", name: "Card.tsx", tooltip: "Card's tooltip", type: "file" },
        ],
      },
      {
        id: "5",
        name: "lib",
        tooltip: "lib",
        type: "folder",
        children: [{ id: "6", name: "utils.ts", tooltip: "utils's tooltip", type: "file" }],
      },
    ],
  },
];

export default function TreeViewDemo() {
  return (
    <div className="p-4 bg-card rounded-xl shadow-sm">
      {demoData.map((node) => (
        <TreeNodeTooltip key={node.id} node={node} />
      ))}
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add tooltip
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
