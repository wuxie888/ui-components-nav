<!-- Sidebar Light · @inference-sh · https://21st.dev/@inference-sh/components/sidebar-light
     license: MIT · category: dashboard
     Lightweight sidebar navigation with nested items, icons, and active route highlighting. -->

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
components/ui/sidebar-light.tsx
import * as React from "react"
import { cn } from "@/lib/utils"

export interface NavItem {
  title: string
  href: string
  icon?: React.ComponentType<{ className?: string }>
  items?: NavItem[]
}

interface SidebarLightProps {
  items: NavItem[]
  pathname: string
  className?: string
  /** Link component to use - defaults to <a> tag. Pass your router's Link component. */
  LinkComponent?: React.ComponentType<{ href: string; className?: string; children: React.ReactNode }>
}

interface NavItemRendererProps {
  item: NavItem
  pathname: string
  depth: number
  LinkComponent: React.ComponentType<{ href: string; className?: string; children: React.ReactNode }>
}

function NavItemRenderer({ item, pathname, depth, LinkComponent }: NavItemRendererProps) {
  const hasChildren = item.items && item.items.length > 0
  const isActive = pathname === item.href

  // Parent item with children (section header)
  if (hasChildren) {
    return (
      <div className="space-y-1">
        <div
          className={cn(
            "flex items-center gap-2 text-sm",
            depth === 0 && "px-3 py-2 font-semibold text-foreground",
            depth > 0 && "px-3 py-1.5 font-medium text-muted-foreground"
          )}
        >
          {item.icon && <item.icon className={cn(depth === 0 ? "h-4 w-4" : "h-3.5 w-3.5")} />}
          {item.title}
        </div>
        <div className={cn("ml-4 space-y-1 border-l border-border pl-2")}>
          {item.items!.map((subItem, index) => (
            <NavItemRenderer
              key={subItem.href !== "#" ? subItem.href : `${subItem.title}-${index}`}
              item={subItem}
              pathname={pathname}
              depth={depth + 1}
              LinkComponent={LinkComponent}
            />
          ))}
        </div>
      </div>
    )
  }

  // Leaf item (link)
  return (
    <LinkComponent
      href={item.href}
      className={cn(
        "flex items-center gap-2 text-sm rounded-md transition-colors",
        depth === 0 && "px-3 py-2",
        depth > 0 && "px-3 py-1.5",
        isActive
          ? "bg-muted font-medium text-foreground"
          : "text-muted-foreground hover:text-foreground hover:bg-muted/50"
      )}
    >
      {item.icon && <item.icon className={cn(depth === 0 ? "h-4 w-4" : "h-3.5 w-3.5")} />}
      {item.title}
    </LinkComponent>
  )
}

// Default link component (plain <a> tag)
const DefaultLink: React.FC<{ href: string; className?: string; children: React.ReactNode }> = ({ href, className, children }) => (
  <a href={href} className={className}>{children}</a>
)

function SidebarLight({ items, pathname, className, LinkComponent = DefaultLink }: SidebarLightProps) {
  return (
    <aside className={cn("w-full", className)}>
      <nav className="space-y-1">
        {items.map((item, index) => (
          <NavItemRenderer
            key={item.href !== "#" ? item.href : `${item.title}-${index}`}
            item={item}
            pathname={pathname}
            depth={0}
            LinkComponent={LinkComponent}
          />
        ))}
      </nav>
    </aside>
  )
}

export { SidebarLight }
export type { SidebarLightProps }

demo.tsx
import { SidebarLight, type NavItem } from "@/components/ui/sidebar-light"
import { Home, BookOpen, Settings, LayoutGrid, FileText, Users } from "lucide-react"

const items: NavItem[] = [
  { title: "Dashboard", href: "/dashboard", icon: Home },
  {
    title: "Documentation",
    href: "#",
    icon: BookOpen,
    items: [
      { title: "Getting Started", href: "/dashboard" },
      { title: "Installation", href: "/docs/installation" },
      { title: "Components", href: "/docs/components" },
    ],
  },
  {
    title: "Projects",
    href: "#",
    icon: LayoutGrid,
    items: [
      { title: "Overview", href: "/projects/overview", icon: FileText },
      { title: "Team", href: "/projects/team", icon: Users },
    ],
  },
  { title: "Settings", href: "/settings", icon: Settings },
]

export default function SidebarLightDemo() {
  return (
    <div className="flex min-h-[420px] w-full justify-center bg-background p-6">
      <div className="w-64 rounded-lg border border-border p-3">
        <SidebarLight items={items} />
      </div>
    </div>
  )
}
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
