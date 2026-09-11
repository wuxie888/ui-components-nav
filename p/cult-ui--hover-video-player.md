<!-- Hover Video Player · cult-ui · https://www.cult-ui.com/docs/components/hover-video-player
     license: MIT · category: card
     Video player component that plays on hover with smooth animations -->

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
components/ui/hover-video-player.tsx
"use client"

/**
 * HoverVideoPlayer Component
 *
 * A React component that plays video on hover/touch with advanced features like:
 * - Lazy loading and intersection observer support
 * - Mobile touch support
 * - Picture-in-Picture
 * - Custom overlay support
 * - Thumbnail support
 * - Playback controls
 *
 * @example
 * ```tsx
 * <HoverVideoPlayer
 *   videoSrc="/path/to/video.mp4"
 *   thumbnailSrc="/path/to/thumbnail.jpg"
 *   pausedOverlay={<PlayIcon />}
 *   loadingOverlay={<Spinner />}
 *   enableControls
 * />
 * ```
 */
import React, {
  ReactNode,
  createContext,
  useCallback,
  useContext,
  useEffect,
  useMemo,
  useRef,
  useState,
} from "react"
import Image from "next/image"
import { Maximize, Minimize, Pause, Play, Volume2, VolumeX } from "lucide-react"
import { AnimatePresence, motion, useReducedMotion } from "motion/react"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"
import { Slider } from "@/components/ui/slider"

const hoverTransition = {
  duration: 0.18,
  ease: [0.2, 0, 0, 1],
} as const

const hoverSpring = {
  type: "spring",
  duration: 0.3,
  bounce: 0,
} as const

// Types
interface VideoPlayerState {
  isHovering: boolean
  isPlaying: boolean
  isLoading: boolean
  progress: number
  muted: boolean
  volume: number
  isPiP: boolean
  isMobile: boolean
  controlsVisible: boolean
  showThumbnail: boolean
  isInView: boolean
}

interface HoverVideoPlayerProps {
  videoSrc: string
  thumbnailSrc?: string
  hoverOverlay?: React.ReactNode
  pausedOverlay?: React.ReactNode
  loadingOverlay?: React.ReactNode
  playbackStartDelay?: number
  restartOnPaused?: boolean
  unloadVideoOnPaused?: boolean
  playbackRangeStart?: number
  playbackRangeEnd?: number
  muted?: boolean
  loop?: boolean
  preload?: "auto" | "metadata" | "none"
  className?: string
  style?: React.CSSProperties
  onHoverStart?: () => void
  onHoverEnd?: () => void
  enableControls?: boolean
  cropTop?: number
  cropBottom?: number
  isVimeo?: boolean
}

interface HoverVideoPlayerContextType {
  isPlaying: boolean
  isHovering: boolean
  isLoading: boolean
  progress: number
  volume: number
  muted: boolean
  isPiP: boolean
  isMobile: boolean
  controlsVisible: boolean
  videoRef: React.RefObject<HTMLVideoElement | null>
  togglePlay: () => void
  toggleMute: () => void
  togglePiP: () => void
  setVolume: (value: number) => void
  setProgress: (value: number) => void
  seekProgress: (value: number) => void
  setSeekProgressHandler: (
    handler: ((value: number) => void | Promise<void>) | null
  ) => void
  setVideoLoading: (isLoading: boolean) => void
  setVideoPlaying: (isPlaying: boolean) => void
  cropTop: number
  cropBottom: number
  thumbnailSrc?: string
}

interface VimeoPlayer {
  destroy: () => void
  ready: () => Promise<void>
  setVolume: (volume: number) => Promise<void>
  getDuration: () => Promise<number>
  setCurrentTime: (seconds: number) => Promise<number>
  play: () => Promise<void>
  pause: () => Promise<void>
  on: (event: string, callback: (...args: any[]) => void) => void
  off: (event: string, callback: (...args: any[]) => void) => void
}

interface VimeoConstructor {
  Player: {
    new (element: HTMLElement, options: any): VimeoPlayer
  }
}

// Context
const HoverVideoPlayerContext =
  createContext<HoverVideoPlayerContextType | null>(null)

/**
 * Custom hook to access HoverVideoPlayer context
 * @throws {Error} If used outside of HoverVideoPlayerContext
 */
const useHoverVideoPlayer = () => {
  const context = useContext(HoverVideoPlayerContext)
  if (!context) {
    throw new Error(
      "useHoverVideoPlayer must be used within a HoverVideoPlayer"
    )
  }
  return context
}

