<!-- BeUI Morphing Modal · @saurabh10102 · https://21st.dev/@saurabh10102/components/be-ui-morphing-modal
     license: unspecified · category: modal
      -->

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
components/motion/morphing-modal.tsx
"use client";
// beui.dev/components/motion/morphing-modal

import {
  AnimatePresence,
  motion,
  useReducedMotion,
} from "motion/react";
import { type ReactNode, useEffect } from "react";
import { EASE_OUT, SPRING_PANEL } from "@/lib/ease";
import { PresenceGate } from "@/lib/presence-gate";
import { cn } from "@/lib/utils";

export interface MorphingModalProps {
  /** Which view is currently shown. `null` closes the modal. */
  viewId: string | null;
  onClose: () => void;
  children: ReactNode;
  /** "bottom" anchors to the viewport bottom (mobile-like). "center" centers vertically. */
  placement?: "bottom" | "center";
  className?: string;
}

export function MorphingModal({
  viewId,
  onClose,
  children,
  placement = "bottom",
  className,
}: MorphingModalProps) {
  const open = viewId !== null;
  const reduce = useReducedMotion();
  const enterY = reduce ? 0 : placement === "bottom" ? 40 : 20;
  const enterScale = reduce ? 1 : 0.97;

  useEffect(() => {
    if (!open) return;
    const prev = document.body.style.overflow;
    document.body.style.overflow = "hidden";
    return () => {
      document.body.style.overflow = prev;
    };
  }, [open]);

  // Mounted only while open, and while open the chrome is two fixed siblings
  // rather than one wrapper: the backdrop spans the viewport edges but carries
  // the scrim colour, and the layer positioning the panel sits inset off every
  // edge (`inset-4`, with the bottom placement's `pb-4` on top of it). Both hang
  // off `PresenceGate`, so interaction releases in the same commit that starts
  // the exit rather than when it ends — `open` is already false for those
  // frames. See tests/fixed-overlay-edge-sampling.test.tsx.
  return (
    <AnimatePresence initial={false}>
      {open ? (
        <PresenceGate key="backdrop">
          {({ gate }) => (
            <motion.button
              type="button"
              aria-label="Close modal"
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              exit={{ opacity: 0 }}
              transition={{ duration: 0.2, ease: EASE_OUT }}
              {...gate}
              onClick={onClose}
              className="pointer-events-auto fixed inset-0 z-[80] bg-background/5 [backdrop-filter:blur(14px)_saturate(140%)] [-webkit-backdrop-filter:blur(14px)_saturate(140%)]"
            />
          )}
        </PresenceGate>
      ) : null}

      {open ? (
        <PresenceGate key="panel-layer">
          {({ isPresent, gate }) => (
            // The layer itself never takes pointer events, so it carries
            // `inert` alone rather than the gate's pointer-events value.
            <div
              inert={!isPresent}
              className={cn(
                "pointer-events-none fixed inset-4 z-[80] flex justify-center",
                placement === "bottom" ? "items-end pb-4" : "items-center",
              )}
            >
              <motion.div
                key="panel"
                layout
                initial={{ opacity: 0, y: enterY, scale: enterScale }}
                animate={{ opacity: 1, y: 0, scale: 1 }}
                exit={{
                  opacity: 0,
                  y: enterY,
                  scale: reduce ? 1 : 0.98,
                  transition: { duration: 0.18, ease: EASE_OUT },
                }}
                transition={SPRING_PANEL}
                {...gate}
                className={cn(
                  "pointer-events-auto relative w-full max-w-sm overflow-hidden rounded-3xl border border-border bg-background shadow-2xl will-change-transform",
                  className,
                )}
              >
                <motion.div layout="position" className="p-5">
                  <AnimatePresence mode="popLayout" initial={false}>
                    <motion.div
                      key={viewId}
                      initial={
                        reduce
                          ? { opacity: 0 }
                          : { opacity: 0, y: 8, filter: "blur(4px)" }
                      }
                      animate={
                        reduce
                          ? {
                              opacity: 1,
                              transition: {
                                duration: 0.18,
                                ease: EASE_OUT,
                              },
                            }
                          : {
                              opacity: 1,
                              y: 0,
                              filter: "blur(0px)",
                              transition: {
                                duration: 0.24,
                                ease: EASE_OUT,
                              },
                            }
                      }
                      exit={
                        reduce
                          ? {
                              opacity: 0,
                              transition: {
                                duration: 0.14,
                                ease: EASE_OUT,
                              },
                            }
                          : {
                              opacity: 0,
                              y: -8,
                              filter: "blur(4px)",
                              transition: {
                                duration: 0.16,
                                ease: EASE_OUT,
                              },
                            }
                      }
                    >
                      {children}
                    </motion.div>
                  </AnimatePresence>
                </motion.div>
              </motion.div>
            </div>
          )}
        </PresenceGate>
      ) : null}
    </AnimatePresence>
  );
}

