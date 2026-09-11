<!-- Tree View · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/tree-view
     license: unspecified · category: file-tree
     A hierarchical data display component with expand/collapse functionality, perfect for file explorers, organizational charts, or any nested data structure. -->

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
components/ui/tree.tsx
"use client";

import { Slot } from "@radix-ui/react-slot";
import { cva, type VariantProps } from "class-variance-authority";
import { ChevronRight, File, Folder, FolderOpen } from "lucide-react";
import * as React from "react";
import { cn } from "@/lib/utils";

interface TreeContextType {
  expandedIds: Set<string>;
  selectedIds: string[];
  toggleExpanded: (nodeId: string) => void;
  handleSelection: (nodeId: string, ctrlKey?: boolean) => void;
  showLines: boolean;
  showIcons: boolean;
  selectable: boolean;
  multiSelect: boolean;
  animateExpand: boolean;
  indent: number;
  onNodeClick?: (nodeId: string, data?: any) => void;
  onNodeExpand?: (nodeId: string, expanded: boolean) => void;
}

const TreeContext = React.createContext<TreeContextType | null>(null);

const useTree = () => {
  const context = React.useContext(TreeContext);
  if (!context)
    throw new Error("Tree components must be used within a TreeProvider");
  return context;
};

const treeVariants = cva(
  "w-full rounded-ele border border-border bg-background",
  {
    variants: {
      variant: {
        default: "",
        outline: "border-2",
        ghost: "border-transparent bg-transparent",
      },
      size: {
        sm: "text-sm",
        default: "",
        lg: "text-lg",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  }
);

const treeItemVariants = cva(
  "group relative flex cursor-pointer select-none items-center rounded-[calc(var(--card-radius)-8px)] px-3 py-2 outline-none transition-colors motion-safe:duration-200",
  {
    variants: {
      variant: {
        default:
          "hover:bg-accent focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring",
        ghost: "hover:bg-accent/50",
        subtle: "hover:bg-muted/50",
      },
      selected: {
        true: "bg-accent text-accent-foreground",
        false: "",
      },
    },
    defaultVariants: {
      variant: "default",
      selected: false,
    },
  }
);

export interface TreeProviderProps
  extends React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof treeVariants> {
  defaultExpandedIds?: string[];
  selectedIds?: string[];
  onSelectionChange?: (selectedIds: string[]) => void;
  onNodeClick?: (nodeId: string, data?: any) => void;
  onNodeExpand?: (nodeId: string, expanded: boolean) => void;
  showLines?: boolean;
  showIcons?: boolean;
  selectable?: boolean;
  multiSelect?: boolean;
  animateExpand?: boolean;
  indent?: number;
}

const TreeProvider = React.forwardRef<HTMLDivElement, TreeProviderProps>(
  (
    {
      className,
      variant,
      size,
      children,
      defaultExpandedIds = [],
      selectedIds = [],
      onSelectionChange,
      onNodeClick,
      onNodeExpand,
      showLines = true,
      showIcons = true,
      selectable = true,
      multiSelect = false,
      animateExpand = true,
      indent = 20,
      ...props
    },
    ref
  ) => {
    const [expandedIds, setExpandedIds] = React.useState<Set<string>>(
      new Set(defaultExpandedIds)
    );
    const [internalSelectedIds, setInternalSelectedIds] =
      React.useState<string[]>(selectedIds);

    const isControlled = onSelectionChange !== undefined;
    const currentSelectedIds = isControlled ? selectedIds : internalSelectedIds;

    const toggleExpanded = React.useCallback(
      (nodeId: string) => {
        setExpandedIds((prev) => {
          const newSet = new Set(prev);
          const isExpanded = newSet.has(nodeId);
          isExpanded ? newSet.delete(nodeId) : newSet.add(nodeId);
          onNodeExpand?.(nodeId, !isExpanded);
          return newSet;
        });
      },
      [onNodeExpand]
    );

    const handleSelection = React.useCallback(
      (nodeId: string, ctrlKey = false) => {
        if (!selectable) return;
        let newSelection: string[];
        if (multiSelect && ctrlKey) {
          newSelection = currentSelectedIds.includes(nodeId)
            ? currentSelectedIds.filter((id) => id !== nodeId)
            : [...currentSelectedIds, nodeId];
        } else {
          newSelection = currentSelectedIds.includes(nodeId) ? [] : [nodeId];
        }
        isControlled
          ? onSelectionChange?.(newSelection)
          : setInternalSelectedIds(newSelection);
      },
      [
        selectable,
        multiSelect,
        currentSelectedIds,
        isControlled,
        onSelectionChange,
      ]
    );

    const contextValue: TreeContextType = {
      expandedIds,
      selectedIds: currentSelectedIds,
      toggleExpanded,
      handleSelection,
      showLines,
      showIcons,
      selectable,
      multiSelect,
      animateExpand,
      indent,
      onNodeClick,
      onNodeExpand,
    };

    return (
      <TreeContext.Provider value={contextValue}>
        <div
          className={cn(
            treeVariants({ variant, size, className }),
            "motion-safe:duration-200 motion-reduce:animate-none"
          )}
          ref={ref}
          {...props}
        >
          <div className="p-2">{children}</div>
        </div>
      </TreeContext.Provider>
    );
  }
);
TreeProvider.displayName = "TreeProvider";

export interface TreeProps extends React.HTMLAttributes<HTMLDivElement> {
  asChild?: boolean;
}

const Tree = React.forwardRef<HTMLDivElement, TreeProps>(
  ({ className, asChild = false, children, ...props }, ref) => {
    const Comp = asChild ? Slot : "div";
    return (
      <Comp
        className={cn("flex flex-col gap-1", className)}
        ref={ref}
        {...props}
      >
        {children}
      </Comp>
    );
  }
);
Tree.displayName = "Tree";

export interface TreeItemProps
  extends React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof treeItemVariants> {
  nodeId: string;
  label: string;
  icon?: React.ReactNode;
  data?: any;
  level?: number;
  isLast?: boolean;
  parentPath?: boolean[];
  hasChildren?: boolean;
  asChild?: boolean;
}

const TreeItem = React.forwardRef<HTMLDivElement, TreeItemProps>(
  (
    {
      className,
      variant,
      nodeId,
      label,
      icon,
      data,
      level = 0,
      isLast = false,
      parentPath = [],
      hasChildren = false,
      asChild = false,
      children,
      onClick,
      ...props
    },
    ref
  ) => {
    const {
      expandedIds,
      selectedIds,
      toggleExpanded,
      handleSelection,
      showLines,
      showIcons,
      animateExpand,
      indent,
      onNodeClick,
    } = useTree();

    const isExpanded = expandedIds.has(nodeId);
    const isSelected = selectedIds.includes(nodeId);
    const currentPath = [...parentPath, isLast];

    const getDefaultIcon = () =>
      hasChildren ? (
        isExpanded ? (
          <FolderOpen aria-hidden="true" className="size-4" />
        ) : (
          <Folder aria-hidden="true" className="size-4" />
        )
      ) : (
        <File aria-hidden="true" className="size-4" />
      );

    const handleClick = (e: React.MouseEvent<HTMLDivElement>) => {
      if (hasChildren) toggleExpanded(nodeId);
      handleSelection(nodeId, e.ctrlKey || e.metaKey);
      onNodeClick?.(nodeId, data);
      onClick?.(e);
    };

    const Comp = asChild ? Slot : "div";

    return (
      <div className="select-none" role="none">
        <div
          aria-expanded={hasChildren ? isExpanded : undefined}
          aria-selected={isSelected || undefined}
          className={cn(
            treeItemVariants({ variant, selected: isSelected, className })
          )}
          onClick={handleClick}
          ref={ref}
          role="treeitem"
          style={{ paddingLeft: level * indent + 8 }}
          tabIndex={0}
          {...props}
        >
          {showLines && level > 0 && (
            <div className="pointer-events-none absolute inset-y-0 left-0">
              {currentPath.map((isLastInPath, pathIndex) => (
                <div
                  className="absolute inset-y-0 border-border/40 border-l"
                  key={pathIndex}
                  style={{
                    left: pathIndex * indent + 12,
                    display:
                      pathIndex === currentPath.length - 1 && isLastInPath
                        ? "none"
                        : "block",
                  }}
                />
              ))}
              <div
                className="absolute top-1/2 border-border/40 border-t"
                style={{
                  left: (level - 1) * indent + 12,
                  width: indent - 4,
                  transform: "translateY(-1px)",
                }}
              />
              {isLast && (
                <div
                  className="absolute top-0 border-border/40 border-l"
                  style={{ left: (level - 1) * indent + 12, height: "50%" }}
                />
              )}
            </div>
          )}

          <div
            aria-hidden="true"
            className={cn(
              "flex size-4 items-center justify-center text-muted-foreground transition-transform motion-safe:duration-200",
              hasChildren && isExpanded ? "rotate-90" : "rotate-0"
            )}
          >
            {hasChildren && <ChevronRight className="size-3" />}
          </div>

          {showIcons && (
            <div className="mx-2 flex size-4 items-center justify-center text-muted-foreground">
              {icon || getDefaultIcon()}
            </div>
          )}

          <span className="flex-1 truncate text-foreground text-sm">
            {label}
          </span>
        </div>

        {hasChildren && (
          <div
            aria-hidden={!isExpanded}
            className={cn(
              "overflow-hidden",
              animateExpand
                ? "transition-[max-height,opacity] motion-safe:duration-200"
                : "",
              isExpanded ? "max-h-[2000px] opacity-100" : "max-h-0 opacity-0"
            )}
          >
            <div className="translate-y-0 will-change-transform motion-safe:duration-200">
              {children}
            </div>
          </div>
        )}
      </div>
    );
  }
);
TreeItem.displayName = "TreeItem";

export { TreeProvider, Tree, TreeItem, treeVariants, treeItemVariants };

demo.tsx
import { TreeView } from "@/components/ui/tree-view";

const DemoOne = () => {

const treeData = [
  {
    id: "1",
    label: "Documents",
    children: [
      {
        id: "1-1",
        label: "Projects",
        children: [
          { id: "1-1-1", label: "Project A.pdf" },
          { id: "1-1-2", label: "Project B.docx" },
          {
            id: "1-1-3",
            label: "Archive",
            children: [
              { id: "1-1-3-1", label: "Old Project.zip" },
              { id: "1-1-3-2", label: "Backup.tar" },
            ],
          },
        ],
      },
      {
        id: "1-2",
        label: "Reports",
        children: [
          { id: "1-2-1", label: "Monthly Report.xlsx" },
          { id: "1-2-2", label: "Annual Report.pdf" },
        ],
      },
    ],
  },
  {
    id: "2",
    label: "Downloads",
    children: [
      { id: "2-1", label: "setup.exe" },
      { id: "2-2", label: "image.jpg" },
      { id: "2-3", label: "video.mp4" },
    ],
  },
  {
    id: "3",
    label: "Desktop",
    children: [{ id: "3-1", label: "shortcut.lnk" }],
  },
];

  return (
    <>
    <div className="max-w-xl mx-auto w-full">
      <TreeView
        data={treeData}
        onNodeClick={(node) => console.log("Clicked:", node.label)}
        defaultExpandedIds={["1"]}
      />
    </div>
    </>
  );
};

export { DemoOne };
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-slot framer-motion lucide-react
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
