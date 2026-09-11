<!-- Email Inbox Skeleton · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-skeleton-13
     license: no-license · category: list
     A loading placeholder skeleton for an email inbox list with avatars, subject lines, and unread indicators. -->

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
components/ui/v-skeleton-13.tsx
import { Skeleton } from "@/registry/default/ui/skeleton";

const rows = [
  { preview: 80, read: false, subject: 56 },
  { preview: 72, read: false, subject: 48 },
  { preview: 88, read: true, subject: 64 },
  { preview: 60, read: true, subject: 40 },
  { preview: 76, read: true, subject: 52 },
  { preview: 68, read: true, subject: 44 },
];

export default function Particle() {
  return (
    <div className="w-full max-w-lg overflow-hidden rounded-xl border">
      <div className="flex items-center justify-between border-b bg-muted/30 px-4 py-3">
        <Skeleton className="h-4 w-12" />
        <div className="flex items-center gap-2">
          <Skeleton className="h-7 w-20 rounded-md" />
          <Skeleton className="h-7 w-7 rounded-md" />
        </div>
      </div>
      {rows.map(({ preview, read, subject }, i) => (
        <div
          className={`flex items-start gap-3 border-b px-4 py-3 last:border-b-0 ${!read ? "bg-accent/20" : ""}`}
          key={String(i)}
        >
          <Skeleton className="mt-0.5 size-8 shrink-0 rounded-full" />
          <div className="min-w-0 flex-1 space-y-1.5">
            <div className="flex items-center justify-between gap-2">
              <Skeleton className="h-3.5 w-28" />
              <Skeleton className="h-3 w-12 shrink-0" />
            </div>
            <Skeleton className={"h-3.5"} style={{ width: `${subject}%` }} />
            <Skeleton className={"h-3"} style={{ width: `${preview}%` }} />
          </div>
          {!read && (
            <Skeleton className="mt-1.5 size-2 shrink-0 rounded-full" />
          )}
        </div>
      ))}
    </div>
  );
}

demo.tsx
import Particle from "@/components/ui/v-skeleton-13";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center bg-background p-6">
      <Particle />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add skeleton
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