// Add this helper function before the HoverVideoPlayer component
function debounce<T extends (...args: any[]) => any>(
  func: T,
  wait: number
): {
  (...args: Parameters<T>): void
  cancel: () => void
} {
  let timeoutId: NodeJS.Timeout | null = null

  const debouncedFn = (...args: Parameters<T>) => {
    if (timeoutId) clearTimeout(timeoutId)
    timeoutId = setTimeout(() => func(...args), wait)
  }

  debouncedFn.cancel = () => {
    if (timeoutId) {
      clearTimeout(timeoutId)
      timeoutId = null
    }
  }

  return debouncedFn
}

// Add this helper function
function isVimeoUrl(url: string): boolean {
  return url.includes("player.vimeo.com/video/") || url.includes("vimeo.com/")
}

// Add this helper function
function loadVimeoSDK(): Promise<VimeoConstructor> {
  return new Promise((resolve, reject) => {
    if ((window as any).Vimeo) {
      resolve((window as any).Vimeo)
      return
    }

    const script = document.createElement("script")
    script.src = "https://player.vimeo.com/api/player.js"
    script.async = true
    script.onload = () => resolve((window as any).Vimeo)
    script.onerror = reject
    document.body.appendChild(script)
  })
}

// Main Component
const HoverVideoPlayer: React.FC<HoverVideoPlayerProps> = ({
  videoSrc,
  thumbnailSrc,
  hoverOverlay,
  pausedOverlay,
  loadingOverlay,
  playbackStartDelay = 0,
  restartOnPaused = false,
  unloadVideoOnPaused = false,
  playbackRangeStart,
  playbackRangeEnd,
  muted: initialMuted = false,
  loop = true,
  preload = "metadata",
  className,
  style,
  onHoverStart,
  onHoverEnd,
  enableControls = false,
  cropTop = 0,
  cropBottom = 0,
  isVimeo = false,
}) => {
  const isVimeoVideo = isVimeo || isVimeoUrl(videoSrc)

  // Refs for DOM elements and timing
  const containerRef = useRef<HTMLDivElement>(null)
  const videoRef = useRef<HTMLVideoElement>(null)
  const playbackTimeoutRef = useRef<NodeJS.Timeout | null>(null)
  const lastPlayAttemptRef = useRef<number>(0)
  const seekProgressHandlerRef = useRef<
    ((value: number) => void | Promise<void>) | null
  >(null)

  // Consolidated state management
  const [state, setState] = useState<VideoPlayerState>({
    isHovering: false,
    isPlaying: false,
    isLoading: false,
    progress: 0,
    muted: initialMuted,
    volume: 1,
    isPiP: false,
    isMobile: false,
    controlsVisible: false,
    showThumbnail: true,
    isInView: false,
  })

  // Mobile detection
  const checkMobile = useMemo(
    () =>
      debounce(() => {
        setState((prev) => ({ ...prev, isMobile: window.innerWidth <= 768 }))
      }, 200),
    []
  )

  // Event Handlers
  const handleTouchStart = useCallback(() => {
    if (state.isMobile) {
      setState((prev) => ({ ...prev, controlsVisible: !prev.controlsVisible }))
    }
  }, [state.isMobile])

  const handleHoverStart = useCallback(() => {
    if (!state.isMobile) {
      console.log("Hover start")
      setState((prev) => ({ ...prev, isHovering: true }))
      onHoverStart?.()
    }
  }, [state.isMobile, onHoverStart])

  const handleHoverEnd = useCallback(() => {
    if (!state.isMobile) {
      console.log("Hover end")
      setState((prev) => ({ ...prev, isHovering: false }))
      onHoverEnd?.()
    }
  }, [state.isMobile, onHoverEnd])

  // Video Playback Controls
  const playVideo = useCallback(() => {
    if (!videoRef.current || !state.isInView) {
      console.log("PlayVideo blocked:", {
        hasVideo: !!videoRef.current,
        isInView: state.isInView,
      })
      return
    }

    const video = videoRef.current
    console.log("Attempting to play video:", {
      readyState: video.readyState,
      paused: video.paused,
      currentSrc: video.currentSrc,
    })

    // Reset loading state when attempting to play
    setState((prev) => ({ ...prev, isLoading: true }))

    // Ensure video is ready to play
    const attemptPlay = () => {
      video
        .play()
        .then(() => {
          console.log("Video play success")
          setState((prev) => ({
            ...prev,
            isLoading: false,
            isPlaying: true,
            showThumbnail: false,
          }))
        })
        .catch((error) => {
          console.log("Video play error:", error.name)
          if (error.name === "NotAllowedError") {
            // User interaction required - show thumbnail
            setState((prev) => ({
              ...prev,
              isLoading: false,
              showThumbnail: true,
            }))
          } else if (error.name !== "AbortError") {
            console.error("HoverVideoPlayer: Playback error:", error)
            setState((prev) => ({
              ...prev,
              isLoading: false,
              showThumbnail: true,
            }))
          }
        })
    }

    if (video.readyState >= 3) {
      console.log("Video ready, attempting immediate play")
      attemptPlay()
    } else {
      console.log("Video not ready, waiting for canplay event")
      const handleCanPlay = () => {
        console.log("Canplay event fired")
        video.removeEventListener("canplay", handleCanPlay)
        attemptPlay()
      }
      video.addEventListener("canplay", handleCanPlay)
    }
  }, [state.isInView])

  const pauseVideo = useCallback(() => {
    if (!videoRef.current) return

    videoRef.current.pause()
    setState((prev) => ({
      ...prev,
      isPlaying: false,
      showThumbnail: true,
    }))

    if (restartOnPaused) {
      videoRef.current.currentTime = playbackRangeStart || 0
    }
  }, [restartOnPaused, playbackRangeStart])

  // Effects

  // Mobile detection effect
  useEffect(() => {
    checkMobile()
    window.addEventListener("resize", checkMobile)
    return () => {
      window.removeEventListener("resize", checkMobile)
      checkMobile.cancel()
    }
  }, [checkMobile])

  // Intersection Observer effect
  useEffect(() => {
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          const isNowInView = entry.isIntersecting
          setState((prev) => ({ ...prev, isInView: isNowInView }))

          if (!isNowInView) {
            pauseVideo()
          }
        })
      },
      {
        root: null,
        rootMargin: "50px",
        threshold: 0.1,
      }
    )

    if (containerRef.current) {
      observer.observe(containerRef.current)
    }

    return () => observer.disconnect()
  }, [pauseVideo])

  // Video event handlers effect
  useEffect(() => {
    const video = videoRef.current
    if (!video || !state.isInView) return

    const handlers = {
      loadstart: () => {
        console.log("Video loadstart")
        setState((prev) => ({ ...prev, isLoading: true }))
      },
      loadeddata: () => {
        console.log("Video loadeddata", {
          readyState: video.readyState,
          duration: video.duration,
          paused: video.paused,
          isHovering: state.isHovering,
          controlsVisible: state.controlsVisible,
        })
        setState((prev) => ({ ...prev, isLoading: false }))
        // Only attempt to play if we're hovering or controls are visible
        if (state.isHovering || state.controlsVisible) {
          console.log("Video loaded and hover/controls active, attempting play")
          playVideo()
        }
      },
      timeupdate: () => {
        if (!Number.isFinite(video.duration) || video.duration <= 0) return

        const nextProgress = (video.currentTime / video.duration) * 100
        setState((prev) => ({ ...prev, progress: nextProgress }))
      },
      canplay: () => {
        console.log("Video canplay event")
      },
      playing: () => {
        console.log("Video playing")
        setState((prev) => ({
          ...prev,
          isLoading: false,
          isPlaying: true,
          showThumbnail: false,
        }))
      },
      pause: () => {
        console.log("Video paused")
        setState((prev) => ({
          ...prev,
          isPlaying: false,
          showThumbnail: true,
        }))
      },
      error: (e: Event) => {
        const videoError = (e.target as HTMLVideoElement).error
        console.error("Video error:", {
          code: videoError?.code,
          message: videoError?.message,
          currentSrc: video.currentSrc,
        })
        setState((prev) => ({
          ...prev,
          isLoading: false,
          showThumbnail: true,
        }))
      },
    }

    // Add event listeners
    Object.entries(handlers).forEach(([event, handler]) => {
      video.addEventListener(event, handler)
    })

    // Set initial source and load the video
    if (!video.src) {
      console.log("Setting video source:", videoSrc)
      video.src = videoSrc
      video.load()
    }

    return () => {
      Object.entries(handlers).forEach(([event, handler]) => {
        video.removeEventListener(event, handler)
      })
    }
  }, [
    state.isInView,
    state.isHovering,
    state.controlsVisible,
    playVideo,
    videoSrc,
  ])

  // Playback control effect
  useEffect(() => {
    console.log("Playback control effect:", {
      isInView: state.isInView,
      isMobile: state.isMobile,
      controlsVisible: state.controlsVisible,
      isHovering: state.isHovering,
    })

    if (!state.isInView) {
      console.log("Not in view, skipping playback")
      return
    }

    if (isVimeoVideo) {
      return
    }

    let playbackTimeout: NodeJS.Timeout | undefined

    if (state.isMobile) {
      if (state.controlsVisible) {
        console.log("Mobile controls visible, playing")
        playVideo()
      } else {
        console.log("Mobile controls hidden, pausing")
        pauseVideo()
      }
    } else if (state.isHovering) {
      console.log(
        "Hovering, scheduling playback with delay:",
        playbackStartDelay
      )
      playbackTimeout = setTimeout(() => {
        console.log("Playback delay completed, attempting to play")
        playVideo()
      }, playbackStartDelay)
    } else {
      console.log("Not hovering, pausing")
      pauseVideo()
    }

    return () => {
      if (playbackTimeout) {
        console.log("Clearing playback timeout")
        clearTimeout(playbackTimeout)
      }
    }
  }, [
    state.isInView,
    state.isMobile,
    state.controlsVisible,
    state.isHovering,
    playVideo,
    pauseVideo,
    playbackStartDelay,
    isVimeoVideo,
  ])

  // Volume effect
  useEffect(() => {
    const video = videoRef.current
    if (!video) return

    video.muted = state.muted
    video.volume = state.volume
  }, [state.muted, state.volume])

  // Control handlers
  const togglePlayPause = useCallback(() => {
    if (state.isPlaying) {
      pauseVideo()
    } else {
      playVideo()
    }
  }, [state.isPlaying, pauseVideo, playVideo])

  const toggleMute = useCallback(() => {
    setState((prev) => ({ ...prev, muted: !prev.muted }))
  }, [])

  const togglePiP = useCallback(async () => {
    if (!document.pictureInPictureEnabled) return

    try {
      if (document.pictureInPictureElement) {
        await document.exitPictureInPicture()
        setState((prev) => ({ ...prev, isPiP: false }))
      } else if (videoRef.current) {
        await videoRef.current.requestPictureInPicture()
        setState((prev) => ({ ...prev, isPiP: true }))
      }
    } catch (error) {
      console.error("PiP error:", error)
    }
  }, [])

  const setSeekProgressHandler = useCallback(
    (handler: ((value: number) => void | Promise<void>) | null) => {
      seekProgressHandlerRef.current = handler
    },
    []
  )

  const seekProgress = useCallback((value: number) => {
    const nextProgress = Math.min(100, Math.max(0, value))
    setState((prev) => ({ ...prev, progress: nextProgress }))

    if (seekProgressHandlerRef.current) {
      void seekProgressHandlerRef.current(nextProgress)
      return
    }

    const video = videoRef.current
    if (!video || !Number.isFinite(video.duration) || video.duration <= 0) {
      return
    }

    video.currentTime = (nextProgress / 100) * video.duration
  }, [])

  const setProgress = useCallback((value: number) => {
    setState((prev) => ({ ...prev, progress: value }))
  }, [])

  const setVolume = useCallback((value: number) => {
    setState((prev) => ({ ...prev, volume: value }))
  }, [])

  const setVideoLoading = useCallback((isLoading: boolean) => {
    setState((prev) => ({ ...prev, isLoading }))
  }, [])

  const setVideoPlaying = useCallback((isPlaying: boolean) => {
    setState((prev) => ({
      ...prev,
      isPlaying,
      showThumbnail: !isPlaying,
    }))
  }, [])

  // Context value
  const contextValue = useMemo<HoverVideoPlayerContextType>(
    () => ({
      isPlaying: state.isPlaying,
      isHovering: state.isHovering,
      isLoading: state.isLoading,
      progress: state.progress,
      volume: state.volume,
      muted: state.muted,
      isPiP: state.isPiP,
      isMobile: state.isMobile,
      controlsVisible: state.controlsVisible,
      videoRef,
      togglePlay: togglePlayPause,
      toggleMute,
      togglePiP,
      setVolume,
      setProgress,
      seekProgress,
      setSeekProgressHandler,
      setVideoLoading,
      setVideoPlaying,
      cropTop,
      cropBottom,
      thumbnailSrc,
    }),
    [
      state,
      togglePlayPause,
      toggleMute,
      togglePiP,
      setVolume,
      setProgress,
      seekProgress,
      setSeekProgressHandler,
      setVideoLoading,
      setVideoPlaying,
      cropTop,
      cropBottom,
      thumbnailSrc,
    ]
  )

  // Render
  return (
    <HoverVideoPlayerContext.Provider value={contextValue}>
      <motion.div
        ref={containerRef}
        className={cn(
          "relative overflow-hidden group cursor-pointer",
          className
        )}
        style={{
          ...style,
          paddingTop: `${cropTop}%`,
          paddingBottom: `${cropBottom}%`,
        }}
        onHoverStart={handleHoverStart}
        onHoverEnd={handleHoverEnd}
        onTouchStart={handleTouchStart}
      >
        {/* Video Element */}
        {state.isInView && (
          <HoverVideoPlayerVideo
            src={videoSrc}
            unloadVideoOnPaused={unloadVideoOnPaused}
            loop={loop}
            preload={preload}
          />
        )}

        {/* Thumbnail */}
        {thumbnailSrc && <HoverVideoPlayerThumbnail src={thumbnailSrc} />}

        {/* Overlays */}
        {pausedOverlay && (
          <HoverVideoPlayerPausedOverlay>
            {pausedOverlay}
          </HoverVideoPlayerPausedOverlay>
        )}
        {loadingOverlay && (
          <HoverVideoPlayerLoadingOverlay>
            {loadingOverlay}
          </HoverVideoPlayerLoadingOverlay>
        )}
        {hoverOverlay && !state.isMobile && (
          <HoverVideoPlayerHoverOverlay>
            {hoverOverlay}
          </HoverVideoPlayerHoverOverlay>
        )}

        {/* Controls */}
        {enableControls && (
          <HoverVideoPlayerControls>
            <div className="flex items-center space-x-2">
              <HoverVideoPlayerPlayPauseButton />
              <HoverVideoPlayerVolumeControl />
              <HoverVideoPlayerPiPButton />
            </div>
            <HoverVideoPlayerProgressBar />
          </HoverVideoPlayerControls>
        )}
      </motion.div>
    </HoverVideoPlayerContext.Provider>
  )
}

