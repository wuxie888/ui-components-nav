<!-- Scramble Hover · @danielpetho · https://21st.dev/@danielpetho/components/scramble-hover
     license: MIT · category: text
     A text component that scrambles the text on hover.

For the scrambling effect, you can use either the original characters, or another set of characters specified in the characters prop. -->

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
components/ui/scramble-hover.tsx
"use client"

import { useEffect, useState } from "react"
import { motion } from "motion/react"

import { cn } from "@/lib/utils"

interface ScrambleHoverProps {
  text: string
  scrambleSpeed?: number
  maxIterations?: number
  sequential?: boolean
  revealDirection?: "start" | "end" | "center"
  useOriginalCharsOnly?: boolean
  characters?: string
  className?: string
  scrambledClassName?: string
}

const ScrambleHover: React.FC<ScrambleHoverProps> = ({
  text,
  scrambleSpeed = 50,
  maxIterations = 10,
  useOriginalCharsOnly = false,
  characters = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz!@#$%^&*()_+",
  className,
  scrambledClassName,
  sequential = false,
  revealDirection = "start",
  ...props
}) => {
  const [displayText, setDisplayText] = useState(text)
  const [isHovering, setIsHovering] = useState(false)
  const [isScrambling, setIsScrambling] = useState(false)
  const [revealedIndices, setRevealedIndices] = useState(new Set<number>())

  useEffect(() => {
    let interval: NodeJS.Timeout
    let currentIteration = 0

    const getNextIndex = () => {
      const textLength = text.length
      switch (revealDirection) {
        case "start":
          return revealedIndices.size
        case "end":
          return textLength - 1 - revealedIndices.size
        case "center":
          const middle = Math.floor(textLength / 2)
          const offset = Math.floor(revealedIndices.size / 2)
          const nextIndex =
            revealedIndices.size % 2 === 0
              ? middle + offset
              : middle - offset - 1

          if (
            nextIndex >= 0 &&
            nextIndex < textLength &&
            !revealedIndices.has(nextIndex)
          ) {
            return nextIndex
          }

          for (let i = 0; i < textLength; i++) {
            if (!revealedIndices.has(i)) return i
          }
          return 0
        default:
          return revealedIndices.size
      }
    }

    const shuffleText = (text: string) => {
      if (useOriginalCharsOnly) {
        const positions = text.split("").map((char, i) => ({
          char,
          isSpace: char === " ",
          index: i,
          isRevealed: revealedIndices.has(i),
        }))

        const nonSpaceChars = positions
          .filter((p) => !p.isSpace && !p.isRevealed)
          .map((p) => p.char)

        // Shuffle remaining non-revealed, non-space characters
        for (let i = nonSpaceChars.length - 1; i > 0; i--) {
          const j = Math.floor(Math.random() * (i + 1))
          ;[nonSpaceChars[i], nonSpaceChars[j]] = [
            nonSpaceChars[j],
            nonSpaceChars[i],
          ]
        }

        let charIndex = 0
        return positions
          .map((p) => {
            if (p.isSpace) return " "
            if (p.isRevealed) return text[p.index]
            return nonSpaceChars[charIndex++]
          })
          .join("")
      } else {
        return text
          .split("")
          .map((char, i) => {
            if (char === " ") return " "
            if (revealedIndices.has(i)) return text[i]
            return availableChars[
              Math.floor(Math.random() * availableChars.length)
            ]
          })
          .join("")
      }
    }

    const availableChars = useOriginalCharsOnly
      ? Array.from(new Set(text.split(""))).filter((char) => char !== " ")
      : characters.split("")

    if (isHovering) {
      setIsScrambling(true)
      interval = setInterval(() => {
        if (sequential) {
          if (revealedIndices.size < text.length) {
            const nextIndex = getNextIndex()
            revealedIndices.add(nextIndex)
            setDisplayText(shuffleText(text))
          } else {
            clearInterval(interval)
            setIsScrambling(false)
          }
        } else {
          setDisplayText(shuffleText(text))
          currentIteration++
          if (currentIteration >= maxIterations) {
            clearInterval(interval)
            setIsScrambling(false)
            setDisplayText(text)
          }
        }
      }, scrambleSpeed)
    } else {
      setDisplayText(text)
      revealedIndices.clear()
    }

    return () => {
      if (interval) clearInterval(interval)
    }
  }, [
    isHovering,
    text,
    characters,
    scrambleSpeed,
    useOriginalCharsOnly,
    sequential,
    revealDirection,
    maxIterations,
  ])

  return (
    <motion.span
      onHoverStart={() => setIsHovering(true)}
      onHoverEnd={() => setIsHovering(false)}
      className={cn("inline-block whitespace-pre-wrap", className)}
      {...props}
    >
      <span className="sr-only">{displayText}</span>
      <span aria-hidden="true">
        {displayText.split("").map((char, index) => (
          <span
            key={index}
            className={cn(
              revealedIndices.has(index) || !isScrambling || !isHovering
                ? className
                : scrambledClassName
            )}
          >
            {char}
          </span>
        ))}
      </span>
    </motion.span>
  )
}

export default ScrambleHover

demo.tsx
import { ScrambleHover } from "@/components/ui/scramble-hover"

function Preview() {
  return (
    <div className="w-full h-full  text-xl sm:text-3xl md:text-5xl bg-background font-normal overflow-hidden p-12 sm:p-20 flex flex-col md:p-24 space-y-2 space-x-6">
      <ScrambleHover
        text={"original characters"}
        scrambleSpeed={50}
        maxIterations={8}
        useOriginalCharsOnly={true}
        className="cursor-pointer"
      />
      <ScrambleHover
        text={"new characters"}
        scrambleSpeed={50}
        maxIterations={8}
        useOriginalCharsOnly={false}
        className="cursor-pointer"
        characters="abcdefghijklmnopqrstuvwxyz!@#$%^&*()_+-=[]{}|;':\,./<>?"
      />
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
