<!-- Image Text · @animbits · https://21st.dev/@animbits/components/image-text
     license: MIT · category: text
     Large heading text filled with an animated image mask that pans and can cycle between multiple photos. -->

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
components/ui/image-text.tsx
"use client";

import React, { useState, useEffect } from "react";
import { motion, HTMLMotionProps, AnimatePresence } from "motion/react";
import { cn } from "@/lib/utils";

interface ImageTextProps extends HTMLMotionProps<"h1"> {
    text: string;
    imageUrl: string | string[] | { src: string } | { src: string }[];
    className?: string;
    direction?: "horizontal" | "vertical" | "diagonal" | "none";
    interval?: number;
}

export function ImageText({
    text,
    imageUrl,
    className,
    direction = "horizontal",
    interval = 3000,
    ...props
}: ImageTextProps) {
    const images = Array.isArray(imageUrl) ? imageUrl : [imageUrl];
    const [currentImageIndex, setCurrentImageIndex] = useState(0);

    useEffect(() => {
        if (!Array.isArray(imageUrl) || imageUrl.length <= 1) return;

        const timer = setInterval(() => {
            setCurrentImageIndex((prev) => (prev + 1) % images.length);
        }, interval);

        return () => clearInterval(timer);
    }, [imageUrl, interval, images.length]);

    // Helper to handle both string URLs and StaticImageData objects
    const getCurrentImageUrl = () => {
        const img = images[currentImageIndex];
        return typeof img === "string" ? img : img.src;
    };

    const getAnimation = () => {
        switch (direction) {
            case "horizontal":
                return {
                    backgroundPosition: ["0% 50%", "100% 50%", "0% 50%"],
                };
            case "vertical":
                return {
                    backgroundPosition: ["50% 0%", "50% 100%", "50% 0%"],
                };
            case "diagonal":
                return {
                    backgroundPosition: ["0% 0%", "100% 100%", "0% 0%"],
                };
            case "none":
                return {
                    backgroundPosition: "center",
                }
            default:
                return {
                    backgroundPosition: ["0% 50%", "100% 50%", "0% 50%"],
                };
        }
    };

    return (
        <span className={cn("inline-block relative", className)}>
            {/* Invisible layout placeholder ensuring size doesn't collapse */}
            <h1 className={cn("text-9xl font-serif italic font-bold tracking-tighter inline-block invisible", className)}>
                {text}
            </h1>

            <AnimatePresence>
                <motion.h1
                    key={currentImageIndex} // Re-render for image change
                    className={cn(
                        "text-9xl font-serif italic font-bold tracking-tighter inline-block absolute inset-0",
                        className
                    )}
                    style={{
                        backgroundImage: `url(${getCurrentImageUrl()})`,
                        backgroundSize: direction === "none" ? "cover" : "150% auto",
                        backgroundClip: "text",
                        WebkitBackgroundClip: "text",
                        WebkitTextFillColor: "transparent",
                        color: "transparent",
                    }}
                    initial={{ opacity: 0 }}
                    animate={{
                        ...getAnimation(),
                        opacity: 1
                    }}
                    exit={{ opacity: 0 }}
                    transition={{
                        // Smooth opacity fade for texture change
                        opacity: { duration: 1 },
                        // Continuous background movement
                        backgroundPosition: {
                            duration: 15,
                            ease: "linear",
                            repeat: Infinity,
                        }
                    }}
                    {...props}
                >
                    {text}
                </motion.h1>
            </AnimatePresence>
        </span>
    );
}

demo.tsx
import { ImageText } from "@/components/ui/image-text";

export default function Default() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center bg-neutral-950 p-8">
      <ImageText
        text="Velocity"
        imageUrl="https://cdn.21st.dev/assets/mirror/6b/6bee424aed70617b7512c3332ce44afbf6b5d5fa2af64cb16e446ec3cba10e31.jpg"
        direction="diagonal"
        className="text-8xl font-black tracking-tighter"
      />
    </div>
  );
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
