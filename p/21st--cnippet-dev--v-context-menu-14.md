<!-- Context Menu Social Feed · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-context-menu-14
     license: MIT · category: menu
     Social feed cards with a right-click context menu to upvote, downvote, reply, and report posts with a nested report submenu. -->

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
components/ui/v-context-menu-14.tsx
"use client";

import {
  ArrowUpIcon,
  FlagIcon,
  MessageSquareIcon,
  ThumbsDownIcon,
  ThumbsUpIcon,
  UserXIcon,
} from "lucide-react";
import { useState } from "react";
import {
  ContextMenu,
  ContextMenuContent,
  ContextMenuGroup,
  ContextMenuItem,
  ContextMenuSeparator,
  ContextMenuSub,
  ContextMenuSubContent,
  ContextMenuSubTrigger,
  ContextMenuTrigger,
} from "@/registry/default/ui/context-menu";

const POSTS = [
  {
    author: "Luna V.",
    body: "Just shipped a redesign of our onboarding flow. Cut drop-off by 34% in the first week. The key was reducing the number of steps from 7 to 3.",
    id: "p1",
    time: "2h",
  },
  {
    author: "Rafi A.",
    body: "Hot take: dark mode is not an accessibility feature — it's a preference. True accessibility is about contrast ratios and text size, not color scheme.",
    id: "p2",
    time: "5h",
  },
];

export function Pattern() {
  const [votes, setVotes] = useState<Record<string, number>>({
    p1: 24,
    p2: 11,
  });

  return (
    <div className="w-full max-w-sm space-y-3">
      {POSTS.map((post) => (
        <ContextMenu key={post.id}>
          <ContextMenuTrigger className="block w-full cursor-default rounded-xl border px-4 py-3 text-left">
            <div className="mb-2 flex items-center justify-between">
              <span className="font-semibold text-sm">{post.author}</span>
              <span className="text-muted-foreground text-xs">
                {post.time} ago
              </span>
            </div>
            <p className="text-muted-foreground text-sm leading-relaxed">
              {post.body}
            </p>
            <div className="mt-3 flex items-center gap-3 text-muted-foreground text-xs">
              <span className="flex items-center gap-1">
                <ArrowUpIcon className="size-3.5" />
                {votes[post.id]}
              </span>
              <span className="flex items-center gap-1">
                <MessageSquareIcon className="size-3.5" />
                Reply
              </span>
            </div>
          </ContextMenuTrigger>
          <ContextMenuContent className="w-44">
            <ContextMenuGroup>
              <ContextMenuItem
                onSelect={() =>
                  setVotes((v) => ({ ...v, [post.id]: (v[post.id] ?? 0) + 1 }))
                }
              >
                <ThumbsUpIcon />
                Upvote
              </ContextMenuItem>
              <ContextMenuItem
                onSelect={() =>
                  setVotes((v) => ({
                    ...v,
                    [post.id]: Math.max(0, (v[post.id] ?? 0) - 1),
                  }))
                }
              >
                <ThumbsDownIcon />
                Downvote
              </ContextMenuItem>
              <ContextMenuItem>
                <MessageSquareIcon />
                Reply
              </ContextMenuItem>
            </ContextMenuGroup>
            <ContextMenuSeparator />
            <ContextMenuSub>
              <ContextMenuSubTrigger>
                <FlagIcon />
                Report
              </ContextMenuSubTrigger>
              <ContextMenuSubContent>
                <ContextMenuGroup>
                  <ContextMenuItem>Spam</ContextMenuItem>
                  <ContextMenuItem>Misinformation</ContextMenuItem>
                  <ContextMenuItem>Harassment</ContextMenuItem>
                  <ContextMenuItem>Off-topic</ContextMenuItem>
                </ContextMenuGroup>
              </ContextMenuSubContent>
            </ContextMenuSub>
            <ContextMenuGroup>
              <ContextMenuItem variant="destructive">
                <UserXIcon />
                Mute author
              </ContextMenuItem>
            </ContextMenuGroup>
          </ContextMenuContent>
        </ContextMenu>
      ))}
    </div>
  );
}

demo.tsx
import { Pattern } from "@/components/ui/v-context-menu-14";

export default function Default() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center p-6">
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
npx shadcn@latest add context-menu menu
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
