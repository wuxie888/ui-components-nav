<!-- AI Conversation Tree · @elements- · https://21st.dev/@elements-/components/conversation-tree
     license: MIT · category: ai-chat
     A compound component that visualizes branching AI conversations as an expandable tree with node type indicators, active path highlighting, and click-to-navigate selection. -->

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
components/ui/ai-conversation-tree.tsx
"use client";

import * as React from "react";

import {
  Bot,
  ChevronRight,
  GitBranch,
  Settings,
  User,
  Wrench,
} from "lucide-react";

import { cn } from "@/lib/utils";

type NodeType = "user" | "assistant" | "system" | "tool";

interface ConversationNode {
  id: string;
  type: NodeType;
  content: string;
  children?: ConversationNode[];
  isActive?: boolean;
}

interface AiConversationTreeContextValue {
  nodes: ConversationNode[];
  activeNodeId?: string;
  expandedIds: Set<string>;
  toggleExpanded: (id: string) => void;
  onNodeSelect?: (nodeId: string) => void;
}

const AiConversationTreeContext =
  React.createContext<AiConversationTreeContextValue | null>(null);

function useConversationTreeContext() {
  const context = React.useContext(AiConversationTreeContext);
  if (!context) {
    throw new Error(
      "AiConversationTree components must be used within <AiConversationTree>",
    );
  }
  return context;
}

function collectAllNodeIds(nodes: ConversationNode[]): string[] {
  const ids: string[] = [];
  const traverse = (node: ConversationNode) => {
    ids.push(node.id);
    node.children?.forEach(traverse);
  };
  nodes.forEach(traverse);
  return ids;
}

function findActivePath(
  nodes: ConversationNode[],
  targetId: string,
): Set<string> {
  const path = new Set<string>();

  const traverse = (node: ConversationNode, currentPath: string[]): boolean => {
    const newPath = [...currentPath, node.id];

    if (node.id === targetId) {
      newPath.forEach((id) => path.add(id));
      return true;
    }

    if (node.children) {
      for (const child of node.children) {
        if (traverse(child, newPath)) {
          return true;
        }
      }
    }

    return false;
  };

  nodes.forEach((node) => traverse(node, []));
  return path;
}

interface AiConversationTreeProps {
  nodes: ConversationNode[];
  activeNodeId?: string;
  onNodeSelect?: (nodeId: string) => void;
  defaultExpandAll?: boolean;
  className?: string;
  children?: React.ReactNode;
}

function AiConversationTree({
  nodes,
  activeNodeId,
  onNodeSelect,
  defaultExpandAll = true,
  className,
  children,
}: AiConversationTreeProps) {
  const [expandedIds, setExpandedIds] = React.useState<Set<string>>(() => {
    if (defaultExpandAll) {
      return new Set(collectAllNodeIds(nodes));
    }
    return new Set();
  });

  const toggleExpanded = React.useCallback((id: string) => {
    setExpandedIds((prev) => {
      const next = new Set(prev);
      if (next.has(id)) {
        next.delete(id);
      } else {
        next.add(id);
      }
      return next;
    });
  }, []);

  const contextValue = React.useMemo(
    () => ({ nodes, activeNodeId, expandedIds, toggleExpanded, onNodeSelect }),
    [nodes, activeNodeId, expandedIds, toggleExpanded, onNodeSelect],
  );

  return (
    <AiConversationTreeContext.Provider value={contextValue}>
      <div
        data-slot="ai-conversation-tree"
        className={cn(
          "rounded-lg border border-border bg-card text-card-foreground",
          className,
        )}
      >
        {children}
      </div>
    </AiConversationTreeContext.Provider>
  );
}

interface AiConversationTreeHeaderProps {
  title?: string;
  children?: React.ReactNode;
  className?: string;
}

