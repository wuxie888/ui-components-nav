<!-- Video Player · @thegridcn · https://21st.dev/@thegridcn/components/video-player
     license: no-license · category: video
     A retro sci-fi HUD-style video player frame with bracket corners, scanline overlay, transport controls, and an elapsed-time readout. -->

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
components/thegridcn/video-player.tsx
"use client";

import {
  FastForward,
  Pause,
  Play,
  Rewind,
  SkipBack,
  SkipForward,
} from "lucide-react";
import type * as React from "react";
import { cn } from "@/lib/utils";

interface VideoPlayerProps extends React.HTMLAttributes<HTMLDivElement> {
  currentTime?: string;
  onFastForward?: () => void;
  onPause?: () => void;
  onPlay?: () => void;
  onRewind?: () => void;
  status?: "playing" | "paused" | "stopped";
}

export function VideoPlayer({
  currentTime = "00:00:00",
  status = "paused",
  onPlay,
  onPause,
  onRewind,
  onFastForward,
  children,
  className,
  ...props
}: VideoPlayerProps) {
  return (
    <div className={cn("relative", className)} {...props}>
      {/* Video frame with bracket corners */}
      <div className="relative overflow-hidden rounded border border-primary/30 bg-black">
        {/* Corner brackets */}
        <div className="pointer-events-none absolute top-2 left-2 h-6 w-6 border-primary border-t-2 border-l-2" />
        <div className="pointer-events-none absolute top-2 right-2 h-6 w-6 border-primary border-t-2 border-r-2" />
        <div className="pointer-events-none absolute bottom-12 left-2 h-6 w-6 border-primary border-b-2 border-l-2" />
        <div className="pointer-events-none absolute right-2 bottom-12 h-6 w-6 border-primary border-r-2 border-b-2" />

        {/* Video content area */}
        <div className="aspect-video bg-muted/20">{children}</div>

        {/* Scanline overlay */}
        <div className="pointer-events-none absolute inset-0 bg-[repeating-linear-gradient(0deg,transparent,transparent_2px,rgba(0,0,0,0.05)_2px,rgba(0,0,0,0.05)_4px)]" />

        {/* Controls bar */}
        <div className="border-primary/30 border-t bg-background/80 px-4 py-3 backdrop-blur-sm">
          <div className="flex items-center justify-center gap-2">
            {/* Left bracket */}
            <span className="font-mono text-lg text-primary/50">[</span>

            {/* Rewind 10 */}
            <button
              type="button"
              onClick={onRewind}
              className="flex items-center gap-1 px-2 py-1 font-mono text-foreground/80 text-xs transition-colors hover:text-primary"
            >
              <span className="text-[10px]">—</span>
              <span>10</span>
              <Rewind className="h-4 w-4" />
            </button>

            {/* Skip back */}
            <button
              type="button"
              className="p-1 text-foreground/80 transition-colors hover:text-primary"
            >
              <SkipBack className="h-4 w-4" />
            </button>

            {/* Play/Pause */}
            <button
              type="button"
              onClick={status === "playing" ? onPause : onPlay}
              className="rounded border border-primary/50 p-2 text-primary transition-all hover:bg-primary/20"
            >
              {status === "playing" ? (
                <Pause className="h-5 w-5" />
              ) : (
                <Play className="h-5 w-5" />
              )}
            </button>

            {/* Skip forward */}
            <button
              type="button"
              className="p-1 text-foreground/80 transition-colors hover:text-primary"
            >
              <SkipForward className="h-4 w-4" />
            </button>

            {/* Fast forward 10 */}
            <button
              type="button"
              onClick={onFastForward}
              className="flex items-center gap-1 px-2 py-1 font-mono text-foreground/80 text-xs transition-colors hover:text-primary"
            >
              <FastForward className="h-4 w-4" />
              <span>10</span>
              <span className="text-[10px]">—</span>
            </button>

            {/* Right bracket */}
            <span className="font-mono text-lg text-primary/50">]</span>
          </div>

          {/* Timestamp */}
          <div className="mt-2 text-center">
            <span className="font-mono text-foreground/80 text-xs uppercase tracking-widest">
              [{status === "playing" ? "PLAY" : "PAUSE"}]
            </span>
            <div className="font-mono text-lg text-primary tracking-wider">
              {currentTime}{" "}
              <span className="text-foreground/80 text-xs">ELAPSED</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}

export function FastForwardOverlay({
  direction = "forward",
  className,
}: {
  direction?: "forward" | "backward";
  className?: string;
}) {
  return (
    <div
      className={cn(
        "flex h-full flex-col items-center justify-center bg-primary/80",
        className
      )}
    >
      {/* Triangle icon */}
      <svg
        aria-hidden="true"
        className="mb-4 h-24 w-24"
        viewBox="0 0 100 100"
        fill="none"
        stroke="currentColor"
        strokeWidth="3"
      >
        {direction === "forward" ? (
          <polygon
            points="25,15 85,50 25,85"
            className="text-white"
            fill="none"
          />
        ) : (
          <polygon
            points="75,15 15,50 75,85"
            className="text-white"
            fill="none"
          />
        )}
      </svg>

      {/* Text */}
      <div className="font-bold font-mono text-3xl text-white uppercase tracking-[0.3em]">
        {direction === "forward" ? "FAST FORWARD" : "REWIND"}
      </div>

      {/* Bottom line decoration */}
      <div className="mt-6 h-1 w-64 bg-white/30" />
    </div>
  );
}

demo.tsx
"use client"

import * as React from "react"
import { VideoPlayer } from "@/components/ui/video-player"

export default function VideoPlayerDemo() {
  const [status, setStatus] = React.useState<"playing" | "paused">("paused")

  return (
    <div className="flex min-h-[540px] w-full items-center justify-center bg-background p-8">
      <div className="w-full max-w-xl">
        <VideoPlayer
          status={status}
          currentTime="00:04:32"
          onPlay={() => setStatus("playing")}
          onPause={() => setStatus("paused")}
          onRewind={() => {}}
          onFastForward={() => {}}
        >
          <div className="flex h-full w-full items-center justify-center bg-gradient-to-br from-primary/20 via-background to-background">
            <span className="font-mono text-sm uppercase tracking-[0.3em] text-primary/70">
              [ SIGNAL FEED ]
            </span>
          </div>
        </VideoPlayer>
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install lucide-react
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
