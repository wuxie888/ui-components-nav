<!-- Tree · @haydenbleasel · https://21st.dev/@haydenbleasel/components/tree
     license: unspecified · category: team
     Here is Tree component -->

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
components/ui/index.tsx
"use client";

import { ChevronRight, File, Folder, FolderOpen } from "lucide-react";
import { AnimatePresence, motion } from "motion/react";
import {
  type ComponentProps,
  createContext,
  type HTMLAttributes,
  type ReactNode,
  useCallback,
  useContext,
  useId,
  useState,
} from "react";
import { cn } from "@/lib/utils";

type TreeContextType = {
  expandedIds: Set<string>;
  selectedIds: string[];
  toggleExpanded: (nodeId: string) => void;
  handleSelection: (nodeId: string, ctrlKey: boolean) => void;
  showLines?: boolean;
  showIcons?: boolean;
  selectable?: boolean;
  multiSelect?: boolean;
  indent?: number;
  animateExpand?: boolean;
};

const TreeContext = createContext<TreeContextType | undefined>(undefined);

const useTree = () => {
  const context = useContext(TreeContext);
  if (!context) {
    throw new Error("Tree components must be used within a TreeProvider");
  }
  return context;
};

type TreeNodeContextType = {
  nodeId: string;
  level: number;
  isLast: boolean;
  parentPath: boolean[];
};

const TreeNodeContext = createContext<TreeNodeContextType | undefined>(
  undefined
);

const useTreeNode = () => {
  const context = useContext(TreeNodeContext);
  if (!context) {
    throw new Error("TreeNode components must be used within a TreeNode");
  }
  return context;
};

export type TreeProviderProps = {
  children: ReactNode;
  defaultExpandedIds?: string[];
  showLines?: boolean;
  showIcons?: boolean;
  selectable?: boolean;
  multiSelect?: boolean;
  selectedIds?: string[];
  onSelectionChange?: (selectedIds: string[]) => void;
  indent?: number;
  animateExpand?: boolean;
  className?: string;
};

export const TreeProvider = ({
  children,
  defaultExpandedIds = [],
  showLines = true,
  showIcons = true,
  selectable = true,
  multiSelect = false,
  selectedIds,
  onSelectionChange,
  indent = 20,
  animateExpand = true,
  className,
}: TreeProviderProps) => {
  const [expandedIds, setExpandedIds] = useState<Set<string>>(
    new Set(defaultExpandedIds)
  );
  const [internalSelectedIds, setInternalSelectedIds] = useState<string[]>(
    selectedIds ?? []
  );

  const isControlled =
    selectedIds !== undefined && onSelectionChange !== undefined;
  const currentSelectedIds = isControlled ? selectedIds : internalSelectedIds;

  const toggleExpanded = useCallback((nodeId: string) => {
    setExpandedIds((prev) => {
      const newSet = new Set(prev);
      if (newSet.has(nodeId)) {
        newSet.delete(nodeId);
      } else {
        newSet.add(nodeId);
      }
      return newSet;
    });
  }, []);

  const handleSelection = useCallback(
    (nodeId: string, ctrlKey = false) => {
      if (!selectable) {
        return;
      }

      let newSelection: string[];

      if (multiSelect && ctrlKey) {
        newSelection = currentSelectedIds.includes(nodeId)
          ? currentSelectedIds.filter((id) => id !== nodeId)
          : [...currentSelectedIds, nodeId];
      } else {
        newSelection = currentSelectedIds.includes(nodeId) ? [] : [nodeId];
      }

      if (isControlled) {
        onSelectionChange?.(newSelection);
      } else {
        setInternalSelectedIds(newSelection);
      }
    },
    [
      selectable,
      multiSelect,
      currentSelectedIds,
      isControlled,
      onSelectionChange,
    ]
  );

  return (
    <TreeContext.Provider
      value={{
        expandedIds,
        selectedIds: currentSelectedIds,
        toggleExpanded,
        handleSelection,
        showLines,
        showIcons,
        selectable,
        multiSelect,
        indent,
        animateExpand,
      }}
    >
      <motion.div
        animate={{ opacity: 1, y: 0 }}
        className={cn("w-full", className)}
        initial={{ opacity: 0, y: 10 }}
        transition={{ duration: 0.3, ease: "easeOut" }}
      >
        {children}
      </motion.div>
    </TreeContext.Provider>
  );
};