function AiConversationTreeHeader({
  title = "Conversation Tree",
  children,
  className,
}: AiConversationTreeHeaderProps) {
  const { nodes } = useConversationTreeContext();

  const stats = React.useMemo(() => {
    let totalNodes = 0;
    let branches = 0;

    const traverse = (node: ConversationNode) => {
      totalNodes++;
      if (node.children && node.children.length > 1) {
        branches++;
      }
      node.children?.forEach(traverse);
    };

    nodes.forEach(traverse);
    return { totalNodes, branches };
  }, [nodes]);

  return (
    <div
      data-slot="ai-conversation-tree-header"
      className={cn(
        "flex items-center gap-3 px-4 py-3 border-b border-border",
        className,
      )}
    >
      <div className="flex size-8 shrink-0 items-center justify-center rounded-md bg-cyan-100 dark:bg-cyan-950">
        <GitBranch className="size-4 text-cyan-600 dark:text-cyan-400" />
      </div>
      <div className="flex flex-1 items-center gap-2">
        <span className="font-medium text-sm">{title}</span>
        <span className="inline-flex items-center gap-1 rounded-full bg-muted px-2 py-0.5 text-xs font-medium text-muted-foreground">
          {stats.totalNodes} messages
          {stats.branches > 0 && ` / ${stats.branches} branches`}
        </span>
      </div>
      {children}
    </div>
  );
}

interface AiConversationTreeContentProps {
  className?: string;
}

function AiConversationTreeContent({
  className,
}: AiConversationTreeContentProps) {
  const { nodes, activeNodeId } = useConversationTreeContext();
  const activePath = React.useMemo(
    () =>
      activeNodeId ? findActivePath(nodes, activeNodeId) : new Set<string>(),
    [nodes, activeNodeId],
  );

  return (
    <div
      data-slot="ai-conversation-tree-content"
      className={cn("p-2", className)}
    >
      {nodes.map((node) => (
        <AiConversationTreeNode
          key={node.id}
          node={node}
          depth={0}
          activePath={activePath}
        />
      ))}
    </div>
  );
}

interface AiConversationTreeNodeProps {
  node: ConversationNode;
  depth: number;
  activePath: Set<string>;
  className?: string;
}

function AiConversationTreeNode({
  node,
  depth,
  activePath,
  className,
}: AiConversationTreeNodeProps) {
  const { activeNodeId, expandedIds, toggleExpanded, onNodeSelect } =
    useConversationTreeContext();

  const hasChildren = node.children && node.children.length > 0;
  const isExpanded = expandedIds.has(node.id);
  const isActive = node.id === activeNodeId;
  const isInActivePath = activePath.has(node.id);

  const nodeConfig = React.useMemo(() => {
    const configs: Record<
      NodeType,
      { icon: React.ReactNode; label: string; className: string }
    > = {
      user: {
        icon: <User className="size-3.5" />,
        label: "User",
        className:
          "bg-blue-100 text-blue-700 dark:bg-blue-950 dark:text-blue-300",
      },
      assistant: {
        icon: <Bot className="size-3.5" />,
        label: "Assistant",
        className:
          "bg-purple-100 text-purple-700 dark:bg-purple-950 dark:text-purple-300",
      },
      system: {
        icon: <Settings className="size-3.5" />,
        label: "System",
        className:
          "bg-gray-100 text-gray-700 dark:bg-gray-800 dark:text-gray-300",
      },
      tool: {
        icon: <Wrench className="size-3.5" />,
        label: "Tool",
        className:
          "bg-amber-100 text-amber-700 dark:bg-amber-950 dark:text-amber-300",
      },
    };
    return configs[node.type];
  }, [node.type]);

  const handleClick = React.useCallback(() => {
    onNodeSelect?.(node.id);
  }, [node.id, onNodeSelect]);

  const handleToggle = React.useCallback(
    (e: React.MouseEvent) => {
      e.stopPropagation();
      toggleExpanded(node.id);
    },
    [node.id, toggleExpanded],
  );

  return (
    <div
      data-slot="ai-conversation-tree-node"
      data-type={node.type}
      data-active={isActive}
      className={cn("", className)}
    >
      <div
        className={cn(
          "group flex items-center gap-2 rounded-md px-2 py-1.5 text-sm transition-colors cursor-pointer",
          isActive
            ? "bg-accent text-accent-foreground"
            : isInActivePath
              ? "bg-muted/50"
              : "hover:bg-muted/50",
          depth > 0 && "ml-4",
        )}
        style={{ marginLeft: depth > 0 ? `${depth * 16}px` : undefined }}
        onClick={handleClick}
      >
        {hasChildren ? (
          <button
            type="button"
            onClick={handleToggle}
            className="flex size-5 shrink-0 items-center justify-center rounded hover:bg-muted"
          >
            <ChevronRight
              className={cn(
                "size-3.5 text-muted-foreground transition-transform duration-200",
                isExpanded && "rotate-90",
              )}
            />
          </button>
        ) : (
          <div className="size-5 shrink-0" />
        )}
        <div
          className={cn(
            "flex size-6 shrink-0 items-center justify-center rounded",
            nodeConfig.className,
          )}
        >
          {nodeConfig.icon}
        </div>
        <span className="flex-1 truncate text-xs">{node.content}</span>
        {hasChildren && (
          <span className="text-xs text-muted-foreground shrink-0">
            {node.children?.length}
          </span>
        )}
      </div>
      {hasChildren && isExpanded && (
        <div className="relative">
          <div
            className="absolute left-[18px] top-0 bottom-0 w-px bg-border"
            style={{ marginLeft: depth > 0 ? `${depth * 16}px` : undefined }}
          />
          {node.children?.map((child) => (
            <AiConversationTreeNode
              key={child.id}
              node={child}
              depth={depth + 1}
              activePath={activePath}
            />
          ))}
        </div>
      )}
    </div>
  );
}

