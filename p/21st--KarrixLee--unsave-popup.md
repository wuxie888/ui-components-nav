<!-- Unsave Popup · @KarrixLee · https://21st.dev/@KarrixLee/components/unsave-popup
     license: MIT · category: alert
     A popup component to confirm before discarding changes. Inspired by Discord, it will shake when the user tries to leave without saving. -->

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
components/ui/unsave-popup.tsx
"use client";

import { Loader2, Save } from "lucide-react";
import { AnimatePresence, easeOut, motion, useAnimation } from "motion/react";
import { Button } from "@/components/ui/button";
import { useState, useEffect, useCallback, useMemo, memo } from "react";
import { cn } from "@/lib/utils";
import React from "react";

const UnsavePopupDescription = memo(
  ({ children }: { children: React.ReactNode }) => {
    return <div className="flex flex-row items-center gap-2">{children}</div>;
  }
);
UnsavePopupDescription.displayName = "UnsavePopupDescription";

const UnsavePopupAction = memo(
  ({
    children,
    isLoading,
    onClick,
  }: {
    children: React.ReactNode;
    isLoading?: boolean;
    onClick?: () => Promise<void>;
  }) => {
    return (
      <Button onClick={onClick} disabled={isLoading} className="rounded-lg">
        {isLoading ? (
          <span className="flex items-center gap-2">
            <Loader2 className="h-4 w-4 animate-spin" />
            Saving...
          </span>
        ) : (
          children
        )}
      </Button>
    );
  }
);
UnsavePopupAction.displayName = "UnsavePopupAction";

const UnsavePopupDismiss = memo(
  ({
    children,
    onClick,
  }: {
    children: React.ReactNode;
    onClick?: () => void;
  }) => {
    return (
      <Button variant="ghost" onClick={onClick} className="rounded-lg">
        {children}
      </Button>
    );
  }
);
UnsavePopupDismiss.displayName = "UnsavePopupDismiss";

// Main component
const UnsavePopup = memo(function UnsavePopup({
  children,
  onSave,
  onReset,
  shouldBlockFn,
  show,
  className,
}: {
  children: React.ReactNode;
  className?: string;
  onSave?: () => Promise<void>;
  onReset?: () => void;
  shouldBlockFn?: () => boolean;
  show: boolean;
}) {
  const controls = useAnimation();
  const [isLoading, setIsLoading] = useState(false);

  const shakeAnimation = useCallback(
    () => ({
      x: [0, -8, 12, -15, 8, -10, 5, -3, 2, -1, 0],
      y: [0, 4, -9, 6, -12, 8, -3, 5, -2, 1, 0],
      filter: [
        "blur(0px)",
        "blur(2px)",
        "blur(2px)",
        "blur(3px)",
        "blur(2px)",
        "blur(2px)",
        "blur(1px)",
        "blur(2px)",
        "blur(1px)",
        "blur(1px)",
        "blur(0px)",
      ],
      "--warning-opacity": [0, 0.5, 0.3, 0.1, 0], 
      transition: {
        duration: 0.4,
        ease: easeOut,
      },
    }),
    []
  );

  const triggerShake = useCallback(async () => {
    await controls.start(shakeAnimation());
  }, [controls, shakeAnimation]);

  const handleSave = useCallback(async () => {
    setIsLoading(true);
    await onSave?.();
    setIsLoading(false);
  }, [onSave]);

  const handleReset = useCallback(() => {
    onReset?.();
  }, [onReset]);

  const { hasCompoundComponents, hasValidComponents } = useMemo(() => {
    const childrenArray = React.Children.toArray(children);
    const hasCompound = childrenArray.some(
      (child) =>
        React.isValidElement(child) &&
        (child.type === UnsavePopupDescription ||
          child.type === UnsavePopupAction ||
          child.type === UnsavePopupDismiss)
    );

    if (!hasCompound) {
      return { hasCompoundComponents: false, hasValidComponents: true };
    }

    const hasDescription = childrenArray.some(
      (child) =>
        React.isValidElement(child) && child.type === UnsavePopupDescription
    );
    const hasAction = childrenArray.some(
      (child) => React.isValidElement(child) && child.type === UnsavePopupAction
    );
    const hasDismiss = childrenArray.some(
      (child) =>
        React.isValidElement(child) && child.type === UnsavePopupDismiss
    );

    return {
      hasCompoundComponents: true,
      hasValidComponents: hasDescription && hasAction && hasDismiss,
    };
  }, [children]);

  useEffect(() => {
    if (hasCompoundComponents && !hasValidComponents) {
      throw new Error(
        cn(
          "When using UnsavePopupDescription, UnsavePopupAction, or UnsavePopupDismiss, ",
          "you must use all three components together. Check out the docs for more info."
        )
      );
    }
  }, [hasCompoundComponents, hasValidComponents]);

  const defaultButtons = useCallback(
    () => (
      <div className="flex flex-row items-center gap-1">
        <Button variant="ghost" onClick={handleReset} className="rounded-lg">
          Reset
        </Button>
        <Button
          onClick={handleSave}
          disabled={isLoading}
          className="rounded-lg"
        >
          {isLoading ? (
            <span className="flex items-center gap-2">
              <Loader2 className="h-4 w-4 animate-spin" />
              Saving...
            </span>
          ) : (
            <span className="flex items-center gap-2">
              <Save className="h-4 w-4" />
              Save
            </span>
          )}
        </Button>
      </div>
    ),
    [isLoading, handleReset, handleSave]
  );

  useEffect(() => {
    if (shouldBlockFn && shouldBlockFn()) {
      triggerShake();
    }
  }, [shouldBlockFn, triggerShake]);

  return (
    <AnimatePresence>
      {show && (
        <motion.div
          initial={{ y: 100, opacity: 0 }}
          animate={{ y: 0, opacity: 1 }}
          exit={{ y: 100, opacity: 0 }}
          transition={{ type: "spring", bounce: 0.5, duration: 0.5 }}
          className="fixed right-0 bottom-4 left-0 z-50 mx-auto w-fit"
        >
          <motion.div
            animate={controls}
            className={cn(
              "flex w-96 flex-row items-center justify-between gap-2 rounded-lg",
              "border border-gray-200 bg-white dark:border-zinc-700",
              "dark:bg-gradient-to-br dark:from-zinc-900 dark:to-zinc-950",
              "px-4 py-2 text-sm shadow-md",
              "relative before:absolute before:inset-0 before:rounded-lg",
              "before:bg-red-500 [--warning-opacity:0]",
              "before:opacity-[var(--warning-opacity)]",
              "before:pointer-events-none",
              className
            )}
          >
            {hasCompoundComponents ? (
              children
            ) : (
              <>
                <div className="flex flex-row items-center gap-2">
                  {children}
                </div>
                {defaultButtons()}
              </>
            )}
          </motion.div>
        </motion.div>
      )}
    </AnimatePresence>
  );
});

