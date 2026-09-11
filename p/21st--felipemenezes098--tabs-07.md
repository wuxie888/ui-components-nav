<!-- Tabs with Count Badges · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/tabs-07
     license: no-license · category: navigation-menu
     A line-style tabbed interface whose trigger labels are paired with secondary count badges, ideal for inbox, drafts, and archive style views. -->

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
components/ui/tabs-07.tsx
import { Badge } from '@/components/ui/badge'
import {
  Tabs,
  TabsContent,
  TabsList,
  TabsTrigger,
} from '@/components/ui/tabs'

export function Tabs07() {
  return (
    <Tabs defaultValue="inbox" className="w-full max-w-md">
      <TabsList variant="line">
        <TabsTrigger value="inbox">
          Inbox
          <Badge variant="secondary">12</Badge>
        </TabsTrigger>
        <TabsTrigger value="drafts">
          Drafts
          <Badge variant="secondary">3</Badge>
        </TabsTrigger>
        <TabsTrigger value="archive">Archive</TabsTrigger>
      </TabsList>
      <TabsContent value="inbox" className="w-full pt-4">
        <ul className="divide-border/60 w-full divide-y rounded-lg border">
          <li className="flex items-center gap-3 px-3 py-2.5 first:rounded-t-lg">
            <span className="bg-primary size-2 shrink-0 rounded-full" aria-hidden />
            <span className="truncate text-sm font-medium">
              Q2 roadmap review
            </span>
          </li>
          <li className="flex items-center gap-3 px-3 py-2.5">
            <span className="bg-primary size-2 shrink-0 rounded-full" aria-hidden />
            <span className="truncate text-sm font-medium">
              Design system handoff
            </span>
          </li>
          <li className="flex items-center gap-3 px-3 py-2.5 last:rounded-b-lg">
            <span className="size-2 shrink-0 rounded-full" aria-hidden />
            <span className="text-muted-foreground truncate text-sm">
              Invoice #1042
            </span>
          </li>
        </ul>
      </TabsContent>
      <TabsContent value="drafts" className="w-full pt-4">
        <ul className="w-full space-y-2">
          <li className="text-muted-foreground rounded-lg border border-dashed px-3 py-2.5 text-sm">
            Weekly update
          </li>
          <li className="text-muted-foreground rounded-lg border border-dashed px-3 py-2.5 text-sm">
            Partnership intro
          </li>
        </ul>
      </TabsContent>
      <TabsContent value="archive" className="w-full pt-4">
        <div className="text-muted-foreground w-full rounded-lg border border-dashed px-4 py-8">
          <p className="text-sm font-medium text-foreground">All caught up</p>
          <p className="text-xs">Archived threads appear here.</p>
        </div>
      </TabsContent>
    </Tabs>
  )
}

demo.tsx
import { Tabs07 } from "@/components/ui/tabs-07";

export default function Default() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center bg-background p-10">
      <Tabs07 />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge flexnative-tabs flexnative-tabs?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068&publisher_install_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdXJwb3NlIjoicHVibGlzaGVyLXJlZ2lzdHJ5LWluc3RhbGwiLCJpYXQiOjE3ODcxMzI5MTYsImV4cCI6MTc4NzEzMzUxNn0.3Q_rzPLziW8vd4WP7avU8a6x4-Zmrq2tj7KWII2yMIg tabs
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
