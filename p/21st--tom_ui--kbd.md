<!-- Keyboard Shortcuts · @tom_ui · https://21st.dev/@tom_ui/components/kbd
     license: MIT · category: kbd
     Display keyboard shortcuts with proper key symbols. -->

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
components/ui/kbd.tsx
"use client";

import { cn } from "@/lib/utils";
import { useEffect, useState } from "react";
import { useHotkeys } from "react-hotkeys-hook";

type KeyItem = string | { display: string; key: string };

interface KbdProps {
  keys: KeyItem[];
  className?: string;
  active?: boolean;
  listenToKeyboard?: boolean;
}

const keySymbolMap = {
  command: "⌘",
  cmd: "⌘",
  control: "⌃",
  ctrl: "⌃",
  alt: "⌥",
  option: "⌥",
  space: "␣",
  arrowleft: "←",
  left: "←",
  arrowdown: "↓",
  down: "↓",
  arrowup: "↑",
  up: "↑",
  arrowright: "→",
  right: "→",
} as const;

const keyHotkeyMap: Record<string, string> = {
  command: "meta",
  cmd: "meta",
  control: "ctrl",
  ctrl: "ctrl",
  alt: "alt",
  option: "alt",
  shift: "shift",
  enter: "enter",
  return: "enter",
  space: "space",
  arrowleft: "left",
  left: "left",
  arrowdown: "down",
  down: "down",
  arrowup: "up",
  up: "up",
  arrowright: "right",
  right: "right",
};

export function Kbd({ keys = [], className, active, listenToKeyboard = false }: KbdProps) {
  const [isPressed, setIsPressed] = useState(false);

  const getKeyDisplay = (item: KeyItem): string => {
    const key = typeof item === "string" ? item : item.display;
    const lowerKey = key.toLowerCase();
    return keySymbolMap[lowerKey as keyof typeof keySymbolMap] || key.toUpperCase();
  };

  const getHotkeyString = (): string => {
    return keys
      .map((item) => {
        const key = typeof item === "string" ? item : item.key;
        const lowerKey = key.toLowerCase();
        return keyHotkeyMap[lowerKey] || lowerKey;
      })
      .join("+");
  };

  useHotkeys(
    getHotkeyString(),
    () => setIsPressed(true),
    {
      enabled: listenToKeyboard,
      keydown: true,
      keyup: false,
      preventDefault: false,
    },
    [keys, listenToKeyboard]
  );

  useEffect(() => {
    if (!listenToKeyboard) return;

    const handleKeyUp = () => {
      setIsPressed(false);
    };

    const handleBlur = () => {
      setIsPressed(false);
    };

    window.addEventListener("keyup", handleKeyUp);
    window.addEventListener("blur", handleBlur);

    return () => {
      window.removeEventListener("keyup", handleKeyUp);
      window.removeEventListener("blur", handleBlur);
    };
  }, [listenToKeyboard]);

  const isActive = active || isPressed;

  return (
    <kbd
      className={cn(
        "box-border align-text-top whitespace-nowrap select-none cursor-default tracking-tight rounded-[0.35em] min-w-[1.75em] shrink-0 justify-center items-center pb-[0.05em] px-[0.5em] text-[0.75em] font-normal leading-[1.7em] inline-flex relative -top-[0.03em] transition-all duration-100",
        isActive
          ? "bg-background text-foreground translate-y-[0.05em] shadow-[inset_0_0.05em_rgba(255,255,255,0.95),inset_0_0.05em_0.2em_rgba(0,0,0,0.1),0_0_0_0.05em_rgba(0,0,0,0.134)] dark:shadow-[inset_0_0.05em_0.2em_rgba(0,0,0,0.3),0_0_0_0.05em_rgba(255,255,255,0.134)]"
          : "bg-background text-foreground shadow-[inset_0_-0.05em_0.5em_rgba(0,0,0,0.034),inset_0_0.05em_rgba(255,255,255,0.95),inset_0_0.25em_0.5em_rgba(0,0,0,0.034),inset_0_-0.05em_rgba(0,0,0,0.172),0_0_0_0.05em_rgba(0,0,0,0.134),0_0.08em_0.17em_rgba(0,0,0,0.231)] dark:shadow-[inset_0_-0.05em_0.5em_rgba(255,255,255,0.034),inset_0_0.05em_rgba(255,255,255,0.1),inset_0_0.25em_0.5em_rgba(255,255,255,0.034),inset_0_-0.05em_rgba(255,255,255,0.172),0_0_0_0.05em_rgba(255,255,255,0.134),0_0.08em_0.17em_rgba(255,255,255,0.231)]",
        className
      )}
    >
      {keys.map((item, index) => (
        <span key={index} className={index > 0 ? "ml-0.5" : ""}>
          {getKeyDisplay(item)}
        </span>
      ))}
    </kbd>
  );
}

demo.tsx
"use client";

import { Kbd } from "@/components/ui/kbd";

export function KeySymbolsDemo() {
  return (
    <div className="flex min-h-[400px] flex-col items-center justify-center gap-6 p-8">
      <p className="text-sm text-muted-foreground">Focus the preview, then press each displayed key</p>
      <div className="flex items-center justify-center gap-3 text-2xl">
        <Kbd keys={["cmd"]} listenToKeyboard />
        <Kbd keys={["ctrl"]} listenToKeyboard />
        <Kbd keys={["alt"]} listenToKeyboard />
        <Kbd keys={["up"]} listenToKeyboard />
        <Kbd keys={["down"]} listenToKeyboard />
        <Kbd keys={["left"]} listenToKeyboard />
        <Kbd keys={["right"]} listenToKeyboard />
      </div>
    </div>
  );
}

export default { KeySymbolsDemo };
```

Install NPM dependencies:
```bash
npm install react-hotkeys-hook
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
