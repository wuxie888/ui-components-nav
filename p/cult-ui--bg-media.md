<!-- Bg Media · cult-ui · https://www.cult-ui.com/docs/components/bg-media
     license: MIT · category: background
     Background media component with video/image support and overlay effects -->

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
components/ui/bg-media.tsx
"use client"

import React, { useRef, useState } from "react"
import { cva } from "class-variance-authority"

import { cn } from "@/lib/utils"

// Make sure this utility exists in your project for combining class names

// Define the type for the variant and type props
type OverlayVariant = "none" | "light" | "dark"
type MediaType = "image" | "video"

// Update the cva call with these types
const backgroundVariants = cva(
  "relative h-screen max-h-[1000px] w-full min-h-[500px] lg:min-h-[600px]",
  {
    variants: {
      overlay: {
        none: "",
        light:
          "before:absolute before:inset-0 before:bg-white before:opacity-30",
        dark: "before:absolute before:inset-0 before:bg-black before:opacity-30",
      },
      type: {
        image: "",
        video: "z-10",
      },
    },
    defaultVariants: {
      overlay: "none",
      type: "image",
    },
  }
)

interface BackgroundMediaProps {
  variant?: OverlayVariant
  type?: MediaType
  src: string
  alt?: string
}

export const BackgroundMedia: React.FC<BackgroundMediaProps> = ({
  variant = "light",
  type = "image",
  src,
  alt = "",
}) => {
  const [isPlaying, setIsPlaying] = useState(true)
  const mediaRef = useRef<HTMLVideoElement | null>(null)

  const toggleMediaPlay = () => {
    if (type === "video" && mediaRef.current) {
      if (isPlaying) {
        mediaRef.current.pause()
      } else {
        mediaRef.current.play()
      }
      setIsPlaying(!isPlaying)
    }
  }

  const mediaClasses = cn(
    backgroundVariants({ overlay: variant, type }),
    "overflow-hidden"
  )

  const renderMedia = () => {
    if (type === "video") {
      return (
        <video
          ref={mediaRef}
          aria-hidden="true"
          muted
          className="absolute inset-0 h-full w-full object-cover transition-opacity duration-300 pointer-events-none"
          autoPlay
          playsInline
        >
          <source src={src} type="video/mp4" />
          Your browser does not support the video tag.
        </video>
      )
    } else {
      return (
        <img
          src={src}
          alt={alt}
          className="absolute inset-0 h-full w-full object-cover rounded-br-[88px]"
          loading="eager"
        />
      )
    }
  }

  return (
    <div className={mediaClasses}>
      {renderMedia()}
      {type === "video" && (
        <button
          aria-label={isPlaying ? "Pause video" : "Play video"}
          className="absolute bottom-4 right-4 z-50 px-4 py-2 bg-gray-900 text-white hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-gray-500 focus:ring-offset-2"
          onClick={toggleMediaPlay}
        >
          {isPlaying ? "Pause" : "Play"}
        </button>
      )}
    </div>
  )
}

export default BackgroundMedia

demo.tsx
import BackgroundMedia from "../ui/bg-media"

export default function BgMediaDemo() {
  return (
    <div className="w-full ">
      <div className="md:min-w-[20rem] ">
        <BackgroundMedia
          type="video"
          variant="light"
          src="https://openaicomproductionae4b.blob.core.windows.net/production-twill-01/c74791d0-75d2-48e6-acae-96d13bc97c56/paper-planes.mp4"
        />
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
