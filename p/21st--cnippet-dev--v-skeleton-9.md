<!-- Chat Thread Skeleton · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-skeleton-9
     license: MIT · category: input
     A chat conversation loading placeholder with avatar, message bubbles, and input bar built from shimmer skeletons. -->

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
components/ui/v-skeleton-9.tsx
//biome-ignore-all lint/suspicious/noArrayIndexKey: <>

import { Skeleton } from "@/registry/default/ui/skeleton";

const messages = [
  { lines: [72, 56], self: false },
  { lines: [88], self: true },
  { lines: [64, 80, 48], self: false },
  { lines: [76, 60], self: true },
  { lines: [52], self: false },
];

export function Pattern() {
  return (
    <div className="flex w-full max-w-sm flex-col overflow-hidden rounded-xl border">
      <div className="flex items-center gap-3 border-b px-4 py-3">
        <Skeleton className="size-8 rounded-full" />
        <div className="space-y-1.5">
          <Skeleton className="h-3.5 w-28" />
          <Skeleton className="h-3 w-16" />
        </div>
      </div>

      <div className="flex flex-1 flex-col gap-4 px-4 py-4">
        {messages.map((msg, i) => (
          <div
            className={`flex items-end gap-2 ${msg.self ? "flex-row-reverse" : ""}`}
            key={i}
          >
            {!msg.self && (
              <Skeleton className="mb-0.5 size-7 shrink-0 rounded-full" />
            )}
            <div
              className={`flex max-w-[75%] flex-col gap-1 ${msg.self ? "items-end" : "items-start"}`}
            >
              {msg.lines.map((w, j) => (
                <Skeleton
                  className={`h-8 rounded-2xl ${
                    msg.self
                      ? j === 0
                        ? "rounded-tr-sm"
                        : j === msg.lines.length - 1
                          ? "rounded-br-sm"
                          : ""
                      : j === 0
                        ? "rounded-tl-sm"
                        : j === msg.lines.length - 1
                          ? "rounded-bl-sm"
                          : ""
                  }`}
                  key={j}
                  style={{ width: `${w}%` }}
                />
              ))}
            </div>
          </div>
        ))}
      </div>

      <div className="flex items-center gap-2 border-t px-3 py-2.5">
        <Skeleton className="h-9 flex-1 rounded-full" />
        <Skeleton className="size-9 shrink-0 rounded-full" />
      </div>
    </div>
  );
}

demo.tsx
import { Pattern } from "@/components/ui/v-skeleton-9";

export default function Default() {
  return (
    <div className="flex min-h-[540px] w-full items-center justify-center bg-background p-8 text-foreground">
      <Pattern />
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
