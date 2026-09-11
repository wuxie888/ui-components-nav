<!-- Image Metadata Preview · @educalvolpz · https://21st.dev/@educalvolpz/components/image-metadata-preview
     license: MIT · category: image
     An image preview card that lifts to reveal a filename, description, and EXIF-style metadata panel with smooth Motion animations. -->

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
components/ui/index.tsx
"use client";

import { ChevronUp, CircleX, Share } from "lucide-react";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import { useEffect, useState } from "react";
import useMeasure from "react-use-measure";

export interface ImageMetadata {
  by: string;
  created: string;
  source: string;
  updated: string;
}

export interface ImageMetadataPreviewProps {
  alt?: string;
  description?: string;
  filename?: string;
  imageSrc: string;
  metadata: ImageMetadata;
  onShare?: () => void;
}

export default function ImageMetadataPreview({
  imageSrc,
  alt = "Image preview",
  filename = "screenshot.png",
  description = "No description",
  metadata,
  onShare,
}: ImageMetadataPreviewProps) {
  const [openInfo, setopenInfo] = useState(false);
  const [isHoverDevice, setIsHoverDevice] = useState(false);
  const [elementRef, bounds] = useMeasure();
  const shouldReduceMotion = useReducedMotion();

  useEffect(() => {
    const mediaQuery = window.matchMedia("(hover: hover) and (pointer: fine)");
    setIsHoverDevice(mediaQuery.matches);

    const handleChange = (e: MediaQueryListEvent) => {
      setIsHoverDevice(e.matches);
    };

    mediaQuery.addEventListener("change", handleChange);
    return () => mediaQuery.removeEventListener("change", handleChange);
  }, []);

  const handleClickOpen = () => {
    setopenInfo((b) => !b);
  };

  const handleClickClose = () => {
    setopenInfo((b) => !b);
  };

  return (
    <div className="absolute bottom-10 flex flex-col items-center justify-center gap-4">
      <motion.div
        animate={shouldReduceMotion ? {} : { y: -bounds.height }}
        className="pointer-events-none overflow-hidden rounded-xl"
        transition={shouldReduceMotion ? { duration: 0 } : { duration: 0.25 }}
      >
        <img
          alt={alt}
          draggable={false}
          height={437}
          src={imageSrc}
          width={300}
        />
      </motion.div>

      <div className="relative flex w-full flex-col items-center gap-4">
        <div className="relative flex w-full flex-row items-center justify-center gap-4">
          <button
            aria-label="Share"
            className={`min-h-[44px] min-w-[44px] rounded-full border bg-background p-3 transition focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 ${
              isHoverDevice ? "hover:bg-muted" : ""
            }`}
            disabled={!onShare}
            onClick={onShare}
            type="button"
          >
            <Share aria-hidden="true" size={16} />
          </button>
          <button
            aria-label="Connect"
            className="min-h-[44px] cursor-not-allowed rounded-full border bg-background px-4 py-3 text-sm transition focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:opacity-50"
            disabled
            type="button"
          >
            Connect
          </button>
          <AnimatePresence>
            {openInfo ? null : (
              <motion.button
                animate={
                  shouldReduceMotion
                    ? { opacity: 1 }
                    : { filter: "blur(0px)", opacity: 1 }
                }
                aria-label="Open Metadata Preview"
                className={`min-h-[44px] min-w-[44px] border bg-background p-3 shadow-xs transition focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 ${
                  isHoverDevice ? "hover:bg-muted" : ""
                }`}
                initial={
                  shouldReduceMotion
                    ? { opacity: 1 }
                    : { filter: "blur(4px)", opacity: 0 }
                }
                onClick={handleClickOpen}
                style={{ borderRadius: 100 }}
                transition={
                  shouldReduceMotion ? { duration: 0 } : { duration: 0.2 }
                }
              >
                <ChevronUp aria-hidden="true" size={16} />
              </motion.button>
            )}
          </AnimatePresence>
        </div>
        <AnimatePresence>
          {openInfo ? (
            <motion.div
              animate={
                shouldReduceMotion
                  ? { opacity: 1 }
                  : { filter: "blur(0px)", opacity: 1 }
              }
              className="absolute bottom-0 w-full cursor-pointer gap-4 border bg-background p-5 shadow-xs"
              initial={
                shouldReduceMotion
                  ? { opacity: 1 }
                  : { filter: "blur(4px)", opacity: 0 }
              }
              onClick={handleClickClose}
              style={{ borderRadius: 20 }}
              transition={
                shouldReduceMotion
                  ? { duration: 0 }
                  : { bounce: 0, duration: 0.25, type: "spring" as const }
              }
            >
              <div className="flex flex-col items-start" ref={elementRef}>
                <div className="flex w-full flex-row items-start justify-between gap-4">
                  <div>
                    <p className="text-foreground">{filename}</p>
                    <p className="text-primary-foreground">{description}</p>
                  </div>

                  <button
                    aria-label="Close metadata preview"
                    className={`flex min-h-[44px] min-w-[44px] items-center justify-center rounded p-2 transition-colors focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 ${
                      isHoverDevice ? "hover:bg-muted" : ""
                    }`}
                    onClick={(e) => {
                      e.stopPropagation();
                      handleClickClose();
                    }}
                    type="button"
                  >
                    <CircleX aria-hidden="true" size={16} />
                  </button>
                </div>
                <table className="flex w-full flex-col items-center gap-4 text-foreground">
                  <tbody className="w-full">
                    <tr className="flex w-full flex-row items-center gap-4">
                      <td className="w-1/2">Created</td>
                      <td className="w-1/2 text-primary-foreground">
                        {metadata.created}
                      </td>
                    </tr>
                    <tr className="flex w-full flex-row items-center gap-4">
                      <td className="w-1/2">Updated</td>
                      <td className="w-1/2 text-primary-foreground">
                        {metadata.updated}
                      </td>
                    </tr>
                    <tr className="flex w-full flex-row items-center gap-4">
                      <td className="w-1/2">By</td>
                      <td className="w-1/2">{metadata.by}</td>
                    </tr>
                    <tr className="flex w-full flex-row items-center gap-4">
                      <td className="w-1/2">Source</td>
                      <td className="w-1/2 truncate">{metadata.source}</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </motion.div>
          ) : null}
        </AnimatePresence>
      </div>
    </div>
  );
}

demo.tsx
"use client";

import ImageMetadataPreview from "@/components/ui/image-metadata-preview";

const sampleMetadata = {
  created: "2024-01-15",
  updated: "2024-01-20",
  by: "John Doe",
  source: "https://example.com/source",
};

export default function Example() {
  const handleShare = () => {
    console.log("Share clicked!");
  };

  return (
    <div className="relative flex min-h-[600px] w-full items-center justify-center overflow-hidden">
      <ImageMetadataPreview
        alt="Mountain landscape"
        description="Beautiful mountain landscape with snow-capped peaks"
        filename="desert-canyon.jpg"
        imageSrc="data:image/webp;base64,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"
        metadata={sampleMetadata}
        onShare={handleShare}
      />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion react-use-measure
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
