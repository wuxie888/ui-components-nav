<!-- FileTree · @ruixen.ui · https://21st.dev/@ruixen.ui/components/file-tree
     license: unspecified · category: file-tree
     This File Tree component is a fully interactive explorer built using shadcn/ui and lucide-react icons. It mimics the structure of an IDE file browser, allowing you to expand and collapse folders, select files or folders, and perform quick actions like adding, renaming, or deleting items. The component demonstrates recursive rendering for nested structures, contextual controls with tooltips, and smooth UI styling powered by shadcn. It’s flexible enough to serve as a base for a project navigator, documentation tree, or any hierarchical data display, while remaining consistent with the shadcn design system. -->

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
components/ui/motion-file-tree.tsx
"use client";

import React, { useState } from "react";
import { cn } from "@/lib/utils";
import { motion, AnimatePresence } from "motion/react";
import { ChevronRight, ChevronDown, Folder, File } from "lucide-react";

// -------- Types --------
export type FileNode = {
  id: string;
  name: string;
  type: "file" | "folder";
  children?: FileNode[];
};

export type MotionFileTreeProps = {
  data: FileNode[];
  defaultExpanded?: Record<string, boolean>;
  onSelect?: (node: FileNode) => void;
};

// -------- Component --------
export default function MotionFileTree({
  data,
  defaultExpanded = {},
  onSelect,
}: MotionFileTreeProps) {
  const [expanded, setExpanded] =
    useState<Record<string, boolean>>(defaultExpanded);
  const [selected, setSelected] = useState<string | null>(null);

  const toggle = (id: string) => {
    setExpanded((prev) => ({ ...prev, [id]: !prev[id] }));
  };

  const renderNodes = (nodes: FileNode[], level = 0) => {
    return nodes.map((n) => (
      <div key={n.id} className="relative">
        <div
          role="treeitem"
          tabIndex={0}
          aria-expanded={n.type === "folder" ? !!expanded[n.id] : undefined}
          className={cn(
            "flex items-center gap-2 px-2 py-1 rounded-md cursor-pointer transition-colors select-none outline-none",
            selected === n.id
              ? "bg-accent text-accent-foreground border-l-2 border-primary"
              : "hover:bg-muted",
          )}
          style={{ paddingLeft: level * 14 + 8 }}
          onClick={() => {
            if (n.type === "folder") toggle(n.id);
            setSelected(n.id);
            onSelect?.(n);
          }}
          onKeyDown={(e) => {
            if (n.type === "folder" && (e.key === "Enter" || e.key === " ")) {
              e.preventDefault();
              toggle(n.id);
            }
          }}
        >
          {n.type === "folder" ? (
            <>
              {expanded[n.id] ? (
                <ChevronDown size={14} />
              ) : (
                <ChevronRight size={14} />
              )}
              <Folder size={16} />
            </>
          ) : (
            <File size={14} />
          )}
          <span className="text-sm truncate">{n.name}</span>
        </div>

        {/* Children with smooth animation */}
        <AnimatePresence initial={false}>
          {n.children && n.children.length > 0 && expanded[n.id] && (
            <motion.div
              key="children"
              role="group"
              initial={{ height: 0, opacity: 0 }}
              animate={{ height: "auto", opacity: 1 }}
              exit={{ height: 0, opacity: 0 }}
              transition={{ duration: 0.25, ease: "easeInOut" }}
              className="pl-3 border-l border-muted"
            >
              {renderNodes(n.children, level + 1)}
            </motion.div>
          )}
        </AnimatePresence>
      </div>
    ));
  };

  return (
    <div role="tree" className="space-y-1 text-sm">
      {renderNodes(data)}
    </div>
  );
}

demo.tsx
import FileTree from "@/components/ui/file-tree";

export default function DemoOne() {
  return <FileTree />;
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add card input separator tooltip
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
