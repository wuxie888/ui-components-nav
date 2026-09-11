<!-- Text Gif · cult-ui · https://www.cult-ui.com/docs/components/text-gif
     license: MIT · category: text
     Text component with GIF-like animation effects and customizable styling -->

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
components/ui/text-gif.tsx
"use client"

import React, { useEffect, useMemo, useState, type CSSProperties } from "react"
import Image from "next/image"
import { cva, type VariantProps } from "class-variance-authority"

import { cn } from "@/lib/utils"

// Define text style variants
const textBaseVariants = cva("", {
  variants: {
    size: {
      default: "text-2xl sm:text-3xl lg:text-4xl",
      xxs: "text-base sm:text-lg lg:text-lg",
      xs: "text-lg sm:text-xl lg:text-2xl",
      sm: "text-xl sm:text-2xl lg:text-3xl",
      md: "text-2xl sm:text-3xl lg:text-4xl",
      lg: "text-3xl sm:text-4xl lg:text-5xl",
      xl: "text-4xl sm:text-5xl lg:text-6xl",
      xxl: "text-[2.5rem] sm:text-6xl lg:text-[6rem]",
      xll: "text-5xl sm:text-6xl lg:text-[7rem]",
      xxxl: "text-[6rem] leading-5 lg:leading-8 sm:text-6xl lg:text-[8rem]",
    },
    weight: {
      default: "font-bold",
      thin: "font-thin",
      base: "font-base",
      semi: "font-semibold",
      bold: "font-bold",
      black: "font-black",
    },
    font: {
      default: "font-sansTight",
      serif: "font-serif",
      mono: "font-mono",
    },
  },
  defaultVariants: {
    size: "default",
    weight: "bold",
    font: "default",
  },
})

interface TextGifProps extends VariantProps<typeof textBaseVariants> {
  gifUrl: string
  text: string
  className?: string
  fallbackColor?: string
  transitionDuration?: number
}

const TextGif = React.memo(function TextGifComponent({
  gifUrl,
  text,
  size,
  weight,
  font,
  className,
  fallbackColor = "black",
  transitionDuration = 300,
}: TextGifProps) {
  const [loaded, setLoaded] = useState(false)
  const [error, setError] = useState(false)

  // Reset states when gifUrl changes
  useEffect(() => {
    setLoaded(false)
    setError(false)
  }, [gifUrl])

  // Memoize className for performance
  const textClassName = useMemo(
    () =>
      cn(
        textBaseVariants({ size, weight, font }),
        loaded && !error ? "text-transparent bg-clip-text" : "",
        className,
        "pb-1.5 md:pb-4"
      ),
    [size, weight, font, className, loaded, error]
  )

  // Memoize style for performance
  const textStyle = useMemo(() => {
    const style: CSSProperties = {
      backgroundSize: "cover",
      backgroundPosition: "center",
      backgroundRepeat: "no-repeat",
      WebkitBackgroundClip: "text",
      lineHeight: 1,
      textAlign: "center",
      color: fallbackColor, // Always set the fallback color initially
      WebkitTextFillColor: fallbackColor, // Safari fix
      transition: `background-image ${transitionDuration}ms ease-in-out, color ${transitionDuration}ms ease-in-out`,
    }

    if (loaded && !error) {
      style.backgroundImage = `url(${gifUrl})`
      style.color = "transparent"
      style.WebkitTextFillColor = "transparent" // Safari fix
    }

    return style
  }, [loaded, error, gifUrl, transitionDuration, fallbackColor])

  return (
    <div className="relative inline-block">
      {/* Hidden image for preloading */}
      {gifUrl && (
        <Image
          src={gifUrl || "/placeholder.svg"}
          alt=""
          width={1}
          height={1}
          className="absolute opacity-0 pointer-events-none"
          onLoad={() => {
            setLoaded(true)
            setError(false)
          }}
          onError={() => {
            setError(true)
            setLoaded(false)
          }}
          priority
          unoptimized
        />
      )}
      <span className={textClassName} style={textStyle}>
        {text}
      </span>
    </div>
  )
})

