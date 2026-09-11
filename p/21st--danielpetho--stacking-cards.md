<!-- Stacking Cards · @danielpetho · https://21st.dev/@danielpetho/components/stacking-cards
     license: MIT · category: scroll-area
     A scroll-driven stack of cards that pin and scale down on top of each other as the user scrolls, built with Motion. -->

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
components/ui/stacking-cards.tsx
// author: Khoa Phan <https://www.pldkhoa.dev>

"use client"

import {
  createContext,
  useContext,
  useRef,
  type HTMLAttributes,
  type PropsWithChildren,
} from "react"
import {
  motion,
  useScroll,
  useTransform,
  type MotionValue,
  type UseScrollOptions,
} from "motion/react"

import { cn } from "@/lib/utils"

interface StackingCardsProps
  extends PropsWithChildren,
    HTMLAttributes<HTMLDivElement> {
  scrollOptions?: UseScrollOptions
  scaleMultiplier?: number
  totalCards: number
}

interface StackingCardItemProps
  extends HTMLAttributes<HTMLDivElement>,
    PropsWithChildren {
  index: number
  topPosition?: string
}

export default function StackingCards({
  children,
  className,
  scrollOptions,
  scaleMultiplier,
  totalCards,
  ...props
}: StackingCardsProps) {
  const targetRef = useRef<HTMLDivElement>(null)
  const { scrollYProgress } = useScroll({
    offset: ["start start", "end end"],
    ...scrollOptions,
    target: targetRef,
  })

  return (
    <StackingCardsContext.Provider
      value={{ progress: scrollYProgress, scaleMultiplier, totalCards }}
    >
      <div className={cn(className)} ref={targetRef} {...props}>
        {children}
      </div>
    </StackingCardsContext.Provider>
  )
}

const StackingCardItem = ({
  index,
  topPosition,
  className,
  children,
  ...props
}: StackingCardItemProps) => {
  const {
    progress,
    scaleMultiplier,
    totalCards = 0,
  } = useStackingCardsContext() // Get from Context
  const scaleTo = 1 - (totalCards - index) * (scaleMultiplier ?? 0.03)
  const rangeScale = [index * (1 / totalCards), 1]
  const scale = useTransform(progress, rangeScale, [1, scaleTo])
  const top = topPosition ?? `${5 + index * 3}%`

  return (
    <div className={cn("h-full sticky top-0", className)} {...props}>
      <motion.div
        className={"origin-top relative h-full"}
        style={{ top, scale }}
      >
        {children}
      </motion.div>
    </div>
  )
}

const StackingCardsContext = createContext<{
  progress: MotionValue<number>
  scaleMultiplier?: number
  totalCards?: number
} | null>(null)

export const useStackingCardsContext = () => {
  const context = useContext(StackingCardsContext)
  if (!context)
    throw new Error("StackingCardItem must be used within StackingCards")
  return context
}

export { StackingCardItem }

demo.tsx
// author: Khoa Phan <https://www.pldkhoa.dev>

"use client";

import { useRef } from "react";
import Image from "next/image";

import { cn } from "@/lib/utils";
import StackingCards, {
  StackingCardItem,
} from "@/components/ui/stacking-cards";

const cards = [
  {
    bgColor: "bg-[#f97316]",
    title: "The Guiding Light",
    description:
      "Lighthouses have stood as beacons of hope for centuries, guiding sailors safely through treacherous waters. Their glowing light and towering presence serve as a reminder of humanity’s connection to the sea.",
    image:
      "https://cdn.21st.dev/assets/mirror/95/95f5a21210036f048d578de0d9fd2c9e7a50b04b8451e2c94d336b6a33d094c9.png",
  },
  {
    bgColor: "bg-[#0015ff]",
    title: "Life Beneath the Waves",
    description:
      "From shimmering schools of fish to solitary hunters, the ocean is home to an incredible variety of marine life. Each species plays a vital role in maintaining the balance of underwater ecosystems.",
    image:
      "https://cdn.21st.dev/assets/mirror/a9/a92f05671c1665afd455869e575c15963bfb8799697c99b7347a0ff18d26ce07.png",
  },
  {
    bgColor: "bg-[#ff5941]",
    title: "Alone on the Open Sea",
    description:
      "Drifting across the endless horizon, traveling alone on the sea is a test of courage and resilience. With nothing but the waves and the sky, solitude becomes both a challenge and a source of deep reflection.",
    image:
      "https://cdn.21st.dev/assets/mirror/93/930d1417a40ff0e454592ef7a0aec477296e6878f544d8d7a8c597cfb2ecad5e.png",
  },
  {
    bgColor: "bg-[#1f464d]",
    title: "The Art of Sailing",
    description:
      "Harnessing the power of the wind, sailing is both a skill and an adventure. Whether racing across the waves or leisurely cruising, it’s a timeless way to explore the vast blue expanse.",
    image:
      "https://cdn.21st.dev/assets/mirror/d4/d4cd55ca340e82fd3e576dacfef401d4afd83c468a9d7cb2dd34a31c5de1792b.png",
  },
  {
    bgColor: "bg-[#0015ff]",
    title: "The Era of Whaling",
    description:
      "Once a thriving industry, whale hunting shaped economies and cultures across the world. Today, efforts to protect these majestic creatures highlight the shift toward conservation and respect for marine life.",
    image:
      "https://cdn.21st.dev/assets/mirror/e2/e2174cb2f0dcbeac067aed4582be5620ecb72cc36c7859c68edc59017fb4e7e4.png",
  },
];

export default function StackingCardsDemo() {
  const container = useRef<HTMLDivElement>(null);

  return (
    <div
      className="h-[620px] bg-white overflow-auto text-white"
      ref={container}
    >
      <StackingCards
        totalCards={cards.length}
        scrollOptions={{ container: container }}
      >
        <div className="relative font-calendas h-[110px] w-full z-10 text-xl md:text-3xl font-bold uppercase flex justify-center items-center text-[#ff5941] whitespace-pre">
          Scroll down ↓
        </div>
        {cards.map(({ bgColor, description, image, title }, index) => {
          return (
            <StackingCardItem key={index} index={index} className="h-[620px]">
              <div
                className={cn(
                  bgColor,
                  "h-[80%] sm:h-[70%] flex-col sm:flex-row aspect-video px-8 py-10 flex w-11/12 rounded-3xl mx-auto relative",
                )}
              >
                <div className="flex-1 flex flex-col justify-center">
                  <h3 className="font-bold text-2xl mb-5">{title}</h3>
                  <p>{description}</p>
                </div>

                <div className="w-full sm:w-1/2 rounded-xl aspect-video relative overflow-hidden">
                  <Image
                    src={image}
                    alt={title}
                    className="object-cover"
                    fill
                  />
                </div>
              </div>
            </StackingCardItem>
          );
        })}

        <div className="w-full h-80 relative overflow-hidden">
          <h2 className="absolute bottom-0 left-0 translate-y-1/3 sm:text-[192px] text-[80px] text-[#ff5941] font-calendas">
            fancy
          </h2>
        </div>
      </StackingCards>
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