/**
 * Video component that handles the actual video element
 */
const HoverVideoPlayerVideo: React.FC<{
  src: string
  unloadVideoOnPaused: boolean
  loop: boolean
  preload: string
}> = ({ src, loop, preload }) => {
  const {
    videoRef,
    muted,
    cropTop,
    cropBottom,
    isHovering,
    isMobile,
    controlsVisible,
    setProgress,
    setSeekProgressHandler,
    setVideoLoading,
    setVideoPlaying,
  } = useHoverVideoPlayer()
  const isVimeoVideo = isVimeoUrl(src)
  const containerRef = useRef<HTMLDivElement>(null)
  const playerRef = useRef<VimeoPlayer | null>(null)
  const shouldPlayVimeo = isMobile ? controlsVisible : isHovering
  const shouldPlayVimeoRef = useRef(shouldPlayVimeo)

  useEffect(() => {
    shouldPlayVimeoRef.current = shouldPlayVimeo
  }, [shouldPlayVimeo])

  useEffect(() => {
    if (!isVimeoVideo || !containerRef.current) return

    const videoId = src.split("/").pop() || ""
    let player: VimeoPlayer | null = null
    setVideoLoading(true)

    loadVimeoSDK()
      .then((Vimeo) => {
        if (!containerRef.current) return

        player = new Vimeo.Player(containerRef.current, {
          id: videoId,
          autopause: false,
          muted,
          loop,
          responsive: true,
          controls: false,
          autoplay: false,
          volume: 1,
        })

        playerRef.current = player
        setSeekProgressHandler(async (value) => {
          const duration = await player?.getDuration()
          if (!duration || !Number.isFinite(duration)) return

          await player?.setCurrentTime((value / 100) * duration)
        })

        player.ready().then(() => {
          console.log("Vimeo video loaded")
          setVideoLoading(false)

          if (shouldPlayVimeoRef.current) {
            player
              ?.play()
              .then(() => setVideoPlaying(true))
              .catch((error) => {
                setVideoPlaying(false)
                console.error("Vimeo play error:", error)
              })
          }
        })

        player.on("play", () => {
          console.log("Vimeo video playing")
          setVideoLoading(false)
          setVideoPlaying(true)
        })
        player.on("pause", () => {
          console.log("Vimeo video paused")
          setVideoPlaying(false)
        })
        player.on("timeupdate", (data) => {
          if (!data.duration || !Number.isFinite(data.duration)) return

          setProgress((data.seconds / data.duration) * 100)
        })
        player.on("error", (err) => {
          setVideoLoading(false)
          setVideoPlaying(false)
          console.error("Vimeo player error:", err)
        })
      })
      .catch((error) => {
        setVideoLoading(false)
        console.error("Failed to load Vimeo SDK:", error)
      })

    return () => {
      if (player) {
        player.destroy()
        playerRef.current = null
      }
      setSeekProgressHandler(null)
    }
  }, [
    isVimeoVideo,
    src,
    muted,
    loop,
    setProgress,
    setSeekProgressHandler,
    setVideoLoading,
    setVideoPlaying,
  ])

  useEffect(() => {
    if (!isVimeoVideo) return

    const player = playerRef.current
    if (!player) return

    if (shouldPlayVimeo) {
      setVideoLoading(true)
      player
        .play()
        .then(() => {
          setVideoLoading(false)
          setVideoPlaying(true)
        })
        .catch((error) => {
          setVideoLoading(false)
          setVideoPlaying(false)
          console.error("Vimeo play error:", error)
        })
    } else {
      player
        .pause()
        .then(() => setVideoPlaying(false))
        .catch((error) => {
          console.error("Vimeo pause error:", error)
        })
    }
  }, [isVimeoVideo, shouldPlayVimeo, setVideoLoading, setVideoPlaying])

  if (isVimeoVideo) {
    return <div ref={containerRef} className="absolute inset-0" />
  }

  return (
    <video
      ref={videoRef}
      className={cn("absolute inset-0 w-full h-full object-cover")}
      style={{
        top: `-${cropTop}%`,
        bottom: `-${cropBottom}%`,
        height: `calc(100% + ${cropTop + cropBottom}%)`,
      }}
      muted={muted}
      loop={loop}
      preload={preload}
      playsInline
      crossOrigin="anonymous"
      aria-label="Video player"
    />
  )
}

