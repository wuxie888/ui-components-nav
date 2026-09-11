<!-- Quick Actions Grid Drawer · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-drawer-15
     license: MIT · category: grid
     A bottom drawer that reveals a 4-column grid of quick action buttons (edit, share, download, delete and more) with a destructive variant and a cancel footer. -->

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
components/ui/v-drawer-15.tsx
"use client";

import {
  ArchiveIcon,
  BookmarkIcon,
  DownloadIcon,
  FlagIcon,
  MessageSquareIcon,
  MoreHorizontalIcon,
  PencilIcon,
  Share2Icon,
  TrashIcon,
} from "lucide-react";
import { useState } from "react";
import { Button } from "@/registry/default/ui/button";
import {
  Drawer,
  DrawerClose,
  DrawerFooter,
  DrawerHeader,
  DrawerPanel,
  DrawerPopup,
  DrawerTitle,
  DrawerTrigger,
} from "@/registry/default/ui/drawer";

const ACTIONS = [
  { description: "Modify this item", icon: PencilIcon, label: "Edit" },
  { description: "Add a comment", icon: MessageSquareIcon, label: "Comment" },
  { description: "Share with others", icon: Share2Icon, label: "Share" },
  { description: "Save for later", icon: BookmarkIcon, label: "Bookmark" },
  { description: "Export as file", icon: DownloadIcon, label: "Download" },
  { description: "Move to archive", icon: ArchiveIcon, label: "Archive" },
  { description: "Flag for review", icon: FlagIcon, label: "Report" },
  {
    description: "Remove permanently",
    destructive: true,
    icon: TrashIcon,
    label: "Delete",
  },
];

export default function Particle() {
  const [lastAction, setLastAction] = useState<string | null>(null);

  return (
    <div className="flex items-center gap-3">
      {lastAction && (
        <p className="text-muted-foreground text-sm">
          Action:{" "}
          <span className="font-medium text-foreground">{lastAction}</span>
        </p>
      )}
      <Drawer>
        <DrawerTrigger render={<Button size="icon" variant="outline" />}>
          <MoreHorizontalIcon className="size-4" />
        </DrawerTrigger>
        <DrawerPopup showBar>
          <DrawerHeader>
            <DrawerTitle>Actions</DrawerTitle>
          </DrawerHeader>
          <DrawerPanel>
            <div className="grid grid-cols-4 gap-2">
              {ACTIONS.map((action) => (
                <DrawerClose
                  key={action.label}
                  render={
                    <button
                      className={`flex flex-col items-center gap-2 rounded-xl border p-3 text-center transition-colors ${
                        action.destructive
                          ? "border-destructive/20 text-destructive hover:bg-destructive/5"
                          : "text-muted-foreground hover:border-foreground/20 hover:text-foreground"
                      }`}
                      onClick={() => setLastAction(action.label)}
                      type="button"
                    />
                  }
                >
                  <action.icon className="size-5" />
                  <span className="font-medium text-xs leading-none">
                    {action.label}
                  </span>
                </DrawerClose>
              ))}
            </div>
          </DrawerPanel>
          <DrawerFooter
            className="justify-center sm:justify-center"
            variant="bare"
          >
            <DrawerClose render={<Button variant="outline" />}>
              Cancel
            </DrawerClose>
          </DrawerFooter>
        </DrawerPopup>
      </Drawer>
    </div>
  );
}

demo.tsx
"use client";

import {
  ArchiveIcon,
  BookmarkIcon,
  DownloadIcon,
  FlagIcon,
  MessageSquareIcon,
  MoreHorizontalIcon,
  PencilIcon,
  Share2Icon,
  TrashIcon,
} from "lucide-react";
import { useState } from "react";
import { Button } from "@/components/ui/button";
import {
  Drawer,
  DrawerClose,
  DrawerFooter,
  DrawerHeader,
  DrawerPanel,
  DrawerPopup,
  DrawerTitle,
  DrawerTrigger,
} from "@/components/ui/v-drawer-15-utils/drawer";

const ACTIONS = [
  { description: "Modify this item", icon: PencilIcon, label: "Edit" },
  { description: "Add a comment", icon: MessageSquareIcon, label: "Comment" },
  { description: "Share with others", icon: Share2Icon, label: "Share" },
  { description: "Save for later", icon: BookmarkIcon, label: "Bookmark" },
  { description: "Export as file", icon: DownloadIcon, label: "Download" },
  { description: "Move to archive", icon: ArchiveIcon, label: "Archive" },
  { description: "Flag for review", icon: FlagIcon, label: "Report" },
  {
    description: "Remove permanently",
    destructive: true,
    icon: TrashIcon,
    label: "Delete",
  },
];

export default function Default() {
  const [lastAction, setLastAction] = useState<string | null>(null);

  return (
    <div className="flex min-h-[440px] w-full items-center justify-center p-6">
      <div className="flex items-center gap-3">
        {lastAction && (
          <p className="text-muted-foreground text-sm">
            Action:{" "}
            <span className="font-medium text-foreground">{lastAction}</span>
          </p>
        )}
        <Drawer defaultOpen>
          <DrawerTrigger render={<Button size="icon" variant="outline" />}>
            <MoreHorizontalIcon className="size-4" />
          </DrawerTrigger>
          <DrawerPopup showBar>
            <DrawerHeader>
              <DrawerTitle>Actions</DrawerTitle>
            </DrawerHeader>
            <DrawerPanel>
              <div className="grid grid-cols-4 gap-2">
                {ACTIONS.map((action) => (
                  <DrawerClose
                    key={action.label}
                    render={
                      <button
                        className={`flex flex-col items-center gap-2 rounded-xl border p-3 text-center transition-colors ${
                          action.destructive
                            ? "border-destructive/20 text-destructive hover:bg-destructive/5"
                            : "text-muted-foreground hover:border-foreground/20 hover:text-foreground"
                        }`}
                        onClick={() => setLastAction(action.label)}
                        type="button"
                      />
                    }
                  >
                    <action.icon className="size-5" />
                    <span className="font-medium text-xs leading-none">
                      {action.label}
                    </span>
                  </DrawerClose>
                ))}
              </div>
            </DrawerPanel>
            <DrawerFooter
              className="justify-center sm:justify-center"
              variant="bare"
            >
              <DrawerClose render={<Button variant="outline" />}>
                Cancel
              </DrawerClose>
            </DrawerFooter>
          </DrawerPopup>
        </Drawer>
      </div>
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
npx shadcn@latest add button button?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068&publisher_install_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdXJwb3NlIjoicHVibGlzaGVyLXJlZ2lzdHJ5LWluc3RhbGwiLCJpYXQiOjE3ODcxMDc0MDEsImV4cCI6MTc4NzEwODAwMX0.3cR8c8MbDP0wotJhsSviIZSE7kY8u-bPRxContBq3nU drawer scroll-area
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
