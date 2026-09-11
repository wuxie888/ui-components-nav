<!-- Expandable Dialog · @patrick-xin · https://21st.dev/@patrick-xin/components/expandable-dialog
     license: MIT · category: modal
     Responsive modal dialog with an expand/collapse toggle that smoothly animates between a compact and an enlarged size, with a scrollable body. -->

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
components/expandable-dialog.tsx
"use client";

import { Maximize2Icon, Minimize2Icon, X } from "lucide-react";
import { AnimatePresence, motion } from "motion/react";
import * as React from "react";
import { Button } from "@/registry/ui/button";
import {
  Dialog,
  DialogBackdrop,
  DialogClose,
  DialogHeader,
  DialogPopup,
  DialogPortal,
  DialogTitle,
  DialogTrigger,
  DialogViewport,
} from "@/registry/ui/dialog";
import { ScrollArea } from "@/registry/ui/scroll-area";

export function ExpandableDialog() {
  const [open, setOpen] = React.useState(false);
  const [isExpanded, setIsExpanded] = React.useState(false);

  return (
    <Dialog onOpenChange={setOpen} open={open}>
      <DialogTrigger render={<Button variant="glow">Open Dialog</Button>} />
      <AnimatePresence>
        {open && (
          <DialogPortal keepMounted>
            <DialogBackdrop />
            <DialogViewport className="fixed inset-0 flex items-center justify-center p-4 sm:p-6">
              <DialogPopup
                render={
                  <motion.div
                    animate={{
                      height: isExpanded ? "75vh" : "50vh",
                      maxWidth: isExpanded ? "56rem" : "40rem",
                      opacity: 1,
                      scale: 1,
                    }}
                    className="relative flex flex-col overflow-hidden rounded-md border bg-background shadow-lg"
                    exit={{ opacity: 0, scale: 0.95 }}
                    initial={{
                      height: "50vh",
                      maxWidth: "40rem",
                      opacity: 0,
                      scale: 0.95,
                    }}
                    layout
                    style={{
                      maxHeight: "90dvh",
                      width: "95vw",
                    }}
                    transition={{
                      bounce: 0,
                      damping: 30,
                      duration: 0.3,
                      stiffness: 300,
                      type: "spring",
                    }}
                  >
                    <DialogHeader className="flex flex-row items-center justify-between border-b p-2 sm:p-4">
                      <DialogTitle>Expandable Dialog</DialogTitle>
                      <div className="flex items-center gap-2">
                        <Button
                          onClick={() => setIsExpanded(!isExpanded)}
                          size="icon-sm"
                          title={isExpanded ? "Collapse" : "Expand"}
                          variant="ghost"
                        >
                          {isExpanded ? (
                            <Minimize2Icon className="size-4" />
                          ) : (
                            <Maximize2Icon className="size-4" />
                          )}
                          <span className="sr-only">Toggle expand</span>
                        </Button>
                        <DialogClose
                          render={
                            <Button size="icon-sm" variant="ghost">
                              <X className="size-4" />
                              <span className="sr-only">Close dialog</span>
                            </Button>
                          }
                          title="Close"
                        />
                      </div>
                    </DialogHeader>
                    <ScrollArea className="pr-1 min-h-0" gradientScrollFade>
                      <div className="space-y-4 p-2 sm:p-4">
                        {Array.from({ length: 30 }).map((_, i) => (
                          <div
                            className="flex items-center gap-4 rounded-md border border-border/50 h-26 bg-card p-3"
                            key={i}
                          >
                            <div className="h-10 w-10 shrink-0 rounded-full bg-muted" />
                            <div className="space-y-4">
                              <div className="h-6 w-24 sm:w-48 bg-muted rounded-md" />
                              <div className="h-6 w-32 sm:w-72 bg-muted rounded-md" />
                            </div>
                          </div>
                        ))}
                      </div>
                    </ScrollArea>
                  </motion.div>
                }
              />
            </DialogViewport>
          </DialogPortal>
        )}
      </AnimatePresence>
    </Dialog>
  );
}

demo.tsx
import { ExpandableDialog } from "@/components/ui/expandable-dialog";

export default function ExpandableDialogDemo() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center p-6">
      <ExpandableDialog />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button dialog scroll-area
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