/**
 * Thumbnail component shown when video is not playing
 */
const HoverVideoPlayerThumbnail: React.FC<{ src: string }> = ({ src }) => {
  const { cropTop, cropBottom, isLoading, isPlaying } = useHoverVideoPlayer()
  const shouldReduceMotion = useReducedMotion()
  const shouldShowThumbnail = !isPlaying || isLoading

  return (
    <motion.div
      className={cn(
        "absolute inset-0 z-1 w-full h-full pointer-events-none transform-gpu"
      )}
      style={{
        top: `-${cropTop}%`,
        bottom: `-${cropBottom}%`,
        height: `calc(100% + ${cropTop + cropBottom}%)`,
      }}
      animate={{
        opacity: shouldShowThumbnail ? 1 : 0,
        scale: shouldShowThumbnail ? 1 : 1.01,
      }}
      initial={false}
      transition={shouldReduceMotion ? { duration: 0 } : hoverTransition}
    >
      <Image
        src={src}
        alt="Video thumbnail"
        fill
        sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
        className="object-cover"
        priority
      />
    </motion.div>
  )
}

/**
 * Controls overlay with play/pause, volume, and progress bar
 */
const HoverVideoPlayerControls: React.FC<{ children: React.ReactNode }> = ({
  children,
}) => {
  const { isHovering, isMobile, controlsVisible } = useHoverVideoPlayer()
  const shouldReduceMotion = useReducedMotion()

  const shouldShowControls = isMobile ? controlsVisible : isHovering

  return (
    <AnimatePresence initial={false}>
      {shouldShowControls && (
        <motion.div
          className="absolute bottom-0 left-0 right-0 z-40 p-4 bg-linear-to-t from-black to-transparent"
          initial={
            shouldReduceMotion ? { opacity: 1, y: 0 } : { opacity: 0, y: 12 }
          }
          animate={{ opacity: 1, y: 0 }}
          exit={
            shouldReduceMotion ? { opacity: 0, y: 0 } : { opacity: 0, y: 8 }
          }
          transition={shouldReduceMotion ? { duration: 0 } : hoverSpring}
        >
          {children}
        </motion.div>
      )}
    </AnimatePresence>
  )
}

