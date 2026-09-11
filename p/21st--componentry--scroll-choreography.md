<!-- Scroll Choreography · @componentry · https://21st.dev/@componentry/components/scroll-choreography
     license: no-license · category: hero
     A scroll-driven animation that choreographs four quadrant images across the viewport before one hero image expands to fill the screen. -->

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
components/ui/scroll-choreography.tsx
"use client";

import { motion, useScroll, useSpring, useTransform } from "framer-motion";
import { useRef } from "react";
import { cn } from "@workspace/ui/lib/utils";

interface ScrollChoreographyProps {
  className?: string;
  images: {
    topLeft: string;
    topRight: string;
    bottomLeft: string;
    bottomRight: string;
  };
}

export function ScrollChoreography({
  className,
  images,
}: ScrollChoreographyProps) {
  const containerRef = useRef<HTMLDivElement>(null);

  const { scrollYProgress } = useScroll({
    target: containerRef,
    offset: ["start start", "end end"],
  });

  const smoothProgress = useSpring(scrollYProgress, {
    stiffness: 400, // Higher stiffness for a slightly faster snap
    damping: 50,   // Play with damping to add a little bounce/jerk
    mass: 1.2,     // Adds a bit more weight to the movement
    restDelta: 0.001,
  });

  // Default positions relative to center
  const xLeft = "-20vw";
  const xRight = "20vw";
  const yTop = "-14vh";
  const yBottom = "14vh";

  // Phase 1: 0 - 0.3 (Diagonal movement)
  // Phase 2: 0.35 - 0.65 (Stack alignment to center)
  // Phase 3: 0.7 - 0.9 (Top Right expands to full screen)

  // Top Left -> moves to Bottom Left, then to Center
  const tlX = useTransform(smoothProgress, [0, 0.3, 0.35, 0.65, 1], [xLeft, xLeft, xLeft, "0vw", "0vw"]);
  const tlY = useTransform(smoothProgress, [0, 0.3, 0.35, 0.65, 1], [yTop, yBottom, yBottom, "0vh", "0vh"]);

  // Bottom Right -> moves to Top Right, then to Center
  const brX = useTransform(smoothProgress, [0, 0.3, 0.35, 0.65, 1], [xRight, xRight, xRight, "0vw", "0vw"]);
  const brY = useTransform(smoothProgress, [0, 0.3, 0.35, 0.65, 1], [yBottom, yTop, yTop, "0vh", "0vh"]);

  // Bottom Left -> stays, then moves to Center
  const blX = useTransform(smoothProgress, [0, 0.3, 0.35, 0.65, 1], [xLeft, xLeft, xLeft, "0vw", "0vw"]);
  const blY = useTransform(smoothProgress, [0, 0.3, 0.35, 0.65, 1], [yBottom, yBottom, yBottom, "0vh", "0vh"]);

  // Top Right -> stays, then moves to Center, then expands
  const trX = useTransform(smoothProgress, [0, 0.3, 0.35, 0.65, 1], [xRight, xRight, xRight, "0vw", "0vw"]);
  const trY = useTransform(smoothProgress, [0, 0.3, 0.35, 0.65, 1], [yTop, yTop, yTop, "0vh", "0vh"]);

  // Top Right (Hero) scaling/expansion properties
  const heroWidth = useTransform(smoothProgress, [0.65, 0.7, 0.9, 1], ["36vw", "36vw", "100vw", "100vw"]);
  const heroHeight = useTransform(smoothProgress, [0.65, 0.7, 0.9, 1], ["24vh", "24vh", "100vh", "100vh"]);

  // Opacity fading for images underneath the hero as it expands
  const underImagesOpacity = useTransform(smoothProgress, [0.75, 0.85], [1, 0]);

  const baseImageClasses =
    "absolute left-1/2 top-1/2 w-[36vw] h-[24vh] overflow-hidden -translate-x-1/2 -translate-y-1/2 bg-muted shadow-2xl will-change-transform";

  return (
    <div ref={containerRef} className={cn("relative h-[300vh] w-full", className)}>
      <div className="sticky top-0 h-screen w-full overflow-hidden">
        <div className="absolute inset-0 flex items-center justify-center">

          {/* Top Left Image */}
          <motion.div
            style={{ x: tlX, y: tlY, opacity: underImagesOpacity }}
            className={cn(baseImageClasses, "z-10")}
          >
            <img src={images.topLeft} alt="Top Left" className="h-full w-full object-cover" />
          </motion.div>

          {/* Bottom Right Image */}
          <motion.div
            style={{ x: brX, y: brY, opacity: underImagesOpacity }}
            className={cn(baseImageClasses, "z-20")}
          >
            <img src={images.bottomRight} alt="Bottom Right" className="h-full w-full object-cover" />
          </motion.div>

          {/* Bottom Left Image */}
          <motion.div
            style={{ x: blX, y: blY, opacity: underImagesOpacity }}
            className={cn(baseImageClasses, "z-30")}
          >
            <img src={images.bottomLeft} alt="Bottom Left" className="h-full w-full object-cover" />
          </motion.div>

          {/* Top Right Image (Hero - expands at the end) */}
          <motion.div
            style={{
              x: trX,
              y: trY,
              width: heroWidth,
              height: heroHeight,
            }}
            className={cn(baseImageClasses, "z-40 origin-center bg-black/5")}
          >
            <img src={images.topRight} alt="Top Right (Hero)" className="h-full w-full object-cover" />
          </motion.div>
        </div>
      </div>
    </div>
  );
}

demo.tsx
import { ScrollChoreography } from "@/components/ui/scroll-choreography"

const images = {
  topLeft: "https://cdn.21st.dev/assets/mirror/d3/d310c7e0904578a268f9b43e56334bbedab73259827f78fa5820bae55938adf0.webp",
  topRight: "https://cdn.21st.dev/assets/mirror/e1/e1a768d91c721771a9095e1079f7ac04e9ce8b81d2264db6f614c42d4cc65a6b.webp",
  bottomLeft: "https://cdn.21st.dev/assets/mirror/00/00f17e7c8379b3c4629810073d1c1fe0c79946ae0e0660f05aebf7c21b0ebbde.webp",
  bottomRight: "https://cdn.21st.dev/assets/mirror/1a/1a300fea6e7f3a7e530448dd8277eb31e0f11c2e726f09ccbac310d516f8075f.webp",
}

export default function Demo() {
  return (
    <div className="w-full min-h-screen">
      <ScrollChoreography images={images} />
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install framer-motion
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