export type TreeViewProps = HTMLAttributes<HTMLDivElement>;

export const TreeView = ({ className, children, ...props }: TreeViewProps) => (
  <div className={cn("p-2", className)} {...props}>
    {children}
  </div>
);

export type TreeNodeProps = HTMLAttributes<HTMLDivElement> & {
  nodeId?: string;
  level?: number;
  isLast?: boolean;
  parentPath?: boolean[];
  children?: ReactNode;
};

export const TreeNode = ({
  nodeId: providedNodeId,
  level = 0,
  isLast = false,
  parentPath = [],
  children,
  className,
  onClick,
  ...props
}: TreeNodeProps) => {
  const generatedId = useId();
  const nodeId = providedNodeId ?? generatedId;

  // Build the parent path - mark positions where the parent was the last child
  const currentPath = level === 0 ? [] : [...parentPath];
  if (level > 0 && parentPath.length < level - 1) {
    // Fill in missing levels with false (not last)
    while (currentPath.length < level - 1) {
      currentPath.push(false);
    }
  }
  if (level > 0) {
    currentPath[level - 1] = isLast;
  }

  return (
    <TreeNodeContext.Provider
      value={{
        nodeId,
        level,
        isLast,
        parentPath: currentPath,
      }}
    >
      <div className={cn("select-none", className)} {...props}>
        {children}
      </div>
    </TreeNodeContext.Provider>
  );
};

export type TreeNodeTriggerProps = ComponentProps<typeof motion.div>;

export const TreeNodeTrigger = ({
  children,
  className,
  onClick,
  ...props
}: TreeNodeTriggerProps) => {
  const { selectedIds, toggleExpanded, handleSelection, indent } = useTree();
  const { nodeId, level } = useTreeNode();
  const isSelected = selectedIds.includes(nodeId);

  return (
    <motion.div
      className={cn(
        "group relative mx-1 flex cursor-pointer items-center rounded-md px-3 py-2 transition-all duration-200",
        "hover:bg-accent/50",
        isSelected && "bg-accent/80",
        className
      )}
      onClick={(e) => {
        toggleExpanded(nodeId);
        handleSelection(nodeId, e.ctrlKey || e.metaKey);
        onClick?.(e);
      }}
      style={{ paddingLeft: level * (indent ?? 0) + 8 }}
      whileTap={{ scale: 0.98, transition: { duration: 0.1 } }}
      {...props}
    >
      <TreeLines />
      {children as ReactNode}
    </motion.div>
  );
};

export const TreeLines = () => {
  const { showLines, indent } = useTree();
  const { level, isLast, parentPath } = useTreeNode();

  if (!showLines || level === 0) {
    return null;
  }

  return (
    <div className="pointer-events-none absolute top-0 bottom-0 left-0">
      {/* Render vertical lines for all parent levels */}
      {Array.from({ length: level }, (_, index) => {
        const shouldHideLine = parentPath[index] === true;
        if (shouldHideLine && index === level - 1) {
          return null;
        }

        return (
          <div
            className="absolute top-0 bottom-0 border-border/40 border-l"
            key={index.toString()}
            style={{
              left: index * (indent ?? 0) + 12,
              display: shouldHideLine ? "none" : "block",
            }}
          />
        );
      })}

      {/* Horizontal connector line */}
      <div
        className="absolute top-1/2 border-border/40 border-t"
        style={{
          left: (level - 1) * (indent ?? 0) + 12,
          width: (indent ?? 0) - 4,
          transform: "translateY(-1px)",
        }}
      />

      {/* Vertical line to midpoint for last items */}
      {isLast && (
        <div
          className="absolute top-0 border-border/40 border-l"
          style={{
            left: (level - 1) * (indent ?? 0) + 12,
            height: "50%",
          }}
        />
      )}
    </div>
  );
};

export type TreeNodeContentProps = ComponentProps<typeof motion.div> & {
  hasChildren?: boolean;
};