interface AiConversationTreePreviewProps {
  className?: string;
}

function AiConversationTreePreview({
  className,
}: AiConversationTreePreviewProps) {
  const { nodes, activeNodeId } = useConversationTreeContext();

  const activeNode = React.useMemo(() => {
    if (!activeNodeId) return null;

    const findNode = (
      searchNodes: ConversationNode[],
    ): ConversationNode | null => {
      for (const node of searchNodes) {
        if (node.id === activeNodeId) return node;
        if (node.children) {
          const found = findNode(node.children);
          if (found) return found;
        }
      }
      return null;
    };

    return findNode(nodes);
  }, [nodes, activeNodeId]);

  if (!activeNode) {
    return (
      <div
        data-slot="ai-conversation-tree-preview"
        className={cn(
          "p-4 text-sm text-muted-foreground text-center",
          className,
        )}
      >
        Select a message to preview
      </div>
    );
  }

  const nodeConfig = {
    user: { label: "User", className: "text-blue-600 dark:text-blue-400" },
    assistant: {
      label: "Assistant",
      className: "text-purple-600 dark:text-purple-400",
    },
    system: { label: "System", className: "text-gray-600 dark:text-gray-400" },
    tool: { label: "Tool", className: "text-amber-600 dark:text-amber-400" },
  }[activeNode.type];

  return (
    <div
      data-slot="ai-conversation-tree-preview"
      className={cn("p-4 border-t border-border", className)}
    >
      <div className="flex items-center gap-2 mb-2">
        <span
          className={cn("text-xs font-medium uppercase", nodeConfig.className)}
        >
          {nodeConfig.label}
        </span>
        <span className="text-xs text-muted-foreground">#{activeNode.id}</span>
      </div>
      <p className="text-sm whitespace-pre-wrap">{activeNode.content}</p>
    </div>
  );
}

export {
  AiConversationTree,
  AiConversationTreeHeader,
  AiConversationTreeContent,
  AiConversationTreeNode,
  AiConversationTreePreview,
};
export type { AiConversationTreeProps, ConversationNode, NodeType };

demo.tsx
"use client";

import {
  AiConversationTree,
  AiConversationTreeContent,
  AiConversationTreeHeader,
  AiConversationTreePreview,
  type ConversationNode,
} from "@/components/ui/conversation-tree";

const DEMO_NODES: ConversationNode[] = [
  {
    id: "1",
    type: "user",
    content: "How do I implement auth?",
    isActive: true,
    children: [
      {
        id: "2",
        type: "assistant",
        content:
          "There are several approaches to implementing authentication. You could use OAuth, JWT, or session-based auth...",
        isActive: true,
        children: [
          {
            id: "3",
            type: "user",
            content: "What about OAuth?",
          },
          {
            id: "4",
            type: "user",
            content: "Tell me about JWT",
            isActive: true,
            children: [
              {
                id: "5",
                type: "assistant",
                content:
                  "JWT (JSON Web Tokens) are a compact, URL-safe means of representing claims between two parties...",
                isActive: true,
              },
            ],
          },
          {
            id: "6",
            type: "user",
            content: "Session-based auth?",
          },
        ],
      },
    ],
  },
];

export default function AiConversationTreeDemo() {
  return (
    <div className="flex w-full items-center justify-center p-6">
      <AiConversationTree
        nodes={DEMO_NODES}
        activeNodeId="5"
        defaultExpandAll
        className="w-full max-w-lg"
      >
        <AiConversationTreeHeader />
        <AiConversationTreeContent />
        <AiConversationTreePreview />
      </AiConversationTree>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
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
