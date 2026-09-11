<!-- Bookmark Toggle List · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-toggle-11
     license: MIT · category: toggle
     A list of article rows where each row has a bookmark toggle button, with a live count of how many items are saved. -->

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
components/ui/v-toggle-11.tsx
"use client";

import { BookmarkIcon } from "lucide-react";
import { useState } from "react";
import { Toggle } from "@/registry/default/ui/toggle";

const articles = [
  {
    author: "Olivia Martin",
    id: "1",
    title: "Building scalable design systems with Base UI",
  },
  {
    author: "Jackson Lee",
    id: "2",
    title: "Tailwind CSS v4 migration guide",
  },
  {
    author: "Isabella Nguyen",
    id: "3",
    title: "Accessible React components from scratch",
  },
];

export function Pattern() {
  const [saved, setSaved] = useState<Set<string>>(new Set());

  const toggle = (id: string) =>
    setSaved((prev) => {
      const next = new Set(prev);
      next.has(id) ? next.delete(id) : next.add(id);
      return next;
    });

  return (
    <div className="w-full max-w-sm space-y-2">
      {articles.map((a) => {
        const isBookmarked = saved.has(a.id);
        return (
          <div
            className="flex items-center justify-between gap-2 rounded-lg border border-border px-3 py-2.5"
            key={a.id}
          >
            <div className="flex min-w-0 flex-col gap-0.5">
              <span className="truncate font-medium text-sm">{a.title}</span>
              <span className="text-muted-foreground text-xs">{a.author}</span>
            </div>
            <Toggle
              aria-label={isBookmarked ? "Remove bookmark" : "Bookmark article"}
              className="shrink-0"
              onPressedChange={() => toggle(a.id)}
              pressed={isBookmarked}
              size="sm"
              variant="outline"
            >
              <BookmarkIcon
                className={`size-4 transition-colors ${isBookmarked ? "fill-current" : ""}`}
              />
            </Toggle>
          </div>
        );
      })}
      <p className="text-center text-muted-foreground text-xs">
        {saved.size} of {articles.length} saved
      </p>
    </div>
  );
}

demo.tsx
"use client";

import { BookmarkIcon } from "lucide-react";
import { useState } from "react";
import { Toggle } from "@/components/ui/v-toggle-11-utils/toggle";

const articles = [
  {
    author: "Olivia Martin",
    id: "1",
    title: "Building scalable design systems with Base UI",
  },
  {
    author: "Jackson Lee",
    id: "2",
    title: "Tailwind CSS v4 migration guide",
  },
  {
    author: "Isabella Nguyen",
    id: "3",
    title: "Accessible React components from scratch",
  },
];

function Pattern() {
  const [saved, setSaved] = useState<Set<string>>(new Set(["1", "3"]));

  const toggle = (id: string) =>
    setSaved((prev) => {
      const next = new Set(prev);
      next.has(id) ? next.delete(id) : next.add(id);
      return next;
    });

  return (
    <div className="w-full max-w-sm space-y-2">
      {articles.map((a) => {
        const isBookmarked = saved.has(a.id);
        return (
          <div
            className="flex items-center justify-between gap-2 rounded-lg border border-border px-3 py-2.5"
            key={a.id}
          >
            <div className="flex min-w-0 flex-col gap-0.5">
              <span className="truncate font-medium text-sm">{a.title}</span>
              <span className="text-muted-foreground text-xs">{a.author}</span>
            </div>
            <Toggle
              aria-label={isBookmarked ? "Remove bookmark" : "Bookmark article"}
              className="shrink-0"
              onPressedChange={() => toggle(a.id)}
              pressed={isBookmarked}
              size="sm"
              variant="outline"
            >
              <BookmarkIcon
                className={`size-4 transition-colors ${isBookmarked ? "fill-current" : ""}`}
              />
            </Toggle>
          </div>
        );
      })}
      <p className="text-center text-muted-foreground text-xs">
        {saved.size} of {articles.length} saved
      </p>
    </div>
  );
}

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center bg-background p-6 text-foreground">
      <Pattern />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add toggle
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