/**
 * Overlay shown when hovering over the video
 */
const HoverVideoPlayerHoverOverlay: React.FC<{ children: React.ReactNode }> = ({
  children,
}) => {
  const { isHovering, isLoading } = useHoverVideoPlayer()
  const shouldReduceMotion = useReducedMotion()

  return (
    <AnimatePresence initial={false}>
      {isHovering && !isLoading && (
        <motion.div
          className="absolute inset-0 z-30 flex items-center justify-center"
          initial={shouldReduceMotion ? { opacity: 1 } : { opacity: 0 }}
          animate={{ opacity: 1 }}
          exit={{ opacity: 0 }}
          transition={shouldReduceMotion ? { duration: 0 } : hoverTransition}
        >
          {children}
        </motion.div>
      )}
    </AnimatePresence>
  )
}

/**
 * Overlay shown when video is paused
 */
const HoverVideoPlayerPausedOverlay: React.FC<{
  children: React.ReactNode
}> = ({ children }) => {
  const { isPlaying, isLoading, isHovering } = useHoverVideoPlayer()
  const shouldReduceMotion = useReducedMotion()

  return (
    <AnimatePresence initial={false}>
      {!isPlaying && !isLoading && !isHovering && (
        <motion.div
          className="absolute inset-0 z-10 flex items-center justify-center bg-black bg-opacity-50"
          initial={shouldReduceMotion ? { opacity: 1 } : { opacity: 0 }}
          animate={{ opacity: 1 }}
          exit={{ opacity: 0 }}
          transition={shouldReduceMotion ? { duration: 0 } : hoverTransition}
        >
          {children}
        </motion.div>
      )}
    </AnimatePresence>
  )
}

