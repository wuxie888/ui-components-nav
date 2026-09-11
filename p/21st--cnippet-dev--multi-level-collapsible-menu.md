<!-- Multi-level Collapsible Menu · @cnippet-dev · https://21st.dev/@cnippet-dev/components/multi-level-collapsible-menu
     license: MIT · category: sidebar
     A nested, multi-level collapsible navigation menu inside a card, with expandable folders, animated chevrons, and selectable items. -->

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
components/ui/collapsible.tsx
"use client";

import { Collapsible as CollapsiblePrimitive } from "@base-ui/react/collapsible";
import type React from "react";
import { cn } from "@/registry/default/lib/utils";

export function Collapsible({
  ...props
}: CollapsiblePrimitive.Root.Props): React.ReactElement {
  return <CollapsiblePrimitive.Root data-slot="collapsible" {...props} />;
}

export function CollapsibleTrigger({
  className,
  ...props
}: CollapsiblePrimitive.Trigger.Props): React.ReactElement {
  return (
    <CollapsiblePrimitive.Trigger
      className={cn("cursor-pointer", className)}
      data-slot="collapsible-trigger"
      {...props}
    />
  );
}

export function CollapsiblePanel({
  className,
  ...props
}: CollapsiblePrimitive.Panel.Props): React.ReactElement {
  return (
    <CollapsiblePrimitive.Panel
      className={cn(
        "h-(--collapsible-panel-height) overflow-hidden transition-[height] duration-200 data-ending-style:h-0 data-starting-style:h-0",
        className,
      )}
      data-slot="collapsible-panel"
      {...props}
    />
  );
}

export { CollapsiblePanel as CollapsibleContent, CollapsiblePrimitive };

demo.tsx
"use client";

import {
  BellIcon,
  ChartBarIcon,
  ChevronRightIcon,
  CreditCardIcon,
  FileTextIcon,
  LayoutDashboardIcon,
  MessageSquareIcon,
  SettingsIcon,
  ShieldIcon,
  UserIcon,
} from "lucide-react";
import { type ReactElement, useState } from "react";
import {
  Card,
  CardContent,
} from "@/components/ui/multi-level-collapsible-menu-utils/card";
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from "@/components/ui/multi-level-collapsible-menu-utils/collapsible";
import {
  Item,
  ItemMedia,
  ItemTitle,
} from "@/components/ui/multi-level-collapsible-menu-utils/item";

type NavItem = {
  id: string;
  name: string;
  icon: ReactElement;
  items?: NavItem[];
};

const navItems: NavItem[] = [
  {
    icon: <LayoutDashboardIcon />,
    id: "dashboard",
    items: [
      {
        icon: <ChartBarIcon />,
        id: "analytics",
        items: [
          {
            icon: <FileTextIcon aria-hidden="true" />,
            id: "real-time",
            name: "Real-time",
          },
          {
            icon: <FileTextIcon aria-hidden="true" />,
            id: "historical",
            name: "Historical",
          },
        ],
        name: "Analytics",
      },
      {
        icon: <MessageSquareIcon aria-hidden="true" />,
        id: "reports",
        name: "Reports",
      },
    ],
    name: "Dashboard",
  },
  {
    icon: <UserIcon aria-hidden="true" />,
    id: "team",
    items: [
      { icon: <UserIcon aria-hidden="true" />, id: "members", name: "Members" },
      {
        icon: <ShieldIcon aria-hidden="true" />,
        id: "permissions",
        name: "Permissions",
      },
    ],
    name: "Team",
  },
  {
    icon: <CreditCardIcon aria-hidden="true" />,
    id: "billing",
    name: "Billing",
  },
  {
    icon: <SettingsIcon aria-hidden="true" />,
    id: "settings",
    name: "Settings",
  },
  {
    icon: <BellIcon aria-hidden="true" />,
    id: "notifications",
    name: "Notifications",
  },
];

const defaultOpenIds = new Set(["dashboard", "analytics"]);

function NavMenuItem({
  item,
  level = 0,
  selectedId,
  onSelect,
}: {
  item: NavItem;
  level?: number;
  selectedId: string | null;
  onSelect: (id: string) => void;
}) {
  const isFolder = !!item.items && item.items.length > 0;
  const isSelected = selectedId === item.id;

  if (isFolder) {
    return (
      <Collapsible
        className="group/collapsible"
        defaultOpen={defaultOpenIds.has(item.id)}
      >
        <CollapsibleTrigger
          nativeButton={false}
          render={
            <Item
              className="group/item cursor-pointer py-1.25 hover:bg-accent data-[state=open]:bg-accent"
              size="xs"
              style={{ paddingLeft: `${level * 12 + 8}px` }}
            />
          }
        >
          <ItemMedia variant="icon">
            <div className="size-3.5 text-muted-foreground group-hover/item:text-foreground">
              {item.icon}
            </div>
          </ItemMedia>
          <ItemTitle className="text-sm data-[state=open]/collapsible:font-semibold">
            {item.name}
          </ItemTitle>
          <ChevronRightIcon
            aria-hidden="true"
            className="ml-auto size-4 in-data-open:rotate-90 text-muted-foreground transition-transform"
          />
        </CollapsibleTrigger>
        <CollapsibleContent className="overflow-hidden pt-0.5 data-[state=closed]:animate-collapsible-up data-[state=open]:animate-collapsible-down">
          <div className="flex flex-col gap-0.5">
            {item.items?.map((child) => (
              <NavMenuItem
                item={child}
                key={child.id}
                level={level + 1}
                onSelect={onSelect}
                selectedId={selectedId}
              />
            ))}
          </div>
        </CollapsibleContent>
      </Collapsible>
    );
  }

  return (
    <Item
      className="group/item cursor-pointer py-1.25 hover:bg-accent data-[active=true]:bg-accent data-[active=true]:text-foreground"
      data-active={isSelected}
      onClick={() => onSelect(item.id)}
      size="xs"
      style={{ paddingLeft: `${level * 12 + 8}px` }}
    >
      <ItemMedia variant="icon">
        <div className="size-3.5 text-muted-foreground group-hover/item:text-foreground group-data-[active=true]/item:text-foreground">
          {item.icon}
        </div>
      </ItemMedia>
      <ItemTitle className="text-sm">{item.name}</ItemTitle>
    </Item>
  );
}

function ExpandedMenu() {
  const [selectedId, setSelectedId] = useState<string | null>("real-time");

  return (
    <div className="min-h-64 w-full max-w-56">
      <Card className="p-0">
        <CardContent className="p-1">
          <div className="flex flex-col gap-0.5">
            {navItems.map((item) => (
              <NavMenuItem
                item={item}
                key={item.id}
                onSelect={setSelectedId}
                selectedId={selectedId}
              />
            ))}
          </div>
        </CardContent>
      </Card>
    </div>
  );
}

export default function MultiLevelCollapsibleMenuDemo() {
  return (
    <div className="flex min-h-96 w-full items-center justify-center p-6">
      <ExpandedMenu />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add card collapsible separator
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
