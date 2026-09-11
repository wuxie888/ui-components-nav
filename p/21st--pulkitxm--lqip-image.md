<!-- LQIP Image · @pulkitxm · https://21st.dev/@pulkitxm/components/lqip-image
     license: no-license · category: image
     Image component with a Low Quality Image Placeholder (LQIP) that shows a blurred placeholder and fades into the full image once it loads. -->

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
components/ui/lqip-image.tsx
"use client";

import Image, { type StaticImageData } from "next/image";
import { useState } from "react";
import { cn } from "@/lib/utils";

interface LqipImageProps {
  src: string | StaticImageData;
  placeholderSrc?: StaticImageData;
  alt: string;
  width?: number;
  height?: number;
  className?: string;
  containerClassName?: string;
  sizes?: string;
  priority?: boolean;
  fill?: boolean;
  quality?: number;
}

function isStaticImageData(src: string | StaticImageData): src is StaticImageData {
  return typeof src === "object" && "src" in src;
}

export function LqipImage({
  src,
  placeholderSrc,
  alt,
  width,
  height,
  className,
  containerClassName,
  sizes,
  priority = false,
  fill = false,
  quality,
}: LqipImageProps) {
  const [loaded, setLoaded] = useState(false);

  const srcString = isStaticImageData(src) ? src.src : src;
  const hasAutoPlaceholder = !(placeholderSrc || isStaticImageData(src));
  const showPlaceholder = placeholderSrc || hasAutoPlaceholder;

  const placeholderImageSrc = hasAutoPlaceholder
    ? ({ height: 2, src: srcString, width: 2 } as { src: string; width: number; height: number })
    : placeholderSrc;

  return (
    <div
      className={cn("relative overflow-hidden", containerClassName)}
      style={
        fill
          ? { height: "100%", position: "relative", width: "100%" }
          : width != null && height != null
            ? { height, width }
            : undefined
      }
      aria-busy={!loaded}
      role="img"
      aria-label={alt}
    >
      {showPlaceholder && placeholderImageSrc && (
        <Image
          src={placeholderImageSrc}
          alt=""
          aria-hidden={true}
          className={cn(
            "absolute inset-0 size-full object-cover transition-opacity duration-700 ease-out",
            loaded ? "opacity-0" : "opacity-100",
            "scale-[1.1] blur-[20px]",
          )}
          fill={true}
          sizes={sizes}
        />
      )}
      <Image
        src={src}
        alt={alt}
        className={cn(
          "object-cover transition-opacity duration-700 ease-out",
          loaded ? "opacity-100" : "opacity-0",
          fill ? "size-full" : undefined,
          className,
        )}
        onLoad={() => setLoaded(true)}
        loading={priority ? "eager" : "lazy"}
        {...(fill ? { fill: true } : { height: height ?? 600, width: width ?? 800 })}
        {...(sizes !== undefined && { sizes })}
        {...(quality !== undefined && { quality })}
      />
    </div>
  );
}

demo.tsx
import { LqipImage } from "@/components/ui/lqip-image";

export default function LqipImageDemo() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center bg-background p-8">
      <LqipImage
        src="https://cdn.21st.dev/assets/mirror/a4/a4582af2139ddf3a31f563d73f0b267cc5178f46af22bf59213f43e8bb059c1f.webp"
        alt="Aerial view of ocean waves meeting the shore"
        width={820}
        height={324}
        priority
        sizes="820px"
        className="rounded-xl"
        containerClassName="rounded-xl border border-border shadow-sm"
      />
    </div>
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
