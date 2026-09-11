<!-- Table of Contents · @inference-sh · https://21st.dev/@inference-sh/components/table-of-contents
     license: no-license · category: navigation-menu
     An auto-scrolling table of contents that highlights the active section using an intersection observer as the reader scrolls. -->

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
components/ui/table-of-contents.tsx
"use client"

import { cn } from '../utils'
import { useEffect, useMemo, useState } from 'react'
import { TableOfContentsIcon } from 'lucide-react'

export interface TocItem {
  id: string
  title: string
  level?: number
  children?: TocItem[]
}

interface TableOfContentsProps {
  /** Markdown content to extract headings from */
  content?: string
  /** Pre-defined items (alternative to content extraction) */
  items?: TocItem[]
  /** Optional header slot (e.g., language selector) */
  header?: React.ReactNode
  /** Optional footer slot (e.g., link to full docs) */
  footer?: React.ReactNode
  className?: string
}

// Extract headings from markdown content (h2 and h3 only, h1 is page title)
// Nests h3s under their preceding h2
function extractHeadings(content: string): TocItem[] {
  const result: TocItem[] = []
  const lines = content.split('\n')
  let currentH2: TocItem | null = null

  for (const line of lines) {
    const match = line.match(/^(#{2,3})\s+(.+)$/)
    if (match) {
      const level = match[1].length
      const title = match[2].trim()
      // Create a URL-safe ID from the heading text
      const id = title
        .toLowerCase()
        .replace(/[^a-z0-9]+/g, '-')
        .replace(/(^-|-$)/g, '')

      if (level === 2) {
        currentH2 = { id, title, level, children: [] }
        result.push(currentH2)
      } else if (level === 3) {
        const item = { id, title, level }
        if (currentH2) {
          currentH2.children = currentH2.children || []
          currentH2.children.push(item)
        } else {
          // h3 without preceding h2, add to root
          result.push(item)
        }
      }
    }
  }

  return result
}

// Flatten nested items for intersection observer
function flattenItems(items: TocItem[]): TocItem[] {
  return items.flatMap(item => [item, ...(item.children || [])])
}

export function TableOfContents({ content, items, header, footer, className }: TableOfContentsProps) {
  const [activeId, setActiveId] = useState<string>('')

  // Use provided items or extract from markdown
  const tocItems = useMemo(() => {
    if (items) return items
    if (content) return extractHeadings(content)
    return []
  }, [items, content])

  // Flatten for observer
  const allItems = useMemo(() => flattenItems(tocItems), [tocItems])

  useEffect(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            setActiveId(entry.target.id)
          }
        })
      },
      {
        rootMargin: '-80px 0px -80% 0px',
        threshold: 0,
      }
    )

    // Observe all heading elements
    allItems.forEach(({ id }) => {
      const element = document.getElementById(id)
      if (element) {
        observer.observe(element)
      }
    })

    return () => observer.disconnect()
  }, [allItems])

  if (tocItems.length === 0) {
    return null
  }

  const renderItem = (item: TocItem, isChild = false) => {
    const isActive = activeId === item.id
    const level = item.level || (isChild ? 3 : 2)

    return (
      <li key={item.id}>
        <a
          href={`#${item.id}`}
          onClick={(e) => {
            e.preventDefault()
            const element = document.getElementById(item.id)
            if (element) {
              element.scrollIntoView({ behavior: 'smooth' })
              setActiveId(item.id)
            }
          }}
          className={cn(
            'flex items-center text-[13px] py-1 -ml-px border-l transition-colors lowercase',
            !isChild && 'pl-3 font-medium',
            isChild && 'pl-4 text-xs',
            isActive
              ? 'border-brand-pink text-brand-pink'
              : 'border-transparent text-muted-foreground hover:text-foreground hover:border-muted-foreground'
          )}
        >
          {isChild && (
            <span className={cn(
              "mr-1.5 text-[10px] font-mono",
              isActive ? "text-brand-pink" : "text-muted-foreground/50"
            )}>
              └
            </span>
          )}
          {item.title}
        </a>
        {item.children && item.children.length > 0 && (
          <ul className="space-y-0.5">
            {item.children.map(child => renderItem(child, true))}
          </ul>
        )}
      </li>
    )
  }

  return (
    <nav className={cn('relative', className)}>
      {header}

      <div className="flex items-center gap-2 mb-3">
        <TableOfContentsIcon className="h-4 w-4 text-muted-foreground" />
        <p className="text-xs font-bold text-muted-foreground">
          on this page
        </p>
      </div>

      <ul className="space-y-1 border-l border-border">
        {tocItems.map(item => renderItem(item))}
      </ul>

      {footer}
    </nav>
  )
}

demo.tsx
"use client";

import { TableOfContents } from "@/components/ui/table-of-contents";

const items = [
  { id: "introduction", title: "Introduction", level: 1 },
  { id: "installation", title: "Installation", level: 1 },
  { id: "install-npm", title: "npm", level: 2 },
  { id: "install-yarn", title: "yarn", level: 2 },
  { id: "usage", title: "Usage", level: 1 },
  { id: "usage-basic", title: "Basic example", level: 2 },
  { id: "usage-advanced", title: "Advanced example", level: 2 },
  { id: "api", title: "API Reference", level: 1 },
];

export default function TableOfContentsDemo() {
  return (
    <div className="flex w-full items-center justify-center bg-background p-10">
      <div className="w-full max-w-xs rounded-xl border border-border bg-card p-6 shadow-sm">
        <TableOfContents items={items} />
      </div>
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
