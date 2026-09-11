<!-- Keyboard Shortcuts Sheet · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-sheet-14
     license: no-license · category: dialog
     A slide-out sheet panel that lists categorized keyboard shortcuts, each rendered with monospace key badges and section separators. -->

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
components/ui/v-sheet-14.tsx
import { KeyboardIcon } from "lucide-react";
import { Badge } from "@/registry/default/ui/badge";
import { Button } from "@/registry/default/ui/button";
import { Separator } from "@/registry/default/ui/separator";
import {
  Sheet,
  SheetClose,
  SheetDescription,
  SheetFooter,
  SheetHeader,
  SheetPanel,
  SheetPopup,
  SheetTitle,
  SheetTrigger,
} from "@/registry/default/ui/sheet";

const shortcuts = [
  {
    items: [
      { keys: ["⌘", "K"], label: "Open command palette" },
      { keys: ["⌘", "P"], label: "Quick file open" },
      { keys: ["⌘", "Shift", "F"], label: "Search in project" },
      { keys: ["⌘", "/"], label: "Toggle comment" },
    ],
    section: "General",
  },
  {
    items: [
      { keys: ["⌘", "S"], label: "Save file" },
      { keys: ["⌘", "Z"], label: "Undo" },
      { keys: ["⌘", "Shift", "Z"], label: "Redo" },
      { keys: ["⌘", "D"], label: "Duplicate line" },
    ],
    section: "Editor",
  },
  {
    items: [
      { keys: ["⌘", "B"], label: "Toggle sidebar" },
      { keys: ["⌘", "J"], label: "Toggle panel" },
      { keys: ["⌘", "`"], label: "Toggle terminal" },
    ],
    section: "View",
  },
];

export default function Particle() {
  return (
    <Sheet>
      <SheetTrigger render={<Button variant="outline" />}>
        <KeyboardIcon aria-hidden="true" />
        Shortcuts
      </SheetTrigger>
      <SheetPopup>
        <SheetHeader>
          <SheetTitle>Keyboard Shortcuts</SheetTitle>
          <SheetDescription>
            A quick reference for available keyboard shortcuts.
          </SheetDescription>
        </SheetHeader>
        <SheetPanel className="space-y-4">
          {shortcuts.map(({ items, section }, i) => (
            <div key={section}>
              {i > 0 && <Separator className="mb-4" />}
              <p className="mb-2 font-medium text-muted-foreground text-xs uppercase tracking-wide">
                {section}
              </p>
              <div className="space-y-1">
                {items.map(({ keys, label }) => (
                  <div
                    className="flex items-center justify-between py-1"
                    key={label}
                  >
                    <span className="text-sm">{label}</span>
                    <div className="flex items-center gap-1">
                      {keys.map((k) => (
                        <Badge
                          className="h-5 rounded px-1.5 font-mono text-[10px]"
                          key={k}
                          variant="outline"
                        >
                          {k}
                        </Badge>
                      ))}
                    </div>
                  </div>
                ))}
              </div>
            </div>
          ))}
        </SheetPanel>
        <SheetFooter>
          <SheetClose render={<Button className="w-full" variant="outline" />}>
            Close
          </SheetClose>
        </SheetFooter>
      </SheetPopup>
    </Sheet>
  );
}

demo.tsx
import Component from "@/components/ui/v-sheet-14";

export default function Default() {
  return (
    <div className="flex min-h-[350px] w-full items-center justify-center p-4">
      <Component />
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
npx shadcn@latest add badge button scroll-area separator sheet
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
