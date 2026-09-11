<!-- Typewriter · @danielpetho · https://21st.dev/@danielpetho/components/typewriter
     license: MIT · category: text
     A component that types out a text, one letter at a time.

As a text, either a string or an array of strings can be passed to the component. The component will automatically split the text into an array of characters, and animate each letter one by one. If you pass an array of strings, the component will animate one text, delete it, then continue animating the next one. The component will loop through the texts if you set the loop prop to true.

The cursor at the end of the text is optional. You can use a character or even a svg node if you like. There is a prop to customize the cursor animation, where you have to define the two motion variants as initial and animate.

Ideally, the component should respect multiple lines. If you experience otherwise, please let me know.:) -->

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
components/ui/typewriter.tsx
"use client"

import { ElementType, useEffect, useState } from "react"
import { motion, Variants } from "motion/react"

import { cn } from "@/lib/utils"

interface TypewriterProps {
  /**
   * Text or array of texts to type out
   */
  text: string | string[]

  /**
   * HTML Tag to render the component as
   * @default div
   */
  as?: ElementType

  /**
   * Speed of typing in milliseconds
   * @default 50
   */
  speed?: number

  /**
   * Initial delay before typing starts
   * @default 0
   */
  initialDelay?: number

  /**
   * Time to wait between typing and deleting
   * @default 2000
   */
  waitTime?: number

  /**
   * Speed of deleting characters
   * @default 30
   */
  deleteSpeed?: number

  /**
   * Whether to loop through texts array
   * @default true
   */
  loop?: boolean

  /**
   * Optional class name for styling
   */
  className?: string

  /**
   * Whether to show the cursor
   * @default true
   */
  showCursor?: boolean

  /**
   * Hide cursor while typing
   * @default false
   */
  hideCursorOnType?: boolean

  /**
   * Character or React node to use as cursor
   * @default "|"
   */
  cursorChar?: string | React.ReactNode

  /**
   * Animation variants for cursor
   */
  cursorAnimationVariants?: {
    initial: Variants["initial"]
    animate: Variants["animate"]
  }

  /**
   * Optional class name for cursor styling
   */
  cursorClassName?: string
}

const Typewriter = ({
  text,
  as: Tag = "div",
  speed = 50,
  initialDelay = 0,
  waitTime = 2000,
  deleteSpeed = 30,
  loop = true,
  className,
  showCursor = true,
  hideCursorOnType = false,
  cursorChar = "|",
  cursorClassName = "ml-1",
  cursorAnimationVariants = {
    initial: { opacity: 0 },
    animate: {
      opacity: 1,
      transition: {
        duration: 0.01,
        repeat: Infinity,
        repeatDelay: 0.4,
        repeatType: "reverse",
      },
    },
  },
  ...props
}: TypewriterProps & React.HTMLAttributes<HTMLElement>) => {
  const [displayText, setDisplayText] = useState("")
  const [currentIndex, setCurrentIndex] = useState(0)
  const [isDeleting, setIsDeleting] = useState(false)
  const [currentTextIndex, setCurrentTextIndex] = useState(0)

  const texts = Array.isArray(text) ? text : [text]

  useEffect(() => {
    let timeout: NodeJS.Timeout

    const currentText = texts[currentTextIndex]

    const startTyping = () => {
      if (isDeleting) {
        if (displayText === "") {
          setIsDeleting(false)
          if (currentTextIndex === texts.length - 1 && !loop) {
            return
          }
          setCurrentTextIndex((prev) => (prev + 1) % texts.length)
          setCurrentIndex(0)
          timeout = setTimeout(() => {}, waitTime)
        } else {
          timeout = setTimeout(() => {
            setDisplayText((prev) => prev.slice(0, -1))
          }, deleteSpeed)
        }
      } else {
        if (currentIndex < currentText.length) {
          timeout = setTimeout(() => {
            setDisplayText((prev) => prev + currentText[currentIndex])
            setCurrentIndex((prev) => prev + 1)
          }, speed)
        } else if (texts.length > 1) {
          timeout = setTimeout(() => {
            setIsDeleting(true)
          }, waitTime)
        }
      }
    }

    // Apply initial delay only at the start
    if (currentIndex === 0 && !isDeleting && displayText === "") {
      timeout = setTimeout(startTyping, initialDelay)
    } else {
      startTyping()
    }

    return () => clearTimeout(timeout)
  }, [
    currentIndex,
    displayText,
    isDeleting,
    speed,
    deleteSpeed,
    waitTime,
    texts,
    currentTextIndex,
    loop,
  ])

  return (
    <Tag className={cn("inline whitespace-pre-wrap tracking-tight", className)} {...props}>
      <span>{displayText}</span>
      {showCursor && (
        <motion.span
          variants={cursorAnimationVariants}
          className={cn(
            cursorClassName,
            hideCursorOnType &&
              (currentIndex < texts[currentTextIndex].length || isDeleting)
              ? "hidden"
              : ""
          )}
          initial="initial"
          animate="animate"
        >
          {cursorChar}
        </motion.span>
      )}
    </Tag>
  )
}

export default Typewriter

demo.tsx
import { Typewriter } from "@/components/ui/typewriter"

function Preview() {
  return (
    <div className="w-full h-full md:text-4xl lg:text-5xl sm:text-3xl text-2xl flex flex-row items-start justify-start bg-background font-normal overflow-hidden p-16 pt-48">
      <p className="whitespace-pre-wrap">
        <span>{"We're born 🌞 to "}</span>
        <Typewriter
          text={[
            "experience",
            "dance",
            "love",
            "be alive",
            "create things that make the world a better place",
          ]}
          speed={70}
          className="text-yellow-500"
          waitTime={1500}
          deleteSpeed={40}
          cursorChar={"_"}
        />
      </p>
    </div>
  )
}

export { Preview }
```

Install NPM dependencies:
```bash
npm install framer-motion motion
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
