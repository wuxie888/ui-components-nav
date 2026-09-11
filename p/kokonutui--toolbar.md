<!-- toolbar · Kokonut UI · https://kokonutui.com/docs/navigation/toolbar
     license: MIT · category: navigation-menu
      -->

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
components/kokonutui/toolbar.tsx
"use client";

/**
 * @author: @dorianbaffier
 * @description: Toolbar
 * @version: 1.0.0
 * @date: 2025-06-26
 * @license: MIT
 * @website: https://kokonutui.com
 * @github: https://github.com/kokonut-labs/kokonutui
 */

import {
  Bell,
  CircleUserRound,
  Edit2,
  FileDown,
  Frame,
  Layers,
  Lock,
  type LucideIcon,
  MousePointer2,
  Move,
  Palette,
  Shapes,
  Share2,
  SlidersHorizontal,
} from "lucide-react";
import { AnimatePresence, motion } from "motion/react";
import * as React from "react";
import { cn } from "@/lib/utils";

interface ToolbarItem {
  id: string;
  title: string;
  icon: LucideIcon;
  type?: never;
}

interface ToolbarProps {
  items?: ToolbarItem[];
  defaultSelected?: string;
  className?: string;
  activeColor?: string;
  onSelect?: (itemId: string) => void;
}

const DEFAULT_TOOLBAR_ITEMS: ToolbarItem[] = [
  { id: "select", title: "Select", icon: MousePointer2 },
  { id: "move", title: "Move", icon: Move },
  { id: "shapes", title: "Shapes", icon: Shapes },
  { id: "layers", title: "Layers", icon: Layers },
  { id: "frame", title: "Frame", icon: Frame },
  { id: "properties", title: "Properties", icon: SlidersHorizontal },
  { id: "export", title: "Export", icon: FileDown },
  { id: "share", title: "Share", icon: Share2 },
  { id: "notifications", title: "Notifications", icon: Bell },
  { id: "profile", title: "Profile", icon: CircleUserRound },
  { id: "appearance", title: "Appearance", icon: Palette },
];

const buttonVariants = {
  initial: {
    gap: 0,
    paddingLeft: ".5rem",
    paddingRight: ".5rem",
  },
  animate: (isSelected: boolean) => ({
    gap: isSelected ? ".5rem" : 0,
    paddingLeft: isSelected ? "1rem" : ".5rem",
    paddingRight: isSelected ? "1rem" : ".5rem",
  }),
};

const spanVariants = {
  initial: { width: 0, opacity: 0 },
  animate: { width: "auto", opacity: 1 },
  exit: { width: 0, opacity: 0 },
};

const notificationVariants = {
  initial: { opacity: 0, y: 10 },
  animate: { opacity: 1, y: -10 },
  exit: { opacity: 0, y: -20 },
};

const lineVariants = {
  initial: { scaleX: 0, x: "-50%" },
  animate: {
    scaleX: 1,
    x: "0%",
    transition: { duration: 0.2, ease: "easeOut" },
  },
  exit: {
    scaleX: 0,
    x: "50%",
    transition: { duration: 0.2, ease: "easeIn" },
  },
};

const transition = { type: "spring", bounce: 0, duration: 0.4 };

export function Toolbar({
  items = DEFAULT_TOOLBAR_ITEMS,
  defaultSelected = "select",
  className,
  activeColor = "text-primary",
  onSelect,
}: ToolbarProps) {
  const [selected, setSelected] = React.useState<string | null>(
    defaultSelected
  );
  const [isToggled, setIsToggled] = React.useState(false);
  const [activeNotification, setActiveNotification] = React.useState<
    string | null
  >(null);
  const outsideClickRef = React.useRef(null);

  const handleItemClick = (itemId: string) => {
    setSelected(selected === itemId ? null : itemId);
    onSelect?.(itemId);
    setActiveNotification(itemId);
    setTimeout(() => setActiveNotification(null), 1500);
  };

  return (
    <div className="space-y-2">
      <div
        className={cn(
          "relative flex items-center gap-3 p-2",
          "bg-background",
          "rounded-xl border",
          "transition-all duration-200",
          className
        )}
        ref={outsideClickRef}
      >
        <AnimatePresence>
          {activeNotification && (
            <motion.div
              animate="animate"
              className="absolute -top-8 left-1/2 z-50 -translate-x-1/2 transform"
              exit="exit"
              initial="initial"
              transition={{ duration: 0.3 }}
              variants={notificationVariants as any}
            >
              <div className="rounded-full bg-primary px-3 py-1 text-primary-foreground text-xs">
                {items.find((item) => item.id === activeNotification)?.title}{" "}
                clicked!
              </div>
              <motion.div
                animate="animate"
                className="absolute -bottom-1 left-1/2 h-[2px] w-full origin-left bg-primary"
                exit="exit"
                initial="initial"
                variants={lineVariants as any}
              />
            </motion.div>
          )}
        </AnimatePresence>

        <div className="flex items-center gap-2">
          {items.map((item) => (
            <motion.button
              animate="animate"
              className={cn(
                "relative flex items-center rounded-none px-3 py-2",
                "font-medium text-sm transition-colors duration-300",
                selected === item.id
                  ? "rounded-lg bg-[#1F9CFE] text-white"
                  : "text-muted-foreground hover:bg-muted hover:text-foreground"
              )}
              custom={selected === item.id}
              initial={false}
              key={item.id}
              onClick={() => handleItemClick(item.id)}
              transition={transition as any}
              variants={buttonVariants as any}
            >
              <item.icon
                className={cn(selected === item.id && "text-white")}
                size={16}
              />
              <AnimatePresence initial={false}>
                {selected === item.id && (
                  <motion.span
                    animate="animate"
                    className="overflow-hidden"
                    exit="exit"
                    initial="initial"
                    transition={transition as any}
                    variants={spanVariants as any}
                  >
                    {item.title}
                  </motion.span>
                )}
              </AnimatePresence>
            </motion.button>
          ))}

          <motion.button
            className={cn(
              "flex items-center gap-2 px-4 py-2",
              "rounded-xl border shadow-sm transition-all duration-200",
              "hover:shadow-md active:border-primary/50",
              isToggled
                ? [
                    "bg-[#1F9CFE] text-white",
                    "border-[#1F9CFE]/30",
                    "hover:bg-[#1F9CFE]/90",
                    "hover:border-[#1F9CFE]/40",
                  ]
                : [
                    "bg-background text-muted-foreground",
                    "border-border/30",
                    "hover:bg-muted",
                    "hover:text-foreground",
                    "hover:border-border/40",
                  ]
            )}
            onClick={() => setIsToggled(!isToggled)}
            whileHover={{ scale: 1.02 }}
            whileTap={{ scale: 0.98 }}
          >
            {isToggled ? (
              <Edit2 className="h-3.5 w-3.5" />
            ) : (
              <Lock className="h-3.5 w-3.5" />
            )}
            <span className="font-medium text-sm">
              {isToggled ? "On" : "Off"}
            </span>
          </motion.button>
        </div>
      </div>
    </div>
  );
}

export default Toolbar;
```

Install NPM dependencies:
```bash
npm install lucide-react motion
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
