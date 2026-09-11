<!-- Interactive Image Selector · @educalvolpz · https://21st.dev/@educalvolpz/components/interactive-image-selector
     license: MIT · category: gallery
     An animated image gallery grid with a toggle selection mode for selecting, sharing, and deleting multiple photos. -->

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

import { Share2, Trash2 } from "lucide-react";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import { useCallback, useState } from "react";

const RESET_DELAY = 200;
const RESET_SCALE_START = 1;
const RESET_SCALE_PEAK = 1.1;
const RESET_SCALE_END = 1;
const RESET_ROTATE_START = 0;
const RESET_ROTATE_POSITIVE = 5;
const RESET_ROTATE_NEGATIVE = -5;
const SELECT_SCALE_START = 1;
const SELECT_SCALE_PEAK = 1.1;
const SELECT_SCALE_END = 1;
const SELECT_ROTATE_START = 0;
const SELECT_ROTATE_NEGATIVE = -5;
const SELECT_ROTATE_POSITIVE = 5;
const CONTAINER_SCALE_START = 1;
const CONTAINER_SCALE_MIN = 0.95;
const CONTAINER_SCALE_END = 1;
const ITEM_SCALE_START = 1;
const ITEM_SCALE_MIN = 0.9;
const ITEM_SCALE_END = 1;
const ITEM_ROTATE_START = 0;
const ITEM_ROTATE_POSITIVE = 2;
const ITEM_ROTATE_NEGATIVE = -2;
const RESET_ANIMATION_DURATION = 0.3;

export interface ImageData {
  id: number;
  src: string;
}

export interface InteractiveImageSelectorProps {
  className?: string;
  images: ImageData[];
  onChange?: (selected: number[]) => void;
  onDelete?: (deleted: number[]) => void;
  onShare?: (selected: number[]) => void;
  selectable?: boolean;
  selectedImages?: number[];
}

