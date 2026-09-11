<!-- Youtube Video Player · cult-ui · https://www.cult-ui.com/docs/components/youtube-video-player
     license: MIT · category: video
     YouTube video player component with custom controls and smooth animations -->

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
components/ui/youtube-video-player.tsx
"use client"

import { useEffect, useState } from "react"
import { Maximize2, Minimize2, Play } from "lucide-react"
import { AnimatePresence, motion } from "motion/react"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"

interface YouTubePlayerProps {
  videoId: string
  title?: string
  defaultExpanded?: boolean
  customThumbnail?: string

  // Container & Layout
  className?: string
  containerClassName?: string
  expandedClassName?: string

  // Thumbnail & Media
  thumbnailClassName?: string
  thumbnailImageClassName?: string

  // Play Button
  playButtonClassName?: string
  playIconClassName?: string

  // Title
  titleClassName?: string

  // Controls
  controlsClassName?: string
  expandButtonClassName?: string

  // Backdrop
  backdropClassName?: string

  // Player
  playerClassName?: string
}

export function YouTubePlayer({
  videoId,
  title,
  defaultExpanded = false,
  customThumbnail,

  // Styling props
  className,
  containerClassName,
  expandedClassName,
  thumbnailClassName,
  thumbnailImageClassName,
  playButtonClassName,
  playIconClassName,
  titleClassName,
  controlsClassName,
  expandButtonClassName,
  backdropClassName,
  playerClassName,
}: YouTubePlayerProps) {
  const [expanded, setExpanded] = useState(defaultExpanded)
  const [playing, setPlaying] = useState(false)
  const [isHovered, setIsHovered] = useState(false)

  // Extract video ID from full URL if needed
  const extractVideoId = (id: string) => {
    if (id.includes("youtube.com") || id.includes("youtu.be")) {
      try {
        const url = new URL(id)
        if (id.includes("youtube.com")) {
          return url.searchParams.get("v") || ""
        } else {
          return url.pathname.substring(1)
        }
      } catch (error) {
        console.error("Invalid YouTube URL:", error)
        return id
      }
    }
    return id
  }

  const actualVideoId = extractVideoId(videoId)

  const handlePlay = () => {
    setPlaying(true)
  }

  const toggleExpand = () => {
    setExpanded(!expanded)
  }

  // Handle Escape key to minimize when expanded
  useEffect(() => {
    const handleKeyDown = (event: KeyboardEvent) => {
      if (event.key === "Escape" && expanded) {
        setExpanded(false)
      }
    }
    if (expanded) {
      document.addEventListener("keydown", handleKeyDown)
    }
    return () => {
      document.removeEventListener("keydown", handleKeyDown)
    }
  }, [expanded])

  const getThumbnailUrl = () => {
    if (customThumbnail) return customThumbnail
    return actualVideoId
      ? `https://i.ytimg.com/vi/${actualVideoId}/hqdefault.jpg`
      : ""
  }

  return (
    <>
      {/* Main container - always in the document flow */}
      <div
        className={cn(
          "relative",
          expanded ? "invisible" : "visible",
          className
        )}
      >
        <motion.div
          layoutId={`youtube-player-${videoId}`}
          className={cn(
            "overflow-hidden border bg-card text-card-foreground shadow-lg rounded-xl",
            containerClassName
          )}
        >
          <motion.div
            layoutId={`youtube-player-content-${videoId}`}
            className={cn("relative aspect-video bg-muted", playerClassName)}
          >
            {!playing && (
              <>
                <motion.div
                  layoutId={`youtube-player-thumbnail-container-${videoId}`}
                  className={cn(
                    "absolute inset-0 bg-gradient-to-br from-muted to-muted/80",
                    thumbnailClassName
                  )}
                >
                  {getThumbnailUrl() && (
                    <motion.img
                      layoutId={`youtube-player-thumbnail-${videoId}`}
                      src={getThumbnailUrl()}
                      alt={title || "Video thumbnail"}
                      className={cn(
                        "absolute inset-0 h-full w-full object-cover opacity-70",
                        thumbnailImageClassName
                      )}
                    />
                  )}
                </motion.div>

                <motion.div
                  layoutId={`youtube-player-content-overlay-${videoId}`}
                  className="absolute inset-0 flex flex-col items-center justify-center z-10"
                >
                  <Button
                    size="lg"
                    variant="secondary"
                    className={cn(
                      "relative h-16 w-16 rounded-full border border-border/20 bg-background/80 backdrop-blur-sm md:h-20 md:w-20 p-0",
                      "focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 focus-visible:ring-offset-background",
                      playButtonClassName
                    )}
                    onClick={handlePlay}
                    aria-label="Play video"
                  >
                    <Play
                      className={cn(
                        "h-6 w-6 translate-x-[2px] fill-primary text-primary md:h-8 md:w-8",
                        playIconClassName
                      )}
                    />
                  </Button>

                  {title && (
                    <motion.h3
                      layoutId={`youtube-player-title-${videoId}`}
                      className={cn(
                        "mt-4 max-w-xs text-center text-sm font-medium text-secondary/90 md:max-w-md md:text-base",
                        titleClassName
                      )}
                    >
                      {title}
                    </motion.h3>
                  )}
                </motion.div>
              </>
            )}

            {playing && (
              <iframe
                src={`https://www.youtube.com/embed/${actualVideoId}?autoplay=1&rel=0&modestbranding=1&iv_load_policy=3&showinfo=0&controls=1`}
                title={title}
                allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
                allowFullScreen
                className="h-full w-full border-0"
              />
            )}

            {/* Controls Overlay */}
            <YouTubePlayerControls
              videoId={videoId}
              expanded={expanded}
              playing={playing}
              isHovered={isHovered}
              onToggleExpand={toggleExpand}
              controlsClassName={controlsClassName}
              expandButtonClassName={expandButtonClassName}
            />
          </motion.div>
        </motion.div>
      </div>

      {/* Expanded state - fixed position */}
      <AnimatePresence>
        {expanded && (
          <>
            <motion.div
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              exit={{ opacity: 0 }}
              transition={{ duration: 0.2 }}
              className={cn(
                "fixed inset-0 z-40 bg-background/80 backdrop-blur-sm",
                backdropClassName
              )}
              onClick={toggleExpand}
              aria-label="Close expanded video"
            />

            <div className="fixed inset-0 z-50 flex items-center justify-center pointer-events-none">
              <motion.div
                layoutId={`youtube-player-${videoId}`}
                className={cn(
                  "overflow-hidden border bg-card text-card-foreground shadow-xl rounded-lg pointer-events-auto",
                  "w-[90vw] max-w-[1200px] max-h-[90vh] aspect-video",
                  expandedClassName
                )}
              >
                <motion.div
                  layoutId={`youtube-player-content-${videoId}`}
                  className={cn(
                    "relative aspect-video bg-muted",
                    playerClassName
                  )}
                >
                  {!playing && (
                    <>
                      <motion.div
                        layoutId={`youtube-player-thumbnail-container-${videoId}`}
                        className={cn(
                          "absolute inset-0 bg-gradient-to-br from-muted to-muted/80",
                          thumbnailClassName
                        )}
                      >
                        {getThumbnailUrl() && (
                          <motion.img
                            layoutId={`youtube-player-thumbnail-${videoId}`}
                            src={getThumbnailUrl()}
                            alt={title || "Video thumbnail"}
                            className={cn(
                              "absolute inset-0 h-full w-full object-cover opacity-70",
                              thumbnailImageClassName
                            )}
                          />
                        )}
                      </motion.div>

                      <motion.div
                        layoutId={`youtube-player-content-overlay-${videoId}`}
                        className="absolute inset-0 flex flex-col items-center justify-center z-10"
                      >
                        <Button
                          size="lg"
                          variant="secondary"
                          className={cn(
                            "relative h-16 w-16 rounded-full border border-border/20 bg-background/80 backdrop-blur-sm md:h-20 md:w-20 p-0",
                            "focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 focus-visible:ring-offset-background",
                            playButtonClassName
                          )}
                          onClick={handlePlay}
                          aria-label="Play video"
                        >
                          <Play
                            className={cn(
                              "h-6 w-6 translate-x-[2px] fill-primary text-primary md:h-8 md:w-8",
                              playIconClassName
                            )}
                          />
                        </Button>

                        {title && (
                          <motion.h3
                            layoutId={`youtube-player-title-${videoId}`}
                            className={cn(
                              "mt-4 max-w-xs text-center text-sm font-medium text-foreground/90 md:max-w-md md:text-base",
                              titleClassName
                            )}
                          >
                            {title}
                          </motion.h3>
                        )}
                      </motion.div>
                    </>
                  )}

                  {playing && (
                    <iframe
                      src={`https://www.youtube.com/embed/${actualVideoId}?autoplay=1&rel=0&modestbranding=1&iv_load_policy=3&showinfo=0&controls=1`}
                      title={title}
                      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
                      allowFullScreen
                      className="h-full w-full border-0"
                    />
                  )}

                  {/* Controls Overlay */}
                  <YouTubePlayerControls
                    videoId={videoId}
                    expanded={expanded}
                    playing={playing}
                    isHovered={isHovered}
                    onToggleExpand={toggleExpand}
                    controlsClassName={controlsClassName}
                    expandButtonClassName={expandButtonClassName}
                  />
                </motion.div>
              </motion.div>
            </div>
          </>
        )}
      </AnimatePresence>
    </>
  )
}

