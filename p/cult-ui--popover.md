<!-- Popover · cult-ui · https://www.cult-ui.com/docs/components/popover
     license: MIT · category: popover
     Animated popover component with form support and smooth transitions -->

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
components/ui/popover.tsx
"use client"

import React, {
  createContext,
  useContext,
  useEffect,
  useId,
  useRef,
  useState,
} from "react"
import { X } from "lucide-react"
import { AnimatePresence, motion, MotionConfig } from "motion/react"

import { cn } from "@/lib/utils"

const TRANSITION = {
  type: "spring" as const,
  bounce: 0.05,
  duration: 0.3,
}

function useClickOutside(
  ref: React.RefObject<HTMLElement | null>,
  handler: () => void
) {
  useEffect(() => {
    const handleClickOutside = (event: MouseEvent) => {
      if (ref.current && !ref.current.contains(event.target as Node)) {
        handler()
      }
    }

    document.addEventListener("mousedown", handleClickOutside)
    return () => {
      document.removeEventListener("mousedown", handleClickOutside)
    }
  }, [ref, handler])
}

interface PopoverContextType {
  isOpen: boolean
  openPopover: () => void
  closePopover: () => void
  uniqueId: string
  note: string
  setNote: (note: string) => void
}

const PopoverContext = createContext<PopoverContextType | undefined>(undefined)

function usePopover() {
  const context = useContext(PopoverContext)
  if (!context) {
    throw new Error("usePopover must be used within a PopoverProvider")
  }
  return context
}

function usePopoverLogic() {
  const uniqueId = useId()
  const [isOpen, setIsOpen] = useState(false)
  const [note, setNote] = useState("")

  const openPopover = () => setIsOpen(true)
  const closePopover = () => {
    setIsOpen(false)
    setNote("")
  }

  return { isOpen, openPopover, closePopover, uniqueId, note, setNote }
}

interface PopoverRootProps {
  children: React.ReactNode
  className?: string
}

export function PopoverRoot({ children, className }: PopoverRootProps) {
  const popoverLogic = usePopoverLogic()

  return (
    <PopoverContext.Provider value={popoverLogic}>
      <MotionConfig transition={TRANSITION}>
        <div
          className={cn(
            "relative flex items-center justify-center isolate",
            className
          )}
        >
          {children}
        </div>
      </MotionConfig>
    </PopoverContext.Provider>
  )
}

interface PopoverTriggerProps {
  children: React.ReactNode
  className?: string
}

export function PopoverTrigger({ children, className }: PopoverTriggerProps) {
  const { openPopover, uniqueId } = usePopover()

  return (
    <motion.button
      key="button"
      layoutId={`popover-${uniqueId}`}
      className={cn(
        "flex h-9 items-center border border-zinc-950/10 bg-white px-3 text-zinc-950 dark:border-zinc-50/10 dark:bg-zinc-700 dark:text-zinc-50",
        className
      )}
      style={{
        borderRadius: 8,
      }}
      onClick={openPopover}
    >
      <motion.span layoutId={`popover-label-${uniqueId}`} className="text-sm">
        {children}
      </motion.span>
    </motion.button>
  )
}

interface PopoverContentProps {
  children: React.ReactNode
  className?: string
}

export function PopoverContent({ children, className }: PopoverContentProps) {
  const { isOpen, closePopover, uniqueId } = usePopover()
  const formContainerRef = useRef<HTMLDivElement>(null)

  useClickOutside(formContainerRef, closePopover)

  useEffect(() => {
    const handleKeyDown = (event: KeyboardEvent) => {
      if (event.key === "Escape") {
        closePopover()
      }
    }

    document.addEventListener("keydown", handleKeyDown)

    return () => {
      document.removeEventListener("keydown", handleKeyDown)
    }
  }, [closePopover])

  return (
    <AnimatePresence>
      {isOpen && (
        <motion.div
          ref={formContainerRef}
          layoutId={`popover-${uniqueId}`}
          className={cn(
            "absolute h-[200px] w-[364px] overflow-hidden border border-zinc-950/10 bg-white outline-none dark:bg-zinc-700 z-50", // Changed z-90 to z-50
            className
          )}
          style={{
            borderRadius: 12,
            top: "auto", // Remove any top positioning
            left: "auto", // Remove any left positioning
            transform: "none", // Remove any transform
          }}
        >
          {children}
        </motion.div>
      )}
    </AnimatePresence>
  )
}

