<!-- Receding Text Plane · @ruixen.ui · https://21st.dev/@ruixen.ui/components/receding-text-plane
     license: MIT · category: text
     A slab of copy raked away from the viewer on a short lens. Near lines swell past the frame edges, far lines fold toward a horizon that was never drawn — no scroll, no animation, one CSS transform. -->

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
components/ui/receding-text-plane.tsx
"use client";

import * as React from "react";
import { cn } from "@/lib/utils";

export interface RecedingTextPlaneProps
  extends React.HTMLAttributes<HTMLDivElement> {
  /**
   * Degrees the plane rakes away from the viewer. 0 is flat type; past ~45 the
   * near lines outrun the frame before the far ones have finished folding.
   */
  angle?: number;
  /**
   * Copy laid onto the plane. Long enough to wrap several times is the point —
   * the effect lives in the depth gap between one line and the next, so a
   * single line has nothing to read against.
   */
  children: React.ReactNode;
  /**
   * Slides the slab along the raked plane, in pixels — the framing knob. 0
   * centres it on the frame. Positive pushes the near, blown-out lines into
   * view; negative pulls the far, folded ones in.
   */
  offset?: number;
  /**
   * Viewer distance in pixels. A wide lens by default — at 200 the near lines
   * swell past 2x while the far ones fold into the horizon. Past ~600 the whole
   * thing flattens into a mild tilt.
   */
  perspective?: number;
}

export function RecedingTextPlane({
  angle = 30,
  children,
  className,
  offset = 0,
  perspective = 200,
  ...props
}: RecedingTextPlaneProps) {
  return (
    <div
      className={cn(
        "relative h-screen w-full overflow-hidden bg-muted",
        className,
      )}
      {...props}
    >
      {/*
        Pinned to the frame rather than sized by the copy. Let this box grow with
        the text and the pivot drifts down with it, so the same `offset` frames a
        short headline and a long paragraph completely differently.
      */}
      <div
        className="absolute inset-0 flex items-center justify-center"
        style={{
          perspective: `${perspective}px`,
          transformStyle: "preserve-3d",
        }}
      >
        {/*
          Rake, then slide, then lift off the plane — read right to left, the
          slab is pushed down the flat plane first and only then tipped, so
          `offset` moves the type along the ground rather than across the screen.
          The 10px of Z keeps the near edge off the plane itself, which stops the
          bottom line from clipping through the vanishing point.
        */}
        <p
          className="w-full max-w-4xl text-center text-4xl font-bold tracking-tighter text-chart-2 sm:text-5xl md:text-6xl"
          style={{
            transform: `rotateX(${angle}deg) translateY(${offset}px) translateZ(10px)`,
            transformStyle: "preserve-3d",
          }}
        >
          {children}
        </p>
      </div>
    </div>
  );
}

demo.tsx
// This is a file with a demo for your component
// That's what users will see in the preview
// Create new files in this directory to add more demos
"use client";

import { RecedingTextPlane } from "@/components/ui/receding-text-plane";

// ONLY DEFAULT EXPORT WILL BE TREATED AS A DEMO
export default function DemoOne() {
  return (
    <RecedingTextPlane className="h-[600px]">
      Type is a plane in space before it is a message on a page. Rake that plane
      away from the eye and every line finds a depth of its own.
    </RecedingTextPlane>
  );
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