/**
 * Overlay shown while video is loading
 */
const HoverVideoPlayerLoadingOverlay: React.FC<{
  children: React.ReactNode
}> = ({ children }) => {
  const { isLoading } = useHoverVideoPlayer()
  const shouldReduceMotion = useReducedMotion()

  return (
    <AnimatePresence initial={false} mode="wait">
      {isLoading && (
        <motion.div
          className="absolute inset-0 z-20 flex items-center justify-center bg-black bg-opacity-50"
          initial={shouldReduceMotion ? { opacity: 1 } : { opacity: 0 }}
          animate={{ opacity: 1 }}
          exit={{ opacity: 0 }}
          transition={shouldReduceMotion ? { duration: 0 } : hoverTransition}
        >
          {children}
        </motion.div>
      )}
    </AnimatePresence>
  )
}

/**
 * Play/Pause button component
 */
const HoverVideoPlayerPlayPauseButton: React.FC = () => {
  const { isPlaying, togglePlay } = useHoverVideoPlayer()

  return (
    <Button
      size="icon"
      variant="ghost"
      onClick={togglePlay}
      className="text-white hover:text-primary-foreground"
      aria-label={isPlaying ? "Pause video" : "Play video"}
    >
      {isPlaying ? <Pause className="h-6 w-6" /> : <Play className="h-6 w-6" />}
    </Button>
  )
}

