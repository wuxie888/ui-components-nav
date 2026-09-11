<!-- Parallax Image · @pulkitxm · https://21st.dev/@pulkitxm/components/parallax-image
     license: MIT · category: gallery
     Scroll-linked parallax wrapper that shifts images or content vertically on scroll to create a sense of depth. -->

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
components/ui/parallax-image.tsx
"use client";

import { motion, useMotionValue } from "framer-motion";
import { useEffect, useRef } from "react";
import { cn } from "@/lib/utils";

interface ParallaxImageProps {
  children: React.ReactNode;
  className?: string;
  childrenClassName?: string;
  intensity?: number;
  containerRef?: React.RefObject<HTMLElement | null>;
}

export function ParallaxImage({
  children,
  className,
  childrenClassName,
  intensity = 50,
  containerRef,
}: ParallaxImageProps) {
  const ref = useRef<HTMLDivElement>(null);
  const y = useMotionValue(0);

  useEffect(() => {
    const el = ref.current;
    if (!el) {
      return;
    }

    const scrollEl: HTMLElement | Window = containerRef?.current ?? window;

    const update = () => {
      const containerTop = containerRef?.current ? containerRef.current.getBoundingClientRect().top : 0;
      const containerHeight = containerRef?.current ? containerRef.current.clientHeight : window.innerHeight;

      const rect = el.getBoundingClientRect();
      const elCenter = rect.top + rect.height / 2 - containerTop;
      const normalized = (elCenter / containerHeight - 0.5) * 2;

      y.set(normalized * intensity);
    };

    scrollEl.addEventListener("scroll", update, { passive: true });
    update();

    return () => scrollEl.removeEventListener("scroll", update);
  }, [containerRef, intensity, y]);

  return (
    <motion.div ref={ref} className={cn("relative overflow-hidden", className)}>
      <motion.div
        style={{
          bottom: -intensity,
          left: 0,
          position: "absolute",
          right: 0,
          top: -intensity,
          y,
        }}
        className={cn("[&_img]:size-full [&_img]:object-cover", childrenClassName)}
      >
        {children}
      </motion.div>
    </motion.div>
  );
}

demo.tsx
"use client";

import { ParallaxImage } from "@/components/ui/parallax-image";
import { useRef } from "react";

const IMAGES = [
  {
    src: "https://www.pulkit.page/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fmountain-lake.0rjyk7ectwbfv.webp&w=640&q=75&dpl=dpl_BZkeptmgHcGQhqRVHCUdGFqm6TXA",
    alt: "Mountain lake",
  },
  {
    src: "https://www.pulkit.page/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fforest-path.17xno5kn9lint.webp&w=640&q=75&dpl=dpl_BZkeptmgHcGQhqRVHCUdGFqm6TXA",
    alt: "Forest path",
  },
  {
    src: "https://www.pulkit.page/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fdesert-dunes.1ye7y5b4vv8a4.webp&w=640&q=75&dpl=dpl_BZkeptmgHcGQhqRVHCUdGFqm6TXA",
    alt: "Desert dunes",
  },
  {
    src: "https://www.pulkit.page/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Focean-waves.2xm0rm20y_whm.webp&w=640&q=75&dpl=dpl_BZkeptmgHcGQhqRVHCUdGFqm6TXA",
    alt: "Ocean waves",
  },
  {
    src: "https://www.pulkit.page/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fsnowy-peaks.0ary_8z_biis7.webp&w=640&q=75&dpl=dpl_BZkeptmgHcGQhqRVHCUdGFqm6TXA",
    alt: "Snowy peaks",
  },
  {
    src: "https://www.pulkit.page/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fautumn-valley.11ky9fnsfnra6.webp&w=640&q=75&dpl=dpl_BZkeptmgHcGQhqRVHCUdGFqm6TXA",
    alt: "Autumn valley",
  },
];

export default function ParallaxImageDemo() {
  const containerRef = useRef<HTMLDivElement>(null);

  return (
    <div className="flex w-full items-center justify-center bg-background p-6">
      <div className="w-full max-w-md overflow-hidden rounded-xl border border-border bg-card shadow-sm">
        <div className="flex items-center justify-between border-b border-border px-5 py-4">
          <h3 className="text-lg font-semibold text-foreground">
            Scroll to explore
          </h3>
          <span className="text-sm text-muted-foreground">↓ scroll</span>
        </div>

        <div ref={containerRef} className="h-[430px] overflow-y-auto">
          <div className="grid grid-cols-2 gap-3 p-3">
            {IMAGES.map((img) => (
              <div
                key={img.alt}
                className="overflow-hidden rounded-lg border border-border bg-muted"
              >
                <ParallaxImage
                  className="h-40"
                  intensity={40}
                  containerRef={containerRef}
                >
                  <img
                    src={img.src}
                    alt={img.alt}
                    className="size-full object-cover"
                  />
                </ParallaxImage>
                <div className="px-3 py-2 text-sm text-muted-foreground">
                  {img.alt}
                </div>
              </div>
            ))}
          </div>
        </div>
      </div>
    </div>
  );
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