export default function InteractiveImageSelector({
  images,
  selectedImages: controlledSelected,
  onChange,
  onDelete,
  onShare,
  className = "",
  selectable = false,
}: InteractiveImageSelectorProps) {
  const [originalImages] = useState<ImageData[]>(images);
  const [internalImages, setInternalImages] = useState<ImageData[]>(images);
  const [internalSelected, setInternalSelected] = useState<number[]>([]);
  const [isSelecting, setIsSelecting] = useState(selectable);
  const [isResetting, setIsResetting] = useState(false);
  const shouldReduceMotion = useReducedMotion();

  const selected = controlledSelected ?? internalSelected;

  const handleImageClick = useCallback(
    (id: number) => {
      if (!isSelecting) {
        return;
      }
      const newSelected = selected.includes(id)
        ? selected.filter((imgId) => imgId !== id)
        : [...selected, id];
      if (onChange) {
        onChange(newSelected);
      } else {
        setInternalSelected(newSelected);
      }
    },
    [isSelecting, selected, onChange]
  );

  const handleDelete = useCallback(() => {
    const newImages = internalImages.filter(
      (img) => !selected.includes(img.id)
    );
    if (onDelete) {
      onDelete(selected);
    }
    setInternalImages(newImages);
    if (onChange) {
      onChange([]);
    } else {
      setInternalSelected([]);
    }
  }, [selected, internalImages, onDelete, onChange]);

  const handleReset = useCallback(() => {
    setIsResetting(true);

    // Add a small delay to show the reset animation
    setTimeout(() => {
      setInternalImages(originalImages);
      if (onChange) {
        onChange([]);
      } else {
        setInternalSelected([]);
      }
      setIsSelecting(false);
      setIsResetting(false);
    }, RESET_DELAY);
  }, [originalImages, onChange]);

  const toggleSelecting = useCallback(() => {
    setIsSelecting((prev) => !prev);
    if (isSelecting) {
      if (onChange) {
        onChange([]);
      } else {
        setInternalSelected([]);
      }
    }
  }, [isSelecting, onChange]);

  const handleShare = useCallback(() => {
    if (onShare) {
      onShare(selected);
    }
  }, [onShare, selected]);

  return (
    <div
      className={`relative flex h-full w-full max-w-[500px] flex-col justify-between p-4 ${className}`}
    >
      <div className="pointer-events-none absolute inset-x-0 top-0 z-10 h-28 bg-linear-to-b from-primary/80 to-transparent dark:from-background/50" />
      <div className="absolute top-5 right-5 left-5 z-20 flex justify-between p-4">
        <motion.button
          animate={
            shouldReduceMotion || !isResetting
              ? {}
              : {
                  rotate: [
                    RESET_ROTATE_START,
                    RESET_ROTATE_POSITIVE,
                    RESET_ROTATE_NEGATIVE,
                    RESET_ROTATE_START,
                  ],
                  scale: [RESET_SCALE_START, RESET_SCALE_PEAK, RESET_SCALE_END],
                }
          }
          aria-label="Reset selection"
          className={`cursor-pointer rounded-full px-3 py-1 font-semibold text-sm bg-blend-luminosity backdrop-blur-xl transition-colors ${
            isResetting
              ? "bg-brand/30 text-white"
              : "bg-background/20 text-foreground"
          }`}
          disabled={isResetting}
          exit={shouldReduceMotion ? {} : { rotate: 0 }}
          initial={shouldReduceMotion ? {} : { rotate: 0 }}
          onClick={handleReset}
          transition={shouldReduceMotion ? { duration: 0 } : { duration: 0.25 }}
          whileHover={shouldReduceMotion ? {} : { scale: 1.05 }}
          whileTap={shouldReduceMotion ? {} : { scale: 0.95 }}
        >
          {isResetting ? "Resetting..." : "Reset"}
        </motion.button>
        <motion.button
          animate={
            isSelecting
              ? {
                  rotate: [
                    SELECT_ROTATE_START,
                    SELECT_ROTATE_NEGATIVE,
                    SELECT_ROTATE_POSITIVE,
                    SELECT_ROTATE_START,
                  ],
                  scale: [
                    SELECT_SCALE_START,
                    SELECT_SCALE_PEAK,
                    SELECT_SCALE_END,
                  ],
                }
              : {}
          }
          aria-label={isSelecting ? "Cancel selection" : "Select images"}
          className={`cursor-pointer rounded-full px-3 py-1 font-semibold text-sm bg-blend-luminosity backdrop-blur-xl ${
            isSelecting
              ? "bg-brand/30 text-white"
              : "bg-background/20 text-foreground"
          }`}
          exit={{ rotate: 0 }}
          initial={{ rotate: 0 }}
          onClick={toggleSelecting}
          transition={{ duration: 0.3 }}
          type="button"
          whileHover={{ scale: 1.05 }}
          whileTap={{ scale: 0.95 }}
        >
          {isSelecting ? "Cancel" : "Select"}
        </motion.button>
      </div>

      <motion.div
        animate={
          shouldReduceMotion || !isResetting
            ? {}
            : {
                scale: [
                  CONTAINER_SCALE_START,
                  CONTAINER_SCALE_MIN,
                  CONTAINER_SCALE_END,
                ],
              }
        }
        className="grid grid-cols-3 gap-1 overflow-scroll"
        layout={!shouldReduceMotion}
        transition={shouldReduceMotion ? { duration: 0 } : { duration: 0.2 }}
      >
        <AnimatePresence>
          {internalImages.map((img) => (
            <motion.div
              animate={{
                opacity: 1,
                rotate: isResetting
                  ? [
                      ITEM_ROTATE_START,
                      ITEM_ROTATE_POSITIVE,
                      ITEM_ROTATE_NEGATIVE,
                      ITEM_ROTATE_START,
                    ]
                  : 0,
                scale: isResetting
                  ? [ITEM_SCALE_START, ITEM_SCALE_MIN, ITEM_SCALE_END]
                  : 1,
              }}
              className="relative aspect-square cursor-pointer"
              exit={{ opacity: 0, scale: 0.8 }}
              initial={{ opacity: 0, scale: 0.8 }}
              key={img.id}
              layout
              onClick={() => handleImageClick(img.id)}
              transition={{
                damping: 25,
                duration: isResetting ? RESET_ANIMATION_DURATION : undefined,
                stiffness: 300,
                type: "spring" as const,
              }}
            >
              <img
                alt={`Gallery item ${img.id}`}
                className={`h-full w-full rounded-lg object-cover ${
                  selected.includes(img.id) && isSelecting ? "opacity-75" : ""
                }`}
                draggable={false}
                height={200}
                loading="lazy"
                src={img.src}
                width={200}
              />
              {isSelecting && selected.includes(img.id) && (
                <div className="absolute right-2 bottom-2 flex h-6 w-6 items-center justify-center rounded-full border border-white bg-brand text-white">
                  ✓
                </div>
              )}
            </motion.div>
          ))}
        </AnimatePresence>
      </motion.div>
      <AnimatePresence>
        {isSelecting ? (
          <motion.div
            animate={shouldReduceMotion ? { opacity: 1 } : { opacity: 1, y: 0 }}
            className="absolute right-2 bottom-0 left-1/2 z-10 flex w-2/3 -translate-x-1/2 items-center justify-between rounded-full bg-background/20 p-4 bg-blend-luminosity backdrop-blur-md"
            exit={
              shouldReduceMotion
                ? { opacity: 0, transition: { duration: 0 } }
                : { opacity: 0, y: 20 }
            }
            initial={
              shouldReduceMotion ? { opacity: 1 } : { opacity: 0, y: 20 }
            }
            transition={
              shouldReduceMotion ? { duration: 0 } : { duration: 0.25 }
            }
          >
            <button
              className="cursor-pointer text-brand"
              onClick={handleShare}
              type="button"
            >
              <Share2 size={24} />
            </button>
            <span className="text-foreground">{selected.length} selected</span>
            <button
              className="cursor-pointer text-brand"
              disabled={selected.length === 0}
              onClick={handleDelete}
              type="button"
            >
              <Trash2 size={24} />
            </button>
          </motion.div>
        ) : null}
      </AnimatePresence>
    </div>
  );
}

