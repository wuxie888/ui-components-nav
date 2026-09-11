<!-- Floating Button · @bundui · https://21st.dev/@bundui/components/floating-button
     license: MIT · category: link
     A floating button is a common UI component used in user interfaces. It typically appears as a button "floating" over other content, often positioned in a corner of the screen. This button allows users to quickly perform a key action. It is also known as a Floating Action Button (FAB). -->

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
components/ui/floating-button.tsx
"use client";

import React, { ReactNode } from "react";
import { AnimatePresence, motion } from "motion/react";
import { useOnClickOutside } from "usehooks-ts";

type FloatingButtonProps = {
  className?: string;
  children: ReactNode;
  triggerContent: ReactNode;
};

type FloatingButtonItemProps = {
  children: ReactNode;
};

const list = {
  visible: {
    opacity: 1,
    transition: {
      staggerChildren: 0.1,
      staggerDirection: -1
    }
  },
  hidden: {
    opacity: 0,
    transition: {
      when: "afterChildren",
      staggerChildren: 0.1
    }
  }
};

const item = {
  visible: { opacity: 1, y: 0 },
  hidden: { opacity: 0, y: 5 }
};

const btn = {
  visible: { rotate: "45deg" },
  hidden: { rotate: 0 }
};

function FloatingButton({ children, triggerContent }: FloatingButtonProps) {
  const ref = React.useRef<HTMLDivElement | null>(null);
  const [isOpen, setIsOpen] = React.useState(false);

  useOnClickOutside(ref as React.RefObject<HTMLDivElement>, () => setIsOpen(false));

  return (
    <div className="relative flex flex-col items-center">
      <AnimatePresence>
        <motion.ul
          key="list"
          className="absolute bottom-14 flex flex-col items-center gap-2"
          initial="hidden"
          animate={isOpen ? "visible" : "hidden"}
          variants={list}>
          {children}
        </motion.ul>
        <motion.div
          key="button"
          variants={btn}
          animate={isOpen ? "visible" : "hidden"}
          ref={ref}
          onClick={() => setIsOpen(!isOpen)}
          className="cursor-pointer">
          {triggerContent}
        </motion.div>
      </AnimatePresence>
    </div>
  );
}

function FloatingButtonItem({ children }: FloatingButtonItemProps) {
  return <motion.li variants={item}>{children}</motion.li>;
}

export { FloatingButton, FloatingButtonItem };

components/ui/page.tsx
import { cn } from "@/lib/utils";
import { DribbbleIcon, FacebookIcon, LinkedinIcon, PlusIcon } from "lucide-react";

import { Button } from "@/components/ui/button";
import { FloatingButton, FloatingButtonItem } from "./floating-button";

export default function FloatingButtonExample() {
  const items = [
    {
      id: "facebook",
      icon: <FacebookIcon />,
      bgColor: "bg-[#1877f2]"
    },
    {
      id: "dribbble",
      icon: <DribbbleIcon />,
      bgColor: "bg-[#ea4c89]"
    },
    {
      id: "linkedin",
      icon: <LinkedinIcon />,
      bgColor: "bg-[#0a66c2]"
    }
  ];

  return (
    <FloatingButton
      triggerContent={
        <Button size="icon" className="size-12 rounded-full">
          <PlusIcon className="size-5" />
        </Button>
      }>
      {items.map((item) => (
        <FloatingButtonItem key={item.id}>
          <Button size="icon" className={cn("size-12 rounded-full", item.bgColor)}>
            {item.icon}
          </Button>
        </FloatingButtonItem>
      ))}
    </FloatingButton>
  );
}

demo.tsx
import { FloatingButton, FloatingButtonItem }  from "@/components/ui/floating-button";
import { cn } from "@/lib/utils";
import { DribbbleIcon, FacebookIcon, LinkedinIcon, PlusIcon } from "lucide-react";

function FloatingButtonExample() {
  const items = [
    {
      icon: <FacebookIcon />,
      bgColor: 'bg-[#1877f2]'
    },
    {
      icon: <DribbbleIcon />,
      bgColor: 'bg-[#ea4c89]'
    },
    {
      icon: <LinkedinIcon />,
      bgColor: 'bg-[#0a66c2]'
    }
  ];

  return (
    <FloatingButton
      triggerContent={
        <button className="flex items-center justify-center h-12 w-12 rounded-full bg-black dark:bg-slate-800 text-white/80 z-10">
          <PlusIcon />
        </button>
      }>
      {items.map((item, key) => (
        <FloatingButtonItem key={key}>
          <button
            className={cn(
              'h-12 w-12 rounded-full flex items-center justify-center text-white/80',
              item.bgColor
            )}>
            {item.icon}
          </button>
        </FloatingButtonItem>
      ))}
    </FloatingButton>
  );
}

export { FloatingButtonExample };
```

Install NPM dependencies:
```bash
npm install framer-motion lucide-react motion usehooks-ts
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button floating-button.json
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
