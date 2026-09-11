<!-- Image Swiper · @lukacho · https://21st.dev/@lukacho/components/image-swiper
     license: MIT · category: gallery
     Image swiper card for a real estate website -->

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
components/ui/image-swiper.tsx
"use client";

import * as React from "react";
import { motion, useMotionValue } from "framer-motion";
import { ChevronLeft, ChevronRight } from "lucide-react";
import { Button } from "@/components/ui/button";
import { cn } from "@/lib/utils";

interface ImageSwiperProps extends React.HTMLAttributes<HTMLDivElement> {
  images: string[];
}

export function ImageSwiper({ images, className, ...props }: ImageSwiperProps) {
  const [imgIndex, setImgIndex] = React.useState(0);
  const dragX = useMotionValue(0);

  const onDragEnd = () => {
    const x = dragX.get();
    if (x <= -10 && imgIndex < images.length - 1) {
      setImgIndex((prev) => prev + 1);
    } else if (x >= 10 && imgIndex > 0) {
      setImgIndex((prev) => prev - 1);
    }
  };

  return (
    <div
      className={cn(
        "group relative aspect-square h-full w-full overflow-hidden rounded-lg",
        className
      )}
      {...props}
    >
      <div className="pointer-events-none absolute inset-0 z-10">
        {imgIndex > 0 && (
          <div className="absolute left-5 top-1/2 -translate-y-1/2">
            <Button
              variant="ghost"
              size="icon"
              className="pointer-events-auto h-8 w-8 rounded-full bg-white/80 opacity-0 transition-opacity group-hover:opacity-100"
              onClick={() => setImgIndex((prev) => prev - 1)}
            >
              {/* NOTE: icon identifiers are not defined inside the bundle slice; inferred as ChevronLeft/ChevronRight from left/right button placement. */}
              <ChevronLeft className="h-4 w-4 text-neutral-600" />
            </Button>
          </div>
        )}
        {imgIndex < images.length - 1 && (
          <div className="absolute right-5 top-1/2 -translate-y-1/2">
            <Button
              variant="ghost"
              size="icon"
              className="pointer-events-auto h-8 w-8 rounded-full bg-white/80 opacity-0 transition-opacity group-hover:opacity-100"
              onClick={() => setImgIndex((prev) => prev + 1)}
            >
              <ChevronRight className="h-4 w-4 text-neutral-600" />
            </Button>
          </div>
        )}
        <div className="absolute bottom-2 w-full flex justify-center">
          <div className="flex min-w-9 items-center justify-center rounded-md bg-black/80 px-2 py-0.5 text-xs text-white opacity-0 transition-opacity group-hover:opacity-100">
            {imgIndex + 1}/{images.length}
          </div>
        </div>
      </div>
      <motion.div
        drag="x"
        dragConstraints={{ left: 0, right: 0 }}
        dragMomentum={false}
        style={{ x: dragX }}
        animate={{ translateX: `-${imgIndex * 100}%` }}
        onDragEnd={onDragEnd}
        transition={{
          damping: 18,
          stiffness: 90,
          type: "spring",
          duration: 0.2,
        }}
        className=" flex h-full cursor-grab items-center rounded-[inherit] active:cursor-grabbing"
      >
        {images.map((src, i) => (
          <motion.div
            key={i}
            className="h-full w-full shrink-0 overflow-hidden bg-neutral-800 object-cover first:rounded-l-[inherit] last:rounded-r-[inherit]"
          >
            <img src={src} className="pointer-events-none h-full w-full object-cover" />
          </motion.div>
        ))}
      </motion.div>
    </div>
  );
}

demo.tsx
import { ImageSwiper } from "@/components/ui/image-swiper"
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'

const images = [
  'https://cdn.21st.dev/assets/mirror/c8/c82a0d93c8d49418568e08924e7615431337c0a1aeeaf773ad4e09c3ba4ffbc9.webp',
  'https://cdn.21st.dev/assets/mirror/31/31a45a259b59134b4296afb0848d5f39ef580746eec83c160ea157059768d9c2.webp',
  'https://cdn.21st.dev/assets/mirror/90/906ab60ca26b478c0c52953d0cd21f54ddbb21d96bb6d2239ef64aa33e4a7e11.webp',
  'https://cdn.21st.dev/assets/mirror/ff/ff6cabcef01f8d32ed0cd44c143985536ec5176759cd29eced971e86289a863f.webp'
]

export function RealEstateCard() {
  return (
    <Card className="max-w-[400px]">
      <CardContent className="p-0">
        <ImageSwiper images={images} />
      </CardContent>
      <CardHeader>
        <CardTitle className="text-lg font-semibold">Batumi, Georgia</CardTitle>
        <p className="text-sm text-muted-foreground">5000 Kilometers away</p>
        <p className="mt-1">
          <span className="font-semibold">$200</span> night
        </p>
      </CardHeader>
    </Card>
  )
}
```

Install NPM dependencies:
```bash
npm install framer-motion lucide-react
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
