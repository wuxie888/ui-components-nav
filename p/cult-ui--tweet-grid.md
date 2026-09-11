<!-- Tweet Grid · cult-ui · https://www.cult-ui.com/docs/components/tweet-grid
     license: MIT · category: grid
     Grid layout component for displaying tweet-like content cards -->

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
components/ui/tweet-grid.tsx
"use client"

import * as React from "react"
import { cva, type VariantProps } from "class-variance-authority"

import { cn } from "@/lib/utils"

const tweetGridVariants = cva("max-w-4xl md:max-w-6xl px-2", {
  variants: {
    columns: {
      1: "columns-1",
      2: "sm:columns-2",
      3: "md:columns-3",
      4: "lg:columns-4",
      5: "xl:columns-5",
    },
  },
  defaultVariants: {
    columns: 3,
  },
})

const tweetItemVariants = cva("break-inside-avoid", {
  variants: {
    spacing: {
      sm: "mb-2",
      md: "mb-4",
      lg: "mb-6",
    },
  },
  defaultVariants: {
    spacing: "md",
  },
})

export interface TweetGridProps
  extends VariantProps<typeof tweetGridVariants>,
    VariantProps<typeof tweetItemVariants> {
  tweets: string[]
  className?: string
}

// Mock Tweet component to avoid react-tweet CSS import issues
const MockTweet: React.FC<{ id: string }> = ({ id }) => {
  return (
    <div className="bg-white dark:bg-gray-800 border border-gray-200 dark:border-gray-700 rounded-lg p-4 shadow-sm">
      <div className="flex items-center space-x-3 mb-3">
        <div className="w-10 h-10 bg-blue-500 rounded-full flex items-center justify-center">
          <span className="text-white font-bold text-sm">T</span>
        </div>
        <div>
          <div className="font-semibold text-gray-900 dark:text-white">
            Twitter User
          </div>
          <div className="text-sm text-gray-500 dark:text-gray-400">@user</div>
        </div>
      </div>
      <div className="text-gray-900 dark:text-white mb-3">
        This is a mock tweet placeholder. Tweet ID: {id}
      </div>
      <div className="flex items-center space-x-4 text-sm text-gray-500 dark:text-gray-400">
        <span>💬 0</span>
        <span>🔄 0</span>
        <span>❤️ 0</span>
      </div>
    </div>
  )
}

export const TweetGrid: React.FC<TweetGridProps> = ({
  tweets,
  columns,
  spacing,
  className,
}) => {
  return (
    <div className={cn(tweetGridVariants({ columns }), className)}>
      {tweets.map((tweetId, i) => (
        <div
          key={`${tweetId}-${i}`}
          className={cn(tweetItemVariants({ spacing }))}
        >
          <MockTweet id={tweetId} />
        </div>
      ))}
    </div>
  )
}

demo.tsx
"use client"

import * as React from "react"

import { GradientHeading } from "../ui/gradient-heading"
import { TweetGrid } from "../ui/tweet-grid"

// Grab tweet ids
const exampleTweets = [
  "1742983975340327184",
  "1743049700583116812",
  "1754067409366073443",
  "1753968111059861648",
  "1754174981897118136",
  "1743632296802988387",
  "1754110885168021921",
  "1760248682828419497",
  "1760230134601122153",
  "1760184980356088267",
]

export default function TweetGridDemo({}) {
  return (
    <div className="pb-12 md:max-w-4xl max-w-md">
      <div className="flex w-full justify-center pb-12">
        <GradientHeading size="xl" weight="black">
          Join the club
        </GradientHeading>
      </div>
      <div className="flex items-center justify-center w-full">
        <TweetGrid
          className="w-80 md:w-full "
          tweets={exampleTweets}
          columns={2}
          spacing="lg"
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