export const TreeNodeContent = ({
  children,
  hasChildren = false,
  className,
  ...props
}: TreeNodeContentProps) => {
  const { animateExpand, expandedIds } = useTree();
  const { nodeId } = useTreeNode();
  const isExpanded = expandedIds.has(nodeId);

  return (
    <AnimatePresence>
      {hasChildren && isExpanded && (
        <motion.div
          animate={{ height: "auto", opacity: 1 }}
          className="overflow-hidden"
          exit={{ height: 0, opacity: 0 }}
          initial={{ height: 0, opacity: 0 }}
          transition={{
            duration: animateExpand ? 0.3 : 0,
            ease: "easeInOut",
          }}
        >
          <motion.div
            animate={{ y: 0 }}
            className={className}
            exit={{ y: -10 }}
            initial={{ y: -10 }}
            transition={{
              duration: animateExpand ? 0.2 : 0,
              delay: animateExpand ? 0.1 : 0,
            }}
            {...props}
          >
            {children}
          </motion.div>
        </motion.div>
      )}
    </AnimatePresence>
  );
};

export type TreeExpanderProps = ComponentProps<typeof motion.div> & {
  hasChildren?: boolean;
};

export const TreeExpander = ({
  hasChildren = false,
  className,
  onClick,
  ...props
}: TreeExpanderProps) => {
  const { expandedIds, toggleExpanded } = useTree();
  const { nodeId } = useTreeNode();
  const isExpanded = expandedIds.has(nodeId);

  if (!hasChildren) {
    return <div className="mr-1 h-4 w-4" />;
  }

  return (
    <motion.div
      animate={{ rotate: isExpanded ? 90 : 0 }}
      className={cn(
        "mr-1 flex h-4 w-4 cursor-pointer items-center justify-center",
        className
      )}
      onClick={(e) => {
        e.stopPropagation();
        toggleExpanded(nodeId);
        onClick?.(e);
      }}
      transition={{ duration: 0.2, ease: "easeInOut" }}
      {...props}
    >
      <ChevronRight className="h-3 w-3 text-muted-foreground" />
    </motion.div>
  );
};

export type TreeIconProps = ComponentProps<typeof motion.div> & {
  icon?: ReactNode;
  hasChildren?: boolean;
};

export const TreeIcon = ({
  icon,
  hasChildren = false,
  className,
  ...props
}: TreeIconProps) => {
  const { showIcons, expandedIds } = useTree();
  const { nodeId } = useTreeNode();
  const isExpanded = expandedIds.has(nodeId);

  if (!showIcons) {
    return null;
  }

  const getDefaultIcon = () =>
    hasChildren ? (
      isExpanded ? (
        <FolderOpen className="h-4 w-4" />
      ) : (
        <Folder className="h-4 w-4" />
      )
    ) : (
      <File className="h-4 w-4" />
    );

  return (
    <motion.div
      className={cn(
        "mr-2 flex h-4 w-4 items-center justify-center text-muted-foreground",
        className
      )}
      transition={{ duration: 0.15 }}
      whileHover={{ scale: 1.1 }}
      {...props}
    >
      {icon || getDefaultIcon()}
    </motion.div>
  );
};

export type TreeLabelProps = HTMLAttributes<HTMLSpanElement>;

export const TreeLabel = ({ className, ...props }: TreeLabelProps) => (
  <span className={cn("font flex-1 truncate text-sm", className)} {...props} />
);

demo.tsx
'use client';

import {
  TreeExpander,
  TreeIcon,
  TreeLabel,
  TreeNode,
  TreeNodeContent,
  TreeNodeTrigger,
  TreeProvider,
  TreeView,
} from '@/components/ui/tree';
import { FileCode, FileJson, FileText, FileType } from 'lucide-react';