interface PopoverFormProps {
  children: React.ReactNode
  onSubmit?: (note: string) => void
  className?: string
}

export function PopoverForm({
  children,
  onSubmit,
  className,
}: PopoverFormProps) {
  const { note, closePopover } = usePopover()

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault()
    onSubmit?.(note)
    closePopover()
  }

  return (
    <form
      className={cn("flex h-full flex-col", className)}
      onSubmit={handleSubmit}
    >
      {children}
    </form>
  )
}

interface PopoverLabelProps {
  children: React.ReactNode
  className?: string
}

export function PopoverLabel({ children, className }: PopoverLabelProps) {
  const { uniqueId, note } = usePopover()

  return (
    <motion.span
      layoutId={`popover-label-${uniqueId}`}
      aria-hidden="true"
      style={{
        opacity: note ? 0 : 1,
      }}
      className={cn(
        "absolute left-4 top-3 select-none text-sm text-zinc-500 dark:text-zinc-400",
        className
      )}
    >
      {children}
    </motion.span>
  )
}

interface PopoverTextareaProps {
  className?: string
}

export function PopoverTextarea({ className }: PopoverTextareaProps) {
  const { note, setNote } = usePopover()

  return (
    <textarea
      className={cn(
        "h-full w-full resize-none rounded-md bg-transparent px-4 py-3 text-sm outline-none",
        className
      )}
      autoFocus
      value={note}
      onChange={(e) => setNote(e.target.value)}
    />
  )
}

interface PopoverFooterProps {
  children: React.ReactNode
  className?: string
}

export function PopoverFooter({ children, className }: PopoverFooterProps) {
  return (
    <div
      key="close"
      className={cn("flex justify-between px-4 py-3", className)}
    >
      {children}
    </div>
  )
}

interface PopoverCloseButtonProps {
  className?: string
}

export function PopoverCloseButton({ className }: PopoverCloseButtonProps) {
  const { closePopover } = usePopover()

  return (
    <button
      type="button"
      className={cn("flex items-center", className)}
      onClick={closePopover}
      aria-label="Close popover"
    >
      <X size={16} className="text-zinc-900 dark:text-zinc-100" />
    </button>
  )
}

interface PopoverSubmitButtonProps {
  className?: string
}

export function PopoverSubmitButton({ className }: PopoverSubmitButtonProps) {
  return (
    <button
      className={cn(
        "relative ml-1 flex h-8 shrink-0 scale-100 select-none appearance-none items-center justify-center rounded-lg border border-zinc-950/10 bg-transparent px-2 text-sm text-zinc-500 transition-colors hover:bg-zinc-100 hover:text-zinc-800 focus-visible:ring-2 active:scale-[0.98] dark:border-zinc-50/10 dark:text-zinc-50 dark:hover:bg-zinc-800",
        className
      )}
      type="submit"
      aria-label="Submit note"
    >
      Submit
    </button>
  )
}

export function PopoverHeader({
  children,
  className,
}: {
  children: React.ReactNode
  className?: string
}) {
  return (
    <div
      className={cn(
        "px-4 py-2 font-semibold text-zinc-900 dark:text-zinc-100",
        className
      )}
    >
      {children}
    </div>
  )
}

export function PopoverBody({
  children,
  className,
}: {
  children: React.ReactNode
  className?: string
}) {
  return <div className={cn("p-4", className)}>{children}</div>
}

// New component: PopoverButton
export function PopoverButton({
  children,
  onClick,
  className,
}: {
  children: React.ReactNode
  onClick?: () => void
  className?: string
}) {
  return (
    <button
      className={cn(
        "flex w-full items-center gap-2 rounded-md px-4 py-2 text-left text-sm hover:bg-zinc-100 dark:hover:bg-zinc-700",
        className
      )}
      onClick={onClick}
    >
      {children}
    </button>
  )
}

demo.tsx
"use client"

import React from "react"
import { Image as ImageIcon, Paintbrush, Plus } from "lucide-react"