// Controls Component
interface YouTubePlayerControlsProps {
  videoId: string
  expanded: boolean
  playing: boolean
  isHovered: boolean
  onToggleExpand: () => void
  controlsClassName?: string
  expandButtonClassName?: string
}

function YouTubePlayerControls({
  videoId,
  expanded,
  playing,
  isHovered,
  onToggleExpand,
  controlsClassName,
  expandButtonClassName,
}: YouTubePlayerControlsProps) {
  const shouldShow = !playing || isHovered || expanded

  return (
    <AnimatePresence>
      {shouldShow && (
        <motion.div
          layoutId={`youtube-player-controls-${videoId}`}
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          exit={{ opacity: 0 }}
          transition={{ duration: 0.2 }}
          className={cn("absolute right-2 top-2 z-20", controlsClassName)}
        >
          <motion.div whileHover={{ scale: 1.05 }} whileTap={{ scale: 0.95 }}>
            <Button
              variant="secondary"
              size="icon"
              onClick={onToggleExpand}
              className={cn(
                "h-8 w-8 rounded-full bg-background/40 backdrop-blur-sm hover:bg-background/60 focus-visible:ring-ring/50 md:h-9 md:w-9",
                expandButtonClassName
              )}
              aria-label={expanded ? "Minimize video" : "Maximize video"}
            >
              <motion.div
                animate={{ rotate: expanded ? 180 : 0 }}
                transition={{ type: "spring", stiffness: 300, damping: 20 }}
              >
                {expanded ? (
                  <Minimize2 className="h-4 w-4 md:h-5 md:w-5" />
                ) : (
                  <Maximize2 className="h-4 w-4 md:h-5 md:w-5" />
                )}
              </motion.div>
            </Button>
          </motion.div>
        </motion.div>
      )}
    </AnimatePresence>
  )
}