export default function TreeExample() {
  return (
    <TreeProvider
      defaultExpandedIds={['src', 'components', 'ui']}
      onSelectionChange={(ids) => console.log('Selected:', ids)}
    >
      <TreeView>
        <TreeNode nodeId="src">
          <TreeNodeTrigger>
            <TreeExpander hasChildren />
            <TreeIcon hasChildren />
            <TreeLabel>src</TreeLabel>
          </TreeNodeTrigger>
          <TreeNodeContent hasChildren>
            <TreeNode level={1} nodeId="components">
              <TreeNodeTrigger>
                <TreeExpander hasChildren />
                <TreeIcon hasChildren />
                <TreeLabel>components</TreeLabel>
              </TreeNodeTrigger>
              <TreeNodeContent hasChildren>
                <TreeNode level={2} nodeId="ui">
                  <TreeNodeTrigger>
                    <TreeExpander hasChildren />
                    <TreeIcon hasChildren />
                    <TreeLabel>ui</TreeLabel>
                  </TreeNodeTrigger>
                  <TreeNodeContent hasChildren>
                    <TreeNode level={3} nodeId="button.tsx">
                      <TreeNodeTrigger>
                        <TreeExpander />
                        <TreeIcon icon={<FileCode className="h-4 w-4" />} />
                        <TreeLabel>button.tsx</TreeLabel>
                      </TreeNodeTrigger>
                    </TreeNode>
                    <TreeNode level={3} nodeId="card.tsx">
                      <TreeNodeTrigger>
                        <TreeExpander />
                        <TreeIcon icon={<FileCode className="h-4 w-4" />} />
                        <TreeLabel>card.tsx</TreeLabel>
                      </TreeNodeTrigger>
                    </TreeNode>
                    <TreeNode isLast level={3} nodeId="dialog.tsx">
                      <TreeNodeTrigger>
                        <TreeExpander />
                        <TreeIcon icon={<FileCode className="h-4 w-4" />} />
                        <TreeLabel>dialog.tsx</TreeLabel>
                      </TreeNodeTrigger>
                    </TreeNode>
                  </TreeNodeContent>
                </TreeNode>
                <TreeNode isLast level={2} nodeId="layout">
                  <TreeNodeTrigger>
                    <TreeExpander hasChildren />
                    <TreeIcon hasChildren />
                    <TreeLabel>layout</TreeLabel>
                  </TreeNodeTrigger>
                  <TreeNodeContent hasChildren>
                    <TreeNode level={3} nodeId="header.tsx">
                      <TreeNodeTrigger>
                        <TreeExpander />
                        <TreeIcon icon={<FileCode className="h-4 w-4" />} />
                        <TreeLabel>header.tsx</TreeLabel>
                      </TreeNodeTrigger>
                    </TreeNode>
                    <TreeNode isLast level={3} nodeId="footer.tsx">
                      <TreeNodeTrigger>
                        <TreeExpander />
                        <TreeIcon icon={<FileCode className="h-4 w-4" />} />
                        <TreeLabel>footer.tsx</TreeLabel>
                      </TreeNodeTrigger>
                    </TreeNode>
                  </TreeNodeContent>
                </TreeNode>
              </TreeNodeContent>
            </TreeNode>
          </TreeNodeContent>
        </TreeNode>
        <TreeNode nodeId="public">
          <TreeNodeTrigger>
            <TreeExpander hasChildren />
            <TreeIcon hasChildren />
            <TreeLabel>public</TreeLabel>
          </TreeNodeTrigger>
          <TreeNodeContent hasChildren>
            <TreeNode isLast level={1} nodeId="images">
              <TreeNodeTrigger>
                <TreeExpander hasChildren />
                <TreeIcon hasChildren />
                <TreeLabel>images</TreeLabel>
              </TreeNodeTrigger>
              <TreeNodeContent hasChildren>
                <TreeNode level={2} nodeId="logo.svg">
                  <TreeNodeTrigger>
                    <TreeExpander />
                    <TreeIcon icon={<FileText className="h-4 w-4" />} />
                    <TreeLabel>logo.svg</TreeLabel>
                  </TreeNodeTrigger>
                </TreeNode>
                <TreeNode isLast level={2} nodeId="hero.png">
                  <TreeNodeTrigger>
                    <TreeExpander />
                    <TreeIcon icon={<FileText className="h-4 w-4" />} />
                    <TreeLabel>hero.png</TreeLabel>
                  </TreeNodeTrigger>
                </TreeNode>
              </TreeNodeContent>
            </TreeNode>
          </TreeNodeContent>
        </TreeNode>
        <TreeNode nodeId="package.json">
          <TreeNodeTrigger>
            <TreeExpander />
            <TreeIcon icon={<FileJson className="h-4 w-4" />} />
            <TreeLabel>package.json</TreeLabel>
          </TreeNodeTrigger>
        </TreeNode>
        <TreeNode nodeId="tsconfig.json">
          <TreeNodeTrigger>
            <TreeExpander />
            <TreeIcon icon={<FileJson className="h-4 w-4" />} />
            <TreeLabel>tsconfig.json</TreeLabel>
          </TreeNodeTrigger>
        </TreeNode>
        <TreeNode isLast nodeId="README.md">
          <TreeNodeTrigger>
            <TreeExpander />
            <TreeIcon icon={<FileText className="h-4 w-4" />} />
            <TreeLabel>README.md</TreeLabel>
          </TreeNodeTrigger>
        </TreeNode>
      </TreeView>
    </TreeProvider>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
