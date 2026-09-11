<!-- Shuffle Button · @badtzx0 · https://21st.dev/@badtzx0/components/shuffle-button
     license: unspecified · category: text
     Here is Shuffle Button component -->

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
components/ui/shuffle-button.tsx
"use client"

import React, { useEffect, useRef, useState } from "react"

import { cn } from "@/lib/utils"

function shuffleChar(char: string): string {
  const characters = "abcdefghijklmnopqrstuvwxyz"
  return char === " "
    ? " "
    : characters[Math.floor(Math.random() * characters.length)]
}

interface ShuffleButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  children: string
  className?: string
  duration?: number
}

export function ShuffleButton({
  children,
  className,
  duration = 1,
  ...props
}: ShuffleButtonProps) {
  const [shuffledText, setShuffledText] = useState<string>(children)
  const [isHovering, setIsHovering] = useState<boolean>(false)
  const intervals = useRef<NodeJS.Timeout[]>([])
  const timeouts = useRef<NodeJS.Timeout[]>([])

  useEffect(() => {
    const textArray = children.split("")
    const numberOfCharacters = textArray.filter((char) => char !== " ").length
    const ABC = (duration * 500) / numberOfCharacters

    if (isHovering) {
      textArray.forEach((char, index) => {
        if (char !== " ") {
          const intervalId = setInterval(() => {
            textArray[index] = shuffleChar(char)
            setShuffledText(textArray.join(""))
          }, 25)
          intervals.current.push(intervalId)

          const timeoutId = setTimeout(
            () => {
              clearInterval(intervalId)
              textArray[index] = children[index]
              setShuffledText(textArray.join(""))
            },
            ABC * (index + 1)
          )
          timeouts.current.push(timeoutId)
        }
      })
    } else {
      textArray.forEach((char, index) => {
        if (char !== " ") {
          const intervalId = setInterval(() => {
            textArray[numberOfCharacters - 1 - index] = shuffleChar(char)
            setShuffledText(textArray.join(""))
          }, 25)
          intervals.current.push(intervalId)

          const timeoutId = setTimeout(
            () => {
              clearInterval(intervalId)
              textArray[numberOfCharacters - 1 - index] =
                children[numberOfCharacters - 1 - index]
              setShuffledText(textArray.join(""))
            },
            ABC * (index + 1)
          )
          timeouts.current.push(timeoutId)
        }
      })
    }

    return () => {
      intervals.current.forEach(clearInterval)
      timeouts.current.forEach(clearTimeout)
      intervals.current = []
      timeouts.current = []
    }
  }, [isHovering, children, duration])

  return (
    <button
      className={cn(
        "inline-flex h-10 items-center justify-center gap-2 rounded-md bg-neutral-100 px-4 py-2 font-mono text-sm font-medium whitespace-nowrap text-black transition-colors disabled:pointer-events-none disabled:opacity-50 dark:bg-neutral-900 dark:text-white [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0",
        className
      )}
      onMouseEnter={() => setIsHovering(true)}
      onMouseLeave={() => setIsHovering(false)}
      {...props}
    >
      {shuffledText}
    </button>
  )
}

demo.tsx
import { ShuffleButton } from "@/components/ui/shuffle-button"

export default function ShuffleButtonDemo() {
  return <ShuffleButton>Hover Me</ShuffleButton>
}
```

Install NPM dependencies:
```bash
npm install clsx tailwind-merge
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