lib/ease.ts
// Shared motion tokens. Easing curves mirror the CSS custom properties in
// globals.css; springs are the canonical physics used across components.
// Strong custom variants — defaults like `ease-in`/`ease-out` feel weak.

export const EASE_OUT = [0.16, 1, 0.3, 1] as const;
export const EASE_IN_OUT = [0.77, 0, 0.175, 1] as const;
export const EASE_DRAWER = [0.32, 0.72, 0, 1] as const;

/** CSS string form of EASE_OUT for inline style transitions. */
export const EASE_OUT_CSS = "cubic-bezier(0.16, 1, 0.3, 1)";

/** Press feedback on buttons and other tappable surfaces. */
export const SPRING_PRESS = {
  type: "spring",
  stiffness: 500,
  damping: 30,
  mass: 0.6,
} as const;

/** Content swaps — label/icon slots trading places inside a control. */
export const SPRING_SWAP = {
  type: "spring",
  stiffness: 460,
  damping: 30,
  mass: 0.55,
} as const;

/** Overlay panel entrances — modals and sheets summoned by pointer. */
export const SPRING_PANEL = {
  type: "spring",
  stiffness: 420,
  damping: 40,
  mass: 0.5,
} as const;

/** Shared-layout glides — pills, indicators and panels morphing between positions. */
export const SPRING_LAYOUT = {
  type: "spring",
  stiffness: 360,
  damping: 32,
  mass: 0.6,
} as const;

/** Cursor-follow physics for decorative mouse tracking (magnetic, tilt, dock). */
export const SPRING_MOUSE = {
  stiffness: 200,
  damping: 15,
  mass: 0.3,
} as const;

/** Dragged handles and fills (sliders) — critically damped `useSpring` config,
 * so the value follows the pointer butterily and never rebounds off an end. */
export const SPRING_GLIDE = {
  stiffness: 700,
  damping: 50,
  mass: 0.5,
} as const;

lib/presence-gate.tsx
"use client";

import { useIsPresent } from "motion/react";
import type { ReactNode } from "react";

export interface PresenceGateRenderProps {
  /**
   * False from the render that starts the exit animation onward. An overlay
   * kept in the tree by `AnimatePresence` is still the topmost thing on the
   * page, so anything it decides from `open` alone stays true for the whole
   * exit — this is the boolean that already knows the overlay is leaving.
   */
  isPresent: boolean;
  /**
   * Spread onto every layer that takes pointer events while the overlay is
   * open. Interaction releases in the same commit that starts the exit while
   * the visual exit keeps playing: pointer events stop landing, and `inert`
   * drops the subtree from focus order, from tab order and from the
   * accessibility tree — an exiting dialog is not a dialog you can still type
   * into. A layer that never takes pointer events (a wrapper that only centres
   * the panel) takes `inert={!isPresent}` alone, so its own
   * `pointer-events-none` is not overwritten.
   */
  gate: {
    inert: boolean;
    style: { pointerEvents: "auto" | "none" };
  };
}

export interface PresenceGateProps {
  children: (props: PresenceGateRenderProps) => ReactNode;
}

/**
 * Reads the presence of the subtree it renders and hands it down.
 *
 * `useIsPresent` only answers inside the `AnimatePresence` subtree, and the
 * components that own an overlay render the `AnimatePresence` themselves, so
 * the boolean has to be read one component further down: this is that
 * component, and the render prop is how it reaches the layers.
 */
export function PresenceGate({ children }: PresenceGateProps) {
  const isPresent = useIsPresent();

  return children({
    isPresent,
    gate: {
      inert: !isPresent,
      style: { pointerEvents: isPresent ? "auto" : "none" },
    },
  });
}

lib/utils.ts
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}

demo.tsx
"use client";

import { useState } from "react";
import { Ban, Lock, ScrollText, ShieldCheck, ScanFace, Trash2 } from "lucide-react";
import { MorphingModal } from "@/components/ui/be-ui-morphing-modal";

type View = "options" | "private-key" | "recovery" | null;

export  default function MorphingModalPreview() {
  const [view, setView] = useState<View>(null);

  return (
    <div className="flex flex-col items-center gap-3">
      <button
        type="button"
        onClick={() => setView("options")}
        className="inline-flex h-10 items-center rounded-full border border-border bg-card px-5 text-sm font-medium text-foreground press hover:border-(--color-border-strong)"
      >
        Open wallet options
      </button>
      <p className="text-xs text-muted-foreground">Click a row. The modal morphs height to match new content.</p>

      <MorphingModal viewId={view} onClose={() => setView(null)}>
        {view === "options" ? (
          <Options
            onPrivateKey={() => setView("private-key")}
            onRecovery={() => setView("recovery")}
            onClose={() => setView(null)}
          />
        ) : view === "private-key" ? (
          <PrivateKey onBack={() => setView("options")} />
        ) : view === "recovery" ? (
          <Recovery onBack={() => setView("options")} />
        ) : null}
      </MorphingModal>
    </div>
  );
}

