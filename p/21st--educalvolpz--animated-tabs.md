<!-- Animated Tabs · @educalvolpz · https://21st.dev/@educalvolpz/components/animated-tabs
     license: MIT · category: navigation-menu
     Animated tabs component with a smooth sliding indicator, offering underline, pill, and segment variants with icon support and full keyboard navigation. -->

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
components/ui/index.tsx
"use client";

import { cn } from "@/lib/utils";
import { motion, useReducedMotion } from "motion/react";
import { type ReactNode, useCallback, useId, useState } from "react";

export interface AnimatedTabsProps {
  activeTab?: string;
  className?: string;
  defaultTab?: string;
  layoutId?: string;
  onChange?: (tabId: string) => void;
  tabs: { id: string; label: string; icon?: ReactNode }[];
  variant?: "underline" | "pill" | "segment";
}

const SPRING = {
  bounce: 0.05,
  duration: 0.25,
  type: "spring" as const,
};

export default function AnimatedTabs({
  tabs,
  activeTab: controlledActiveTab,
  defaultTab,
  onChange,
  variant = "underline",
  layoutId: customLayoutId,
  className,
}: AnimatedTabsProps) {
  const shouldReduceMotion = useReducedMotion();
  const generatedId = useId();
  const layoutId = customLayoutId ?? `animated-tabs-${generatedId}`;

  const [internalActiveTab, setInternalActiveTab] = useState(
    defaultTab ?? tabs[0]?.id ?? ""
  );

  const isControlled = controlledActiveTab !== undefined;
  const activeTab = isControlled ? controlledActiveTab : internalActiveTab;

  const handleTabChange = useCallback(
    (tabId: string) => {
      if (!isControlled) {
        setInternalActiveTab(tabId);
      }
      onChange?.(tabId);
    },
    [isControlled, onChange]
  );

  const handleKeyDown = useCallback(
    (event: React.KeyboardEvent, currentIndex: number) => {
      let newIndex = currentIndex;

      if (event.key === "ArrowRight") {
        event.preventDefault();
        newIndex = (currentIndex + 1) % tabs.length;
      } else if (event.key === "ArrowLeft") {
        event.preventDefault();
        newIndex = (currentIndex - 1 + tabs.length) % tabs.length;
      } else if (event.key === "Home") {
        event.preventDefault();
        newIndex = 0;
      } else if (event.key === "End") {
        event.preventDefault();
        newIndex = tabs.length - 1;
      } else {
        return;
      }

      const newTab = tabs[newIndex];
      if (newTab) {
        handleTabChange(newTab.id);
        const tabElement = document.getElementById(
          `${layoutId}-tab-${newTab.id}`
        );
        tabElement?.focus();
      }
    },
    [tabs, handleTabChange, layoutId]
  );

  const baseContainerStyles = cn(
    "relative inline-flex",
    variant === "underline" && "gap-1 border-border border-b",
    variant === "pill" && "gap-1 rounded-full bg-muted p-1",
    variant === "segment" && "gap-0 rounded-lg bg-muted p-1"
  );

  const getTabStyles = (isActive: boolean) =>
    cn(
      "relative z-10 flex cursor-pointer items-center justify-center gap-2 px-4 py-2 font-medium text-sm transition-colors",
      "focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2",
      variant === "underline" && [
        "rounded-t-md",
        isActive
          ? "text-foreground"
          : "text-muted-foreground hover:text-foreground",
      ],
      variant === "pill" && [
        "rounded-full",
        isActive
          ? "text-foreground"
          : "text-muted-foreground hover:text-foreground",
      ],
      variant === "segment" && [
        "flex-1 rounded-md",
        isActive
          ? "text-foreground"
          : "text-muted-foreground hover:text-foreground",
      ]
    );

  const getIndicatorStyles = () =>
    cn(
      "absolute",
      variant === "underline" && "right-0 -bottom-px left-0 h-0.5 bg-brand",
      variant === "pill" &&
        "inset-0 rounded-full border border-border bg-background shadow-sm",
      variant === "segment" &&
        "inset-0 rounded-md border border-border bg-background shadow-sm"
    );

  return (
    <div
      aria-label="Tabs"
      className={cn(baseContainerStyles, className)}
      role="tablist"
    >
      {tabs.map((tab, index) => {
        const isActive = activeTab === tab.id;

        return (
          <button
            aria-selected={isActive}
            className={getTabStyles(isActive)}
            id={`${layoutId}-tab-${tab.id}`}
            key={tab.id}
            onClick={() => handleTabChange(tab.id)}
            onKeyDown={(e) => handleKeyDown(e, index)}
            role="tab"
            tabIndex={isActive ? 0 : -1}
            type="button"
          >
            {isActive && (
              <motion.span
                className={getIndicatorStyles()}
                layout
                layoutId={layoutId}
                style={{ originY: "0px" }}
                transition={shouldReduceMotion ? { duration: 0 } : SPRING}
              />
            )}
            {tab.icon ? (
              <span className="relative z-10">{tab.icon}</span>
            ) : null}
            <span className="relative z-10">{tab.label}</span>
          </button>
        );
      })}
    </div>
  );
}

demo.tsx
"use client";

import AnimatedTabs from "@/components/ui/animated-tabs";
import { useState } from "react";

const tabs = [
  { id: "home", label: "Home" },
  { id: "profile", label: "Profile" },
  { id: "settings", label: "Settings" },
];

export default function AnimatedTabsDemo() {
  const [activeTab, setActiveTab] = useState("home");

  return (
    <div className="flex flex-col items-center gap-8">
      <AnimatedTabs
        activeTab={activeTab}
        layoutId="underline-demo"
        onChange={setActiveTab}
        tabs={tabs}
        variant="underline"
      />

      <AnimatedTabs
        activeTab={activeTab}
        layoutId="pill-demo"
        onChange={setActiveTab}
        tabs={tabs}
        variant="pill"
      />

      <AnimatedTabs
        activeTab={activeTab}
        layoutId="segment-demo"
        onChange={setActiveTab}
        tabs={tabs}
        variant="segment"
      />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add tokens.json
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