demo.tsx
"use client";

import InteractiveImageSelector, {
  type ImageData,
} from "@/components/ui/interactive-image-selector";
import { useEffect, useState } from "react";

const demoImages: ImageData[] = [
  {
    id: 1,
    src: "https://cdn.21st.dev/assets/mirror/25/25ee6ad6cc8d34653e9e52a438ed8eb100c3533f59e8eb3a4863a1254d4863f5.jpg",
  },
  {
    id: 2,
    src: "https://cdn.21st.dev/assets/mirror/2e/2e0581aa07c04e8c0db328b221f13bd151e71c87f7e64f4d93046a6cc0e6a4a4.jpg",
  },
  {
    id: 3,
    src: "https://cdn.21st.dev/assets/mirror/3b/3b12f4f0d9160101b532af0a11ce695bb338b9a828f2f1c2e3473c344c7af33e.jpg",
  },
  {
    id: 4,
    src: "https://cdn.21st.dev/assets/mirror/39/39130fc742c7995b212f63fffcafe2177371ce2aadc44e1cc64669d268f48cdd.jpg",
  },
  {
    id: 5,
    src: "https://cdn.21st.dev/assets/mirror/bd/bd93e2bb7edc0ace09495a3ade8690a69562d53a8582a561f179f65fc91c6fd8.jpg",
  },
  {
    id: 6,
    src: "https://cdn.21st.dev/assets/mirror/41/41ebb031313d81fea56184cfde2954b3e4c2921b596e05964f439aeac9e5d617.jpg",
  },
];

const InteractiveImageSelectorDemo = () => {
  const [selected, setSelected] = useState<number[]>([]);
  const [images, setImages] = useState<ImageData[]>(demoImages);
  const [notification, setNotification] = useState<string | null>(null);

  useEffect(() => {
    if (notification) {
      const timer = setTimeout(() => setNotification(null), 3000);
      return () => clearTimeout(timer);
    }
  }, [notification]);

  return (
    <div className="relative flex h-[520px] w-full items-stretch justify-center bg-background p-4">
      {notification && (
        <div className="absolute top-4 right-4 z-50 rounded-lg border bg-background px-4 py-2 text-sm text-foreground shadow-lg">
          {notification}
        </div>
      )}
      <InteractiveImageSelector
        images={images}
        onChange={setSelected}
        onDelete={(deleted) =>
          setImages((imgs) => imgs.filter((img) => !deleted.includes(img.id)))
        }
        onShare={(sharedImages) =>
          setNotification(`Share images: ${sharedImages.join(", ")}`)
        }
        selectable={false}
        selectedImages={selected}
      />
    </div>
  );
};

export default InteractiveImageSelectorDemo;
```

Install NPM dependencies:
```bash
npm install lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add tokens.json
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
