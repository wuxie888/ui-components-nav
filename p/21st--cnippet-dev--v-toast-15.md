<!-- Unsaved Changes Reminder Toast · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-toast-15
     license: no-license · category: toast
     A toast that reminds users of unsaved edits with inline save and discard actions. -->

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
components/ui/v-toast-15.tsx
"use client";

import { useState } from "react";
import { Button } from "@/registry/default/ui/button";
import { toastManager } from "@/registry/default/ui/toast";

type SaveState = "clean" | "dirty" | "saving";

export function Pattern() {
  const [saveState, setSaveState] = useState<SaveState>("clean");
  const [content, setContent] = useState(
    "Project Alpha\n\nThis is the project description. Click below to make changes and see the unsaved changes toast appear.",
  );
  const toastIdRef = { current: null as string | null };

  const makeChange = () => {
    if (saveState === "dirty") return;
    setContent((c) => `${c} Updated.`);
    setSaveState("dirty");

    const id = toastManager.add({
      actionProps: {
        children: "Save now",
        onClick: () => save(id),
      },
      description: "Your recent edits haven't been saved yet.",
      timeout: 0,
      title: "Unsaved changes",
      type: "warning",
    });
    toastIdRef.current = id;
  };

  const save = (id?: string) => {
    const toastId = id ?? toastIdRef.current;
    if (toastId) toastManager.close(toastId);
    setSaveState("saving");
    setTimeout(() => {
      setSaveState("clean");
      toastManager.add({
        description: "All changes have been saved to the server.",
        title: "Document saved",
        type: "success",
      });
    }, 900);
  };

  return (
    <div className="flex w-full max-w-xs flex-col gap-3">
      <div className="min-h-24 rounded-lg border border-input bg-background p-3 text-foreground/80 text-sm leading-relaxed">
        {content}
      </div>
      <div className="flex gap-2">
        <Button
          className="flex-1"
          disabled={saveState !== "clean"}
          onClick={makeChange}
          variant="outline"
        >
          Edit document
        </Button>
        <Button
          className="flex-1"
          disabled={saveState !== "dirty"}
          loading={saveState === "saving"}
          onClick={() => save()}
        >
          Save
        </Button>
      </div>
    </div>
  );
}

demo.tsx
"use client";

import Pattern from "@/components/ui/v-toast-15";
import { ToastProvider } from "@/components/ui/toast";

export default function Default() {
  return (
    <ToastProvider>
      <div className="flex min-h-72 w-full items-center justify-center p-8">
        <Pattern />
      </div>
    </ToastProvider>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button button?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068 toast toast?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068
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
