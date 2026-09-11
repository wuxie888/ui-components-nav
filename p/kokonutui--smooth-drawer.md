<!-- smooth-drawer · Kokonut UI · https://kokonutui.com/docs/navigation/smooth-drawer
     license: MIT · category: pricing-section
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
components/kokonutui/smooth-drawer.tsx
"use client";

/**
 * @author: @dorianbaffier
 * @description: Smooth Drawer
 * @version: 1.0.0
 * @date: 2025-06-26
 * @license: MIT
 * @website: https://kokonutui.com
 * @github: https://github.com/kokonut-labs/kokonutui
 */

import { Fingerprint } from "lucide-react";
import { motion } from "motion/react";
import Image from "next/image";
import Link from "next/link";
import type * as React from "react";
import { Button } from "@/components/ui/button";
import {
  Drawer,
  DrawerClose,
  DrawerContent,
  DrawerDescription,
  DrawerFooter,
  DrawerHeader,
  DrawerTitle,
  DrawerTrigger,
} from "@/components/ui/drawer";

interface PriceTagProps {
  price: number;
  discountedPrice: number;
}

function PriceTag({ price, discountedPrice }: PriceTagProps) {
  return (
    <div className="mx-auto flex max-w-fit items-center justify-around gap-4">
      <div className="flex items-baseline gap-2">
        <span className="bg-gradient-to-br from-zinc-900 to-zinc-700 bg-clip-text font-bold text-4xl text-transparent dark:from-white dark:to-zinc-300">
          ${discountedPrice}
        </span>
        <span className="text-lg text-zinc-400 line-through dark:text-zinc-500">
          ${price}
        </span>
      </div>
      <div className="flex flex-col items-center gap-0.5">
        <span className="font-medium text-sm text-zinc-900 dark:text-zinc-100">
          Lifetime access
        </span>
        <span className="text-xs text-zinc-700 dark:text-zinc-300">
          One-time payment
        </span>
      </div>
    </div>
  );
}

interface DrawerDemoProps extends React.HTMLAttributes<HTMLDivElement> {
  title?: string;
  description?: string;
  primaryButtonText?: string;
  secondaryButtonText?: string;
  onPrimaryAction?: () => void;
  onSecondaryAction?: () => void;
  price?: number;
  discountedPrice?: number;
}

const drawerVariants = {
  hidden: {
    y: "100%",
    opacity: 0,
    rotateX: 5,
    transition: {
      type: "spring",
      stiffness: 300,
      damping: 30,
    },
  },
  visible: {
    y: 0,
    opacity: 1,
    rotateX: 0,
    transition: {
      type: "spring",
      stiffness: 300,
      damping: 30,
      mass: 0.8,
      staggerChildren: 0.07,
      delayChildren: 0.2,
    },
  },
};

const itemVariants = {
  hidden: {
    y: 20,
    opacity: 0,
    transition: {
      type: "spring",
      stiffness: 300,
      damping: 30,
    },
  },
  visible: {
    y: 0,
    opacity: 1,
    transition: {
      type: "spring",
      stiffness: 300,
      damping: 30,
      mass: 0.8,
    },
  },
};

export default function SmoothDrawer({
  title = "KokonutUI - Pro",
  description = "100+ collection of UI Components and templates built for React, Next.js, and Tailwind CSS. Spend no time on design and focus on shipping.",
  primaryButtonText = "Buy Now",
  secondaryButtonText = "Maybe Later",
  onSecondaryAction,
  price = 169,
  discountedPrice = 99,
}: DrawerDemoProps) {
  const handleSecondaryClick = () => {
    onSecondaryAction?.();
  };

  return (
    <Drawer>
      <DrawerTrigger asChild>
        <Button variant="outline">Open Drawer</Button>
      </DrawerTrigger>
      <DrawerContent className="mx-auto max-w-fit rounded-2xl p-6 shadow-xl">
        <motion.div
          animate="visible"
          className="mx-auto w-full max-w-[340px] space-y-6"
          initial="hidden"
          variants={drawerVariants as any}
        >
          <motion.div variants={itemVariants as any}>
            <DrawerHeader className="space-y-2.5 px-0">
              <DrawerTitle className="flex items-center gap-2.5 font-semibold text-2xl tracking-tighter">
                <motion.div variants={itemVariants as any}>
                  <div className="rounded-xl bg-gradient-to-br from-zinc-100 to-zinc-200 p-1.5 shadow-inner dark:from-zinc-800 dark:to-zinc-900">
                    <Image alt="Logo" height={32} src="/logo.svg" width={32} />
                  </div>
                </motion.div>
                <motion.span variants={itemVariants as any}>
                  {title}
                </motion.span>
              </DrawerTitle>
              <motion.div variants={itemVariants as any}>
                <DrawerDescription className="text-sm text-zinc-600 leading-relaxed tracking-tighter dark:text-zinc-400">
                  {description}
                </DrawerDescription>
              </motion.div>
            </DrawerHeader>
          </motion.div>

          <motion.div variants={itemVariants as any}>
            <PriceTag discountedPrice={discountedPrice} price={price} />
          </motion.div>

          <motion.div variants={itemVariants as any}>
            <DrawerFooter className="flex flex-col gap-3 px-0">
              <div className="w-full">
                <Link
                  className="group relative inline-flex h-11 w-full items-center justify-center overflow-hidden rounded-xl bg-gradient-to-r from-rose-500 to-pink-500 font-semibold text-sm text-white tracking-wide shadow-lg shadow-rose-500/20 transition-all duration-500 hover:from-rose-600 hover:to-pink-600 hover:shadow-rose-500/30 hover:shadow-xl dark:from-rose-600 dark:to-pink-600 dark:hover:from-rose-500 dark:hover:to-pink-500"
                  href="https://kokonutui.pro/#pricing"
                  target="_blank"
                >
                  <motion.span
                    className="absolute inset-0 translate-x-[-200%] bg-gradient-to-r from-transparent via-white/20 to-transparent"
                    transition={{
                      duration: 1.5,
                      ease: "easeInOut",
                      repeat: 0,
                    }}
                    whileHover={{
                      x: ["-200%", "200%"],
                    }}
                  />
                  <motion.div
                    animate={{ opacity: 1 }}
                    className="relative flex items-center gap-2 tracking-tighter"
                    initial={{ opacity: 0 }}
                    transition={{ duration: 0.3 }}
                  >
                    {primaryButtonText}
                    <motion.div
                      animate={{
                        rotate: [0, 15, -15, 0],
                        y: [0, -2, 2, 0],
                      }}
                      transition={{
                        duration: 2,
                        ease: "easeInOut",
                        repeat: Number.POSITIVE_INFINITY,
                        repeatDelay: 1,
                      }}
                    >
                      <Fingerprint className="h-4 w-4" />
                    </motion.div>
                  </motion.div>
                </Link>
              </div>
              <DrawerClose asChild>
                <Button
                  className="h-11 w-full rounded-xl border-zinc-200 font-semibold text-sm tracking-tighter transition-colors hover:bg-zinc-100 dark:border-zinc-800 dark:hover:bg-zinc-800/80"
                  onClick={handleSecondaryClick}
                  variant="outline"
                >
                  {secondaryButtonText}
                </Button>
              </DrawerClose>
            </DrawerFooter>
          </motion.div>
        </motion.div>
      </DrawerContent>
    </Drawer>
  );
}

demo.tsx
import SmoothDrawer from "@/components/ui/smooth-drawer";

export default function Default() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center p-6">
      <SmoothDrawer />
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
npx shadcn@latest add button drawer
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