import {
  PopoverBody,
  PopoverButton,
  PopoverCloseButton,
  PopoverContent,
  PopoverFooter,
  PopoverForm,
  PopoverHeader,
  PopoverLabel,
  PopoverRoot,
  PopoverSubmitButton,
  PopoverTextarea,
  PopoverTrigger,
} from "../ui/popover"

function PopoverInput() {
  const handleSubmit = (note: string) => {
    console.log("Submitted note:", note)
  }

  return (
    <PopoverRoot>
      <PopoverTrigger>Add Feedback</PopoverTrigger>
      <PopoverContent>
        <PopoverForm onSubmit={handleSubmit}>
          <PopoverLabel>Add Feedback</PopoverLabel>
          <PopoverTextarea />
          <PopoverFooter>
            <PopoverCloseButton />
            <PopoverSubmitButton />
          </PopoverFooter>
        </PopoverForm>
      </PopoverContent>
    </PopoverRoot>
  )
}

const ColorPickerPopover = () => {
  const colors = [
    "#FF5733",
    "#33FF57",
    "#3357FF",
    "#FF33F1",
    "#33FFF1",
    "#F1FF33",
  ]

  return (
    <PopoverRoot>
      <PopoverTrigger>Choose Color</PopoverTrigger>
      <PopoverContent className="w-48 h-48">
        <PopoverHeader>Pick a Color</PopoverHeader>
        <PopoverBody className="flex flex-col justify-center items-center">
          <div className="grid grid-cols-3 gap-2">
            {colors.map((color) => (
              <button
                key={color}
                className="w-8 h-8 rounded-full focus:outline-none focus:ring-2 focus:ring-offset-2"
                style={{ backgroundColor: color }}
                onClick={() => console.log(`Selected color: ${color}`)}
              />
            ))}
          </div>
        </PopoverBody>
        <PopoverFooter>
          <PopoverCloseButton />
        </PopoverFooter>
      </PopoverContent>
    </PopoverRoot>
  )
}

const QuickActionsPopover = () => {
  const actions = [
    {
      icon: <Plus className="w-4 h-4" />,
      label: "New File",
      action: () => console.log("New File"),
    },
    {
      icon: <ImageIcon className="w-4 h-4" />,
      label: "Upload Image",
      action: () => console.log("Upload Image"),
    },
    {
      icon: <Paintbrush className="w-4 h-4" />,
      label: "Edit Colors",
      action: () => console.log("Edit Colors"),
    },
  ]

  return (
    <PopoverRoot>
      <PopoverTrigger>Quick Actions</PopoverTrigger>
      <PopoverContent className="w-48 h-48">
        <PopoverHeader>Quick Actions</PopoverHeader>
        <PopoverBody>
          {actions.map((action, index) => (
            <PopoverButton key={index} onClick={action.action}>
              {action.icon}
              <span>{action.label}</span>
            </PopoverButton>
          ))}
        </PopoverBody>
      </PopoverContent>
    </PopoverRoot>
  )
}

const ImagePreviewPopover = () => {
  return (
    <PopoverRoot>
      <PopoverTrigger>Preview Image</PopoverTrigger>
      <PopoverContent className="w-96 h-[500px]">
        <PopoverHeader>Image Preview</PopoverHeader>
        <PopoverBody>
          <img
            src="/placeholder.svg?height=200&width=300"
            alt="Preview"
            className="w-full h-auto rounded-md"
          />
          <p className="mt-2 text-sm text-zinc-600 dark:text-zinc-400">
            Image preview description goes here.
          </p>
        </PopoverBody>
        <PopoverFooter>
          <PopoverCloseButton />
        </PopoverFooter>
      </PopoverContent>
    </PopoverRoot>
  )
}

export default function PopoverExamples() {
  return (
    <div className="p-8 space-y-8">
      <div className="grid md:grid-cols-2 gap-36">
        <div className="flex flex-col gap-24 flex-wrap ">
          <PopoverInput />
          <ColorPickerPopover />
        </div>
        <div className="flex flex-col gap-24 flex-wrap ">
          <QuickActionsPopover />
          <ImagePreviewPopover />
        </div>
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install motion
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