/**
 * Volume control component with mute toggle and volume slider
 */
const HoverVideoPlayerVolumeControl: React.FC = () => {
  const { muted, toggleMute, volume, setVolume } = useHoverVideoPlayer()

  return (
    <div className="flex items-center space-x-2">
      <Button
        size="icon"
        variant="ghost"
        onClick={toggleMute}
        className="text-white hover:text-primary-foreground"
        aria-label={muted ? "Unmute" : "Mute"}
      >
        {muted ? (
          <VolumeX className="h-6 w-6" />
        ) : (
          <Volume2 className="h-6 w-6" />
        )}
      </Button>
      <Slider
        className="w-24"
        value={[muted ? 0 : volume * 100]}
        min={0}
        max={100}
        step={1}
        onValueChange={(value) => setVolume(value[0] / 100)}
        aria-label="Volume"
      />
    </div>
  )
}

/**
 * Progress bar component for video timeline
 */
const HoverVideoPlayerProgressBar: React.FC = () => {
  const { progress, seekProgress } = useHoverVideoPlayer()

  return (
    <Slider
      className="mt-2"
      value={[progress]}
      min={0}
      max={100}
      step={0.1}
      onValueChange={(value) => {
        seekProgress(value[0])
      }}
      aria-label="Video progress"
    />
  )
}

/**
 * Picture-in-Picture toggle button
 */