// Export sub-components for advanced customization
export { YouTubePlayerControls }

demo.tsx
"use client"

import { YouTubePlayer } from "@/registry/default/ui/youtube-video-player"

export default function YouTubeVideoPlayerDemo() {
  return (
    <div className="space-y-12 p-8">
      <div className="space-y-4">
        <h1 className="text-3xl font-bold">YouTube Video Player Examples</h1>
        <p className="text-muted-foreground">
          A collection of YouTube video player examples showcasing different
          configurations and styling options.
        </p>
      </div>

      {/* Custom Thumbnail */}
      <section className="space-y-4">
        <div>
          <h2 className="text-2xl font-semibold mb-2">Custom Thumbnail</h2>
          <p className="text-muted-foreground">
            Player with a custom thumbnail image instead of the default YouTube
            thumbnail.
          </p>
        </div>
        <div className="max-w-2xl">
          <YouTubePlayer
            videoId="jNQXAC9IVRw"
            title="Me at the zoo - First YouTube Video"
            customThumbnail="https://images.unsplash.com/photo-1611162617474-5b21e879e113?w=800&h=450&fit=crop&crop=center"
          />
        </div>
      </section>

      {/* Custom Styling */}
      <section className="space-y-4">
        <div>
          <h2 className="text-2xl font-semibold mb-2">Custom Styling</h2>
          <p className="text-muted-foreground">
            Player with custom styling classes for different elements.
          </p>
        </div>
        <div className="max-w-2xl">
          <YouTubePlayer
            videoId="kJQP7kiw5Fk"
            title="Despacito - Luis Fonsi ft. Daddy Yankee"
            containerClassName="border-2 border-primary rounded-2xl shadow-2xl"
            thumbnailImageClassName="opacity-90 saturate-150"
            playButtonClassName="bg-primary/20 border-border/20 hover:bg-primary/30"
            playIconClassName="text-secondary fill-secondary"
            titleClassName="text-secondary font-bold"
            controlsClassName="right-4 top-4"
            expandButtonClassName="bg-secondary/20 hover:bg-secondary/30 border-secondary text-secondary"
          />
        </div>
      </section>

      {/* Multiple Players Grid */}
      <section className="space-y-4">
        <div>
          <h2 className="text-2xl font-semibold mb-2">Multiple Players</h2>
          <p className="text-muted-foreground">
            A grid of multiple video players with different content.
          </p>
        </div>
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          <YouTubePlayer
            videoId="9bZkp7q19f0"
            title="PSY - GANGNAM STYLE"
            className="w-full"
          />
          <YouTubePlayer
            videoId="fJ9rUzIMcZQ"
            title="Queen – Bohemian Rhapsody"
            className="w-full"
          />
          <YouTubePlayer
            videoId="L_jWHffIx5E"
            title="Smash Mouth - All Star"
            className="w-full"
          />
          <YouTubePlayer
            videoId="hTWKbfoikeg"
            title="Nirvana - Smells Like Teen Spirit"
            className="w-full"
          />
          <YouTubePlayer
            videoId="djV11Xbc914"
            title="a-ha - Take On Me"
            className="w-full"
          />
          <YouTubePlayer
            videoId="ZbZSe6N_BXs"
            title="Happy - Pharrell Williams"
            className="w-full"
          />
        </div>
      </section>

      {/* Different Aspect Ratios */}
      <section className="space-y-4">
        <div>
          <h2 className="text-2xl font-semibold mb-2">Different Sizes</h2>
          <p className="text-muted-foreground">
            Players in different container sizes to show responsive behavior.
          </p>
        </div>
        <div className="space-y-8">
          {/* Small */}
          <div>
            <h3 className="text-lg font-medium mb-3">Small (300px)</h3>
            <div className="w-[300px]">
              <YouTubePlayer
                videoId="2yJgwwDcgV8"
                title="Nyan Cat [original]"
              />
            </div>
          </div>

          {/* Medium */}
          <div>
            <h3 className="text-lg font-medium mb-3">Medium (500px)</h3>
            <div className="w-[500px]">
              <YouTubePlayer videoId="oHg5SJYRHA0" title="RickRoll'D" />
            </div>
          </div>

          {/* Large */}
          <div>
            <h3 className="text-lg font-medium mb-3">Large (800px)</h3>
            <div className="w-[800px]">
              <YouTubePlayer videoId="y6120QOlsfU" title="Darude - Sandstorm" />
            </div>
          </div>
        </div>
      </section>

      {/* URL Formats */}
      <section className="space-y-4">
        <div>
          <h2 className="text-2xl font-semibold mb-2">Different URL Formats</h2>
          <p className="text-muted-foreground">
            The player can handle different YouTube URL formats automatically.
          </p>
        </div>
        <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
          {/* Regular video ID */}
          <div>
            <h3 className="text-sm font-medium mb-2">
              Video ID: "dQw4w9WgXcQ"
            </h3>
            <YouTubePlayer videoId="dQw4w9WgXcQ" title="Using Video ID" />
          </div>

          {/* Full YouTube URL */}
          <div>
            <h3 className="text-sm font-medium mb-2">
              Full URL: "https://www.youtube.com/watch?v=dQw4w9WgXcQ"
            </h3>
            <YouTubePlayer
              videoId="https://www.youtube.com/watch?v=dQw4w9WgXcQ"
              title="Using Full YouTube URL"
            />
          </div>

          {/* Short URL */}
          <div>
            <h3 className="text-sm font-medium mb-2">
              Short URL: "https://youtu.be/dQw4w9WgXcQ"
            </h3>
            <YouTubePlayer
              videoId="https://youtu.be/dQw4w9WgXcQ"
              title="Using Short YouTube URL"
            />
          </div>
        </div>
      </section>

      {/* Feature Highlights */}
      <section className="space-y-4">
        <div>
          <h2 className="text-2xl font-semibold mb-2">Features</h2>
          <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div className="space-y-2">
              <h3 className="text-lg font-medium">✨ Key Features</h3>
              <ul className="text-sm text-muted-foreground space-y-1">
                <li>• Expandable full-screen mode</li>
                <li>• Custom thumbnails support</li>
                <li>• Smooth animations with Framer Motion</li>
                <li>• Keyboard shortcuts (ESC to close)</li>
                <li>• Responsive design</li>
                <li>• Accessible controls</li>
                <li>• Multiple URL format support</li>
              </ul>
            </div>
            <div className="space-y-2">
              <h3 className="text-lg font-medium">🎨 Customization</h3>
              <ul className="text-sm text-muted-foreground space-y-1">
                <li>• Fully customizable styling</li>
                <li>• Custom play button designs</li>
                <li>• Thumbnail overlay effects</li>
                <li>• Control button positioning</li>
                <li>• Container and backdrop styling</li>
                <li>• Title and text customization</li>
              </ul>
            </div>
          </div>
        </div>
      </section>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install motion
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