function Header({ title, onClose }: { title: string; onClose: () => void }) {
  return (
    <div className="mb-4 flex items-center justify-between">
      <h2 className="text-base font-semibold text-foreground">{title}</h2>
      <button
        type="button"
        onClick={onClose}
        aria-label="Close"
        className="inline-flex h-7 w-7 items-center justify-center rounded-full text-muted-foreground hover:bg-foreground/[0.06]"
      >
        ✕
      </button>
    </div>
  );
}

function Row({
  icon: Icon,
  label,
  destructive,
  onClick,
}: {
  icon: typeof Lock;
  label: string;
  destructive?: boolean;
  onClick: () => void;
}) {
  return (
    <button
      type="button"
      onClick={onClick}
      className={`flex w-full items-center gap-3 rounded-2xl px-4 py-3 text-sm font-medium transition-colors press ${
        destructive
          ? "bg-destructive/10 text-destructive hover:bg-destructive/15"
          : "bg-foreground/[0.04] text-foreground hover:bg-foreground/[0.08]"
      }`}
    >
      <Icon className="h-4 w-4" />
      {label}
    </button>
  );
}

function Options({
  onPrivateKey,
  onRecovery,
  onClose,
}: {
  onPrivateKey: () => void;
  onRecovery: () => void;
  onClose: () => void;
}) {
  return (
    <div>
      <Header title="Options" onClose={onClose} />
      <div className="flex flex-col gap-2">
        <Row icon={Lock} label="View Private Key" onClick={onPrivateKey} />
        <Row icon={ScrollText} label="View Recovery Phrase" onClick={onRecovery} />
        <Row icon={Trash2} label="Remove Wallet" destructive onClick={onClose} />
      </div>
    </div>
  );
}

function PrivateKey({ onBack }: { onBack: () => void }) {
  return (
    <div>
      <div className="mb-3 flex items-start justify-between">
        <Lock className="h-5 w-5 text-foreground" />
        <button
          type="button"
          onClick={onBack}
          aria-label="Back"
          className="inline-flex h-7 w-7 items-center justify-center rounded-full text-muted-foreground hover:bg-foreground/[0.06]"
        >
          ✕
        </button>
      </div>
      <h2 className="text-xl font-semibold tracking-tight text-foreground">Private Key</h2>
      <p className="mt-2 text-sm text-muted-foreground">
        Your Private Key is the key used to back up your wallet. Keep it secret and secure at all times.
      </p>
      <hr className="my-4 border-border" />
      <ul className="flex flex-col gap-2.5 text-sm text-muted-foreground">
        <li className="flex items-center gap-2.5"><ShieldCheck className="h-4 w-4" /> Keep your private key safe</li>
        <li className="flex items-center gap-2.5"><ScrollText className="h-4 w-4" /> Don&apos;t share it with anyone else</li>
        <li className="flex items-center gap-2.5"><Ban className="h-4 w-4" /> If you lose it, we can&apos;t recover it</li>
      </ul>
      <div className="mt-5 flex gap-2">
        <button
          type="button"
          onClick={onBack}
          className="inline-flex h-10 flex-1 items-center justify-center rounded-full bg-foreground/[0.06] text-sm font-medium text-foreground press"
        >
          Cancel
        </button>
        <button
          type="button"
          onClick={onBack}
          className="inline-flex h-10 flex-1 items-center justify-center gap-2 rounded-full bg-foreground text-sm font-medium text-background press"
        >
          <ScanFace className="h-4 w-4" />
          Reveal
        </button>
      </div>
    </div>
  );
}

function Recovery({ onBack }: { onBack: () => void }) {
  return (
    <div>
      <div className="mb-3 flex items-start justify-between">
        <ScrollText className="h-5 w-5 text-foreground" />
        <button
          type="button"
          onClick={onBack}
          aria-label="Back"
          className="inline-flex h-7 w-7 items-center justify-center rounded-full text-muted-foreground hover:bg-foreground/[0.06]"
        >
          ✕
        </button>
      </div>
      <h2 className="text-xl font-semibold tracking-tight text-foreground">Recovery Phrase</h2>
      <p className="mt-2 text-sm text-muted-foreground">
        12 words you can use to restore your wallet on any device. Write them down somewhere safe.
      </p>
      <div className="mt-4 grid grid-cols-3 gap-2">
        {["mountain", "river", "candle", "harbor", "amber", "violet", "spring", "ocean", "marble", "thunder", "willow", "crystal"].map((w, i) => (
          <div key={w} className="rounded-lg border border-border bg-background/40 px-2 py-1.5 text-xs text-foreground">
            <span className="mr-1 text-muted-foreground">{i + 1}.</span>
            {w}
          </div>
        ))}
      </div>
      <button
        type="button"
        onClick={onBack}
        className="mt-5 inline-flex h-10 w-full items-center justify-center rounded-full bg-foreground text-sm font-medium text-background press"
      >
        Done
      </button>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install clsx motion tailwind-merge
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