// Export common GIF URLs
const gifUrls = [
  "https://media.giphy.com/media/3zvbrvbRe7wxBofOBI/giphy.gif",
  "https://media.giphy.com/media/fnglNFjBGiyAFtm6ke/giphy.gif",
  "https://media.giphy.com/media/9Pmfazv34l7aNIKK05/giphy.gif",
  "https://media.giphy.com/media/4bhs1boql4XVJgmm4H/giphy.gif",
]

// Optional: Preloader component
function PreloadGifs() {
  return (
    <div className="hidden">
      {gifUrls.map((url) => (
        <Image
          key={url}
          src={url}
          alt=""
          width={1}
          height={1}
          priority
          unoptimized
        />
      ))}
    </div>
  )
}

export { TextGif }
export default TextGif

demo.tsx
"use client"

import { useState } from "react"

import { Input } from "@/components/ui/input"
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select"

import { TextGif } from "../ui/text-gif"

export default function TextGifDemo() {
  const [text, setText] = useState("TextGif")
  const [size, setSize] = useState("xl")
  const [weight, setWeight] = useState("bold")

  const gifUrls = [
    "https://media.giphy.com/media/3zvbrvbRe7wxBofOBI/giphy.gif",
    "https://media.giphy.com/media/fnglNFjBGiyAFtm6ke/giphy.gif",
    "https://media.giphy.com/media/9Pmfazv34l7aNIKK05/giphy.gif",
    "https://media.giphy.com/media/4bhs1boql4XVJgmm4H/giphy.gif",
  ]

  const [selectedGif, setSelectedGif] = useState(gifUrls[0])

  return (
    <div className="max-w-3xl mx-auto space-y-8 p-6 bg-neutral-50 dark:bg-neutral-900 rounded-xl">
      {/* Preview */}
      <div className="flex items-center justify-center p-12 bg-white dark:bg-black rounded-xl">
        <TextGif
          gifUrl={selectedGif}
          text={text}
          size={size as any}
          weight={weight as any}
        />
      </div>

      {/* Controls */}
      <div className="grid gap-6 md:grid-cols-2">
        <div className="space-y-4">
          <label className="text-sm font-medium">Text</label>
          <Input
            value={text}
            onChange={(e) => setText(e.target.value)}
            placeholder="Enter text"
          />
        </div>

        <div className="space-y-4">
          <label className="text-sm font-medium">GIF Background</label>
          <Select value={selectedGif} onValueChange={setSelectedGif}>
            <SelectTrigger>
              <SelectValue placeholder="Select GIF" />
            </SelectTrigger>
            <SelectContent>
              {gifUrls.map((gif, index) => (
                <SelectItem key={index} value={gif}>
                  GIF {index + 1}
                </SelectItem>
              ))}
            </SelectContent>
          </Select>
        </div>

        <div className="space-y-4">
          <label className="text-sm font-medium">Text Size</label>
          <Select value={size} onValueChange={setSize}>
            <SelectTrigger>
              <SelectValue placeholder="Select size" />
            </SelectTrigger>
            <SelectContent>
              {["sm", "md", "lg", "xl", "xxl"].map((s) => (
                <SelectItem key={s} value={s}>
                  {s}
                </SelectItem>
              ))}
            </SelectContent>
          </Select>
        </div>

        <div className="space-y-4">
          <label className="text-sm font-medium">Font Weight</label>
          <Select value={weight} onValueChange={setWeight}>
            <SelectTrigger>
              <SelectValue placeholder="Select weight" />
            </SelectTrigger>
            <SelectContent>
              {["normal", "medium", "semi", "bold"].map((w) => (
                <SelectItem key={w} value={w}>
                  {w}
                </SelectItem>
              ))}
            </SelectContent>
          </Select>
        </div>
      </div>

      {/* Examples */}
      <div className="space-y-4">
        <h2 className="text-xl font-semibold">Examples</h2>
        <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div className="flex flex-col items-center justify-center p-6 bg-white dark:bg-black rounded-xl">
            <TextGif
              gifUrl={gifUrls[1]}
              text="Headings"
              size="xl"
              weight="bold"
            />
          </div>
          <div className="flex flex-col items-center justify-center p-6 bg-white dark:bg-black rounded-xl">
            <TextGif gifUrl={gifUrls[2]} text="$49" size="xxl" weight="bold" />
            <p className="text-sm mt-2">per month</p>
          </div>
        </div>
      </div>
    </div>
  )
}
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
