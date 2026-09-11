<!-- Autosave Input Group · @shadcnspace · https://21st.dev/@shadcnspace/components/input-group-01
     license: MIT · category: form
     A text input with an inline autosave status indicator that shows a spinner while typing and a checkmark once changes are saved. -->

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
components/shadcn-space/input-group/input-group-01.tsx
"use client";

import { useState, useEffect, useRef } from "react";
import {
  InputGroup,
  InputGroupAddon,
  InputGroupInput,
} from "@/components/ui/input-group";
import { Loader2, CheckCircle2 } from "lucide-react";
import { motion, AnimatePresence } from "motion/react";

const InputGroupDemo = () => {
  const [value, setValue] = useState("");
  const [status, setStatus] = useState<"idle" | "saving" | "saved">("idle");
  const typingTimeoutRef = useRef<NodeJS.Timeout | null>(null);
  const idleTimeoutRef = useRef<NodeJS.Timeout | null>(null);

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setValue(e.target.value);
    setStatus("saving");
    if (typingTimeoutRef.current) clearTimeout(typingTimeoutRef.current);
    if (idleTimeoutRef.current) clearTimeout(idleTimeoutRef.current);
    typingTimeoutRef.current = setTimeout(() => {
      setStatus("saved");
      idleTimeoutRef.current = setTimeout(() => {
        setStatus("idle");
      }, 2000);
    }, 1000);
  };

  // Cleanup timeouts on unmount
  useEffect(() => {
    return () => {
      if (typingTimeoutRef.current) clearTimeout(typingTimeoutRef.current);
      if (idleTimeoutRef.current) clearTimeout(idleTimeoutRef.current);
    };
  }, []);

  return (
    <div className="flex flex-col gap-2">
      <InputGroup className="h-10 rounded-xl px-1">
        <InputGroupInput
          placeholder="Type message to save..."
          value={value}
          onChange={handleChange}
        />
        <InputGroupAddon align="inline-end" className="pr-3">
          <AnimatePresence mode="wait">
            {status === "saving" && (
              <motion.div
                key="saving"
                initial={{ opacity: 0, scale: 0.8, y: 4 }}
                animate={{ opacity: 1, scale: 1, y: 0 }}
                exit={{ opacity: 0, scale: 0.8, y: -4 }}
                transition={{ duration: 0.15 }}
                className="flex items-center gap-1.5 text-xs text-muted-foreground font-medium"
              >
                <Loader2 className="size-3.5 animate-spin text-primary" />
                <span>Saving</span>
              </motion.div>
            )}
            {status === "saved" && (
              <motion.div
                key="saved"
                initial={{ opacity: 0, scale: 0.8, y: 4 }}
                animate={{ opacity: 1, scale: 1, y: 0 }}
                exit={{ opacity: 0, scale: 0.8, y: -4 }}
                transition={{ duration: 0.15 }}
                className="flex items-center gap-1.5 text-xs text-teal-400 font-medium"
              >
                <CheckCircle2 className="size-3.5" />
                <span>Saved</span>
              </motion.div>
            )}
          </AnimatePresence>
        </InputGroupAddon>
      </InputGroup>
      <p className="text-xs text-muted-foreground px-1">
        Typing triggers{" "}
        <span className="font-semibold text-foreground">saving</span> state.
        Stopping for 1s shows{" "}
        <span className="font-semibold text-teal-400">
          saved
        </span>
        .
      </p>
    </div>
  );
};

export default InputGroupDemo;

demo.tsx
import InputGroupDemo from "@/components/ui/input-group-01";

export default function Demo() {
  return (
    <div className="flex w-full max-w-sm items-center justify-center">
      <InputGroupDemo />
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
npx shadcn@latest add input-group
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
