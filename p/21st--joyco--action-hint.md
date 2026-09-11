<!-- Action Hint · @joyco · https://21st.dev/@joyco/components/action-hint
     license: no-license · category: toast
     A particle emitter that shows an ephemeral action-feedback hint above an element and fades it out, for confirming actions like copy, save, or share. -->

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
components/action-hint.tsx
'use client'

import * as React from 'react'
import { cn } from '@/lib/utils'

/* -------------------------------------------------------------------------------------------------
 * Types
 * -----------------------------------------------------------------------------------------------*/

type Align = 'start' | 'center' | 'end'

type Particle = {
  id: number
  content: React.ReactNode
}

type ActionHintContextValue = {
  emit: (content: React.ReactNode) => void
}

/* -------------------------------------------------------------------------------------------------
 * Context
 * -----------------------------------------------------------------------------------------------*/

const ActionHintContext = React.createContext<ActionHintContextValue | null>(
  null
)

export function useActionHint() {
  const context = React.useContext(ActionHintContext)
  if (!context) {
    throw new Error('useActionHint must be used within an ActionHintEmitter')
  }
  return context
}

/* -------------------------------------------------------------------------------------------------
 * ActionHintEmitter
 * -----------------------------------------------------------------------------------------------*/

interface ActionHintEmitterProps {
  children: React.ReactNode
  className?: string
  /** Horizontal alignment of the hint relative to the emitter */
  align?: Align
  /** Vertical margin (gap) between the hint and the emitter in pixels */
  margin?: number
  /** Duration of the fade animation in milliseconds */
  duration?: number
}

export const ActionHintEmitter = React.forwardRef<
  HTMLDivElement,
  ActionHintEmitterProps
>(function ActionHintEmitter(
  { children, className, align = 'center', margin = 4, duration = 1500 },
  forwardedRef
) {
  const [particles, setParticles] = React.useState<Particle[]>([])
  const idRef = React.useRef(0)

  const emit = React.useCallback(
    (content: React.ReactNode) => {
      const id = idRef.current++

      // Clear all existing particles and add new one
      setParticles([{ id, content }])

      // Remove after animation completes
      setTimeout(() => {
        setParticles((prev) => prev.filter((p) => p.id !== id))
      }, duration)
    },
    [duration]
  )

  const contextValue = React.useMemo(() => ({ emit }), [emit])

  return (
    <ActionHintContext.Provider value={contextValue}>
      <div ref={forwardedRef} className={cn('relative inline-flex', className)}>
        {children}
        {particles.map((particle) => (
          <ActionHintParticle
            key={particle.id}
            align={align}
            margin={margin}
            duration={duration}
          >
            {particle.content}
          </ActionHintParticle>
        ))}
      </div>
    </ActionHintContext.Provider>
  )
})

/* -------------------------------------------------------------------------------------------------
 * ActionHintParticle
 * -----------------------------------------------------------------------------------------------*/

const alignToPosition: Record<
  Align,
  { left?: string; right?: string; transform: string }
> = {
  start: { left: '0', transform: 'translateY(-100%)' },
  center: { left: '50%', transform: 'translateX(-50%) translateY(-100%)' },
  end: { right: '0', transform: 'translateY(-100%)' },
}

function ActionHintParticle({
  children,
  align,
  margin,
  duration,
}: {
  children: React.ReactNode
  align: Align
  margin: number
  duration: number
}) {
  const positionStyles = alignToPosition[align]

  return (
    <>
      <style>
        {`
          @keyframes action-hint-fade {
            0% {
              opacity: 1;
              margin-top: var(--action-hint-margin);
            }
            100% {
              opacity: 0;
              margin-top: calc(var(--action-hint-margin) - 1rem);
            }
          }
        `}
      </style>
      <div
        data-slot="action-hint-particle"
        className={cn(
          'pointer-events-none absolute top-0 z-50',
          'rounded-md px-2 py-1 text-xs font-medium whitespace-nowrap shadow-sm',
          'bg-secondary text-secondary-foreground'
        )}
        style={
          {
            ...positionStyles,
            '--action-hint-margin': `${-margin}px`,
            animation: `action-hint-fade ${duration}ms ease-out forwards`,
          } as React.CSSProperties
        }
      >
        {children}
      </div>
    </>
  )
}

demo.tsx
"use client";

import { ActionHintEmitter, useActionHint } from "@/components/ui/action-hint";
import { Button } from "@/components/ui/button";
import { Copy, Check, Download, Share2, Bookmark } from "lucide-react";

function CopyButton() {
  const { emit } = useActionHint();
  return (
    <Button
      variant="secondary"
      size="sm"
      onClick={() =>
        emit(
          <span className="flex items-center gap-1.5">
            <Check className="size-3" />
            Copied!
          </span>,
        )
      }
    >
      <Copy className="size-4" />
      Copy
    </Button>
  );
}

function DownloadButton() {
  const { emit } = useActionHint();
  return (
    <Button
      variant="secondary"
      size="sm"
      onClick={() =>
        emit(
          <span className="flex items-center gap-1.5">
            <Download className="size-3" />
            Downloading ...
          </span>,
        )
      }
    >
      <Download className="size-4" />
      Download
    </Button>
  );
}

function ShareButton() {
  const { emit } = useActionHint();
  return (
    <Button
      variant="secondary"
      size="sm"
      onClick={() =>
        emit(
          <span className="flex items-center gap-1.5">
            <Share2 className="size-3" />
            Link shared!
          </span>,
        )
      }
    >
      <Share2 className="size-4" />
      Share
    </Button>
  );
}

function SaveButton() {
  const { emit } = useActionHint();
  return (
    <Button
      variant="secondary"
      size="sm"
      onClick={() =>
        emit(
          <span className="flex items-center gap-1.5">
            <Bookmark className="size-3" />
            Saved!
          </span>,
        )
      }
    >
      <Bookmark className="size-4" />
      Save
    </Button>
  );
}

export default function ActionHintDemo() {
  return (
    <div className="flex min-h-40 w-full items-center justify-center p-8">
      <div className="flex flex-wrap items-center gap-2">
        <ActionHintEmitter>
          <CopyButton />
        </ActionHintEmitter>

        <ActionHintEmitter>
          <DownloadButton />
        </ActionHintEmitter>

        <ActionHintEmitter>
          <ShareButton />
        </ActionHintEmitter>

        <ActionHintEmitter>
          <SaveButton />
        </ActionHintEmitter>
      </div>
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button utils
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