const HoverVideoPlayerPiPButton: React.FC = () => {
  const { isPiP, togglePiP } = useHoverVideoPlayer()

  return (
    <Button
      size="icon"
      variant="ghost"
      onClick={togglePiP}
      className="text-white hover:text-primary-foreground"
      aria-label={
        isPiP ? "Exit picture in picture" : "Enter picture in picture"
      }
    >
      {isPiP ? (
        <Minimize className="h-6 w-6" />
      ) : (
        <Maximize className="h-6 w-6" />
      )}
    </Button>
  )
}

/**
 * Wrapper component for maintaining aspect ratio with padding
 */
const HoverVideoPaddingWrapper: React.FC<{
  width: number
  height: number
  className?: string
  children?: ReactNode
}> = ({ width, height, className, children }) => {
  return (
    <div
      style={{ "--width": width, "--height": height } as React.CSSProperties}
      className={cn(
        className,
        "relative aspect-[var(--width)/var(--height)] [--radius:theme(borderRadius.xl)]"
      )}
    >
      <div className="absolute -inset-[var(--padding)] rounded-[calc(var(--radius)+var(--padding))] shadow-sm ring-1 ring-black/5 [--padding:theme(spacing.2)]" />
      {children}
    </div>
  )
}

// Add display names for better debugging
HoverVideoPlayerControls.displayName = "HoverVideoPlayerControls"
HoverVideoPaddingWrapper.displayName = "HoverVideoPaddingWrapper"
HoverVideoPlayerHoverOverlay.displayName = "HoverVideoPlayerHoverOverlay"
HoverVideoPlayerPausedOverlay.displayName = "HoverVideoPlayerPausedOverlay"
HoverVideoPlayerLoadingOverlay.displayName = "HoverVideoPlayerLoadingOverlay"
HoverVideoPlayerPlayPauseButton.displayName = "HoverVideoPlayerPlayPauseButton"
HoverVideoPlayerVolumeControl.displayName = "HoverVideoPlayerVolumeControl"
HoverVideoPlayerProgressBar.displayName = "HoverVideoPlayerProgressBar"
HoverVideoPlayerPiPButton.displayName = "HoverVideoPlayerPiPButton"

export {
  HoverVideoPlayer,
  HoverVideoPlayerControls,
  HoverVideoPlayerHoverOverlay,
  HoverVideoPlayerPausedOverlay,
  HoverVideoPlayerLoadingOverlay,
  HoverVideoPlayerPlayPauseButton,
  HoverVideoPlayerVolumeControl,
  HoverVideoPlayerProgressBar,
  HoverVideoPlayerPiPButton,
  HoverVideoPaddingWrapper,
}

export default HoverVideoPlayer

demo.tsx
"use client"

import { motion, useReducedMotion } from "motion/react"

import { cn } from "@/lib/utils"

import { GradientHeading } from "../ui/gradient-heading"
import { HoverVideoPlayer } from "../ui/hover-video-player"

export default function HoverVideoPlayerDemo() {
  const shouldReduceMotion = useReducedMotion()

  return (
    <div className="flex flex-col gap-12 py-12 w-full h-full items-center justify-center">
      <div className="text-center">
        <GradientHeading>Hover video player</GradientHeading>
      </div>
      <motion.div
        animate={shouldReduceMotion ? "expanded" : "collapsed"}
        className="flex aspect-video w-full max-w-4xl items-center justify-center"
        initial={false}
        whileHover={shouldReduceMotion ? undefined : "expanded"}
      >
        <motion.div
          variants={{
            collapsed: { width: "50%" },
            expanded: { width: "100%" },
          }}
          transition={{
            type: "spring",
            duration: 0.42,
            bounce: 0,
          }}
          className={cn(
            "group relative flex flex-col overflow-hidden rounded-lg transform-gpu",
            "bg-white shadow-sm ring-1 ring-black/5",
            "data-dark:bg-stone-800 data-dark:ring-white/15"
          )}
        >
          <HoverVideoPlayer
            videoSrc="https://player.vimeo.com/video/1037289858"
            thumbnailSrc="/placeholders/newcopy-thumbnail.png"
            enableControls
            style={{
              width: "100%",
              maxWidth: "100vw",
              aspectRatio: "16/9",
            }}
          />
        </motion.div>
      </motion.div>

      <a href="https://www.newcult.co" target="_blank" rel="noreferrer">
        newcopy.ai
      </a>
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