UnsavePopup.displayName = "UnsavePopup";

export {
  UnsavePopup,
  UnsavePopupDescription,
  UnsavePopupAction,
  UnsavePopupDismiss,
};

demo.tsx
"use client"

import type React from "react"

import { Label } from "@/components/ui/label"
import { X, Info } from "lucide-react"
import { Input } from "@/components/ui/input"
import { useState, useCallback, useMemo } from "react"
import { Button } from "@/components/ui/button"
import { UnsavePopup } from "@/components/ui/unsave-popup"
import { cn } from "@/lib/utils"

export function UnsavePopupDemo() {
  const [value, setValue] = useState("")
  const [showPopup, setShowPopup] = useState(false)
  const [shouldBlockNav, setShouldBlockNav] = useState(false)
  const [closeForm, setCloseForm] = useState(false)
  const [isSaved, setIsSaved] = useState(false)

  const handleInputChange = useCallback((e: React.ChangeEvent<HTMLInputElement>) => {
    setValue(e.target.value)
    setShowPopup(true)
    setIsSaved(false)
  }, [])

  const handleSave = useCallback(async () => {
    await new Promise((resolve) => setTimeout(resolve, 1000))
    setShowPopup(false)
    setIsSaved(true)
  }, [])

  const handleReset = useCallback(() => {
    setValue("")
    setShowPopup(false)
    setShouldBlockNav(false)
  }, [])

  const handleCloseFormClick = useCallback(() => {
    if (value && !isSaved) {
      setShouldBlockNav(true)
      setTimeout(() => setShouldBlockNav(false), 100)
      return
    }
    setCloseForm(true)
  }, [value, isSaved])

  const shouldBlockFn = useCallback(() => shouldBlockNav, [shouldBlockNav])

  const formContent = useMemo(
    () => (
      <div className="grid w-full max-w-sm items-center gap-1.5">
        <Label htmlFor="name" className="font-medium">
          Name
        </Label>
        <Input
          placeholder="Type anything to trigger it..."
          value={value}
          className="text-sm"
          onChange={handleInputChange}
        />
      </div>
    ),
    [value, handleInputChange],
  )

  const popupContent = useMemo(
    () => (
      <>
        <Info className="h-4 w-4" /> Try to press the &quot;x&quot; to close it!
      </>
    ),
    [],
  )

  return (
    <div className="min-w-[400px]">
      {!closeForm && (
        <div key="preview" className="flex flex-col h-full items-center justify-center p-2">
          <div
            className={cn(
              "flex items-center justify-center p-4 gap-2 border border-gray-200",
              "rounded-lg w-full max-w-[500px] h-[300px] shadow-md relative",
            )}
          >
            {formContent}
            <div className="absolute top-1 right-1">
              <Button
                variant="ghost"
                size="icon"
                onClick={handleCloseFormClick}
                className="rounded-full hover:bg-transparent"
              >
                <X className="h-4 w-4 text-gray-500" />
              </Button>
            </div>
          </div>
        </div>
      )}

      <UnsavePopup
        onSave={handleSave}
        onReset={handleReset}
        shouldBlockFn={shouldBlockFn}
        show={showPopup}
        className="w-full"
      >
        {popupContent}
      </UnsavePopup>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
