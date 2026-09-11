<!-- 3D Flip Card · @ayushmxxn · https://21st.dev/@ayushmxxn/components/3d-flip-card
     license: MIT · category: gallery
     An interactive card stack component with 3D hover effects and animations. Perfect for showcasing images, portfolios, or featured content.

Features
- Smooth 3D rotation effects
- Responsive design
- Interactive card selection
- Customizable card dimensions
- Configurable stack spacing
- Mobile-friendly fallback animations
- Blur effects for depth perception -->

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
components/serenity/flip-card-3d.tsx
"use client";

import {
  motion,
  type TargetAndTransition,
  type Transition,
} from "framer-motion";
import Image from "next/image";
import React, { useEffect, useState } from "react";

// Replace these with your own images
const DEFAULT_IMAGES = [
  {
    src: "https://images.pexels.com/photos/4588065/pexels-photo-4588065.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=2",
    alt: "Sunset landscape",
  },
  {
    src: "https://images.pexels.com/photos/321552/pexels-photo-321552.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=2",
    alt: "Desert scene",
  },
  {
    src: "https://images.pexels.com/photos/208821/pexels-photo-208821.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=2",
    alt: "Red building",
  },
  {
    src: "https://images.pexels.com/photos/33550/cows-curious-cattle-agriculture.jpg?auto=compress&cs=tinysrgb&w=600",
    alt: "Cactus close-up",
  },
  {
    src: "https://images.pexels.com/photos/70568/spotted-baumwaran-monitor-tree-monitor-lizard-70568.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=2",
    alt: "Ocean view",
  },
];

// Card size — small and photo-like, no frame around them
const CARD_WIDTH_CLASS = "w-64 sm:w-72";
const CARD_HEIGHT_CLASS = "h-32 sm:h-36";

// How the stack peeks out from behind the front card before you even hover it.
// Each card behind sits a little further out and a little smaller, like a real pile of photos
const REST_PEEK_X = 7; // how far each card behind shifts sideways
const REST_PEEK_Y = 7; // how far each card behind shifts down
const REST_PEEK_SCALE_STEP = 0.025; // how much smaller each card behind gets
const REST_PEEK_ROTATIONS = [0, -3, 4, -2.5, 3]; // slight tilt per card, index 0 is the front one

// How much the whole stack rises when you hover it, so nothing gets clipped by the page
const FAN_LIFT_DESKTOP = -50;
const FAN_LIFT_MOBILE = -40;

// Gap between cards once they fan out
const FAN_SPREAD_X = 45; // sideways gap on desktop
const FAN_SPREAD_Y_MOBILE = 42; // downward gap on mobile

// How high a card rises when you click it to bring it fully to front
const POP_LIFT_DESKTOP = -130;
const POP_LIFT_MOBILE = -110;
const POP_SCALE = 1.18;

// Spring feel for each state. Higher stiffness snaps quicker, higher damping settles smoother.
// POP_SPRING carries extra `mass` so the click-to-center motion feels like it has real
// physical weight landing in place, rather than a light, snappy bounce.
const REST_SPRING: Transition = { type: "spring", stiffness: 300, damping: 22 };
const FAN_SPRING: Transition = { type: "spring", stiffness: 280, damping: 26 };
const POP_SPRING: Transition = {
  type: "spring",
  stiffness: 180,
  damping: 24,
  mass: 1.6,
};

// Shadows modeled on light falling from above. They stay soft and low opacity at rest,
// and only grow bigger and softer as a card lifts further off the stack, like in real life
const SHADOW_REST = "0 2px 6px rgba(0,0,0,0.06), 0 1px 2px rgba(0,0,0,0.04)";
const SHADOW_POPPED =
  "0 30px 45px rgba(0,0,0,0.16), 0 10px 15px rgba(0,0,0,0.08)";
const fannedShadow = (depth: number) =>
  `0 ${6 + depth * 2}px ${16 + depth * 3}px rgba(0,0,0,${0.07 + depth * 0.015})`;

const MOBILE_BREAKPOINT = 768;

interface FlipImage {
  src: string;
  alt: string;
}

interface CardProps {
  image: FlipImage;
  index: number;
  isStackHovered: boolean;
  isMobile: boolean;
  isFront: boolean;
  anyCardIsFront: boolean;
  onClick: (index: number) => void;
}

const Card: React.FC<CardProps> = ({
  image,
  index,
  isStackHovered,
  isMobile,
  isFront,
  anyCardIsFront,
  onClick,
}) => {
  const fanLift = isMobile ? FAN_LIFT_MOBILE : FAN_LIFT_DESKTOP;
  const popLift = isMobile ? POP_LIFT_MOBILE : POP_LIFT_DESKTOP;
  const peekRotation = REST_PEEK_ROTATIONS[index] ?? 0;

  // Each state is explicitly typed as `TargetAndTransition` so all three share one
  // consistent shape (including the optional `rotateY` field that only `fannedState`
  // and `poppedState` use). Without this, TS infers three structurally different
  // object types and the union assigned to `animate` no longer matches what
  // `motion.div` expects, which is the root cause of the original error.
  // While another card is popped to center, every non-front card dims down (in
  // addition to the existing blur) so the focused card visually "pops" against a
  // faded backdrop, matching the reference image.
  const backgroundOpacity = anyCardIsFront && !isFront ? 0.35 : 1;

  const restState: TargetAndTransition = {
    rotate: isMobile ? 0 : peekRotation,
    x: index * REST_PEEK_X,
    y: index * REST_PEEK_Y,
    scale: 1 - index * REST_PEEK_SCALE_STEP,
    opacity: backgroundOpacity,
    boxShadow: SHADOW_REST,
    transition: { ...REST_SPRING, delay: (4 - index) * 0.06 },
  };

  const fannedState: TargetAndTransition = {
    rotate: 0,
    rotateY: isMobile ? 0 : -45,
    x: isMobile ? 0 : index * FAN_SPREAD_X,
    y: (isMobile ? index * FAN_SPREAD_Y_MOBILE : index * -6) + fanLift,
    scale: 1.04,
    opacity: backgroundOpacity,
    boxShadow: fannedShadow(index),
    transition: { ...FAN_SPRING, delay: index * 0.07 },
  };

  const poppedState: TargetAndTransition = {
    rotate: 0,
    rotateY: 0,
    x: 0,
    y: popLift,
    scale: POP_SCALE,
    opacity: 1,
    boxShadow: SHADOW_POPPED,
    transition: POP_SPRING,
  };

  const animate: TargetAndTransition = isFront
    ? poppedState
    : isStackHovered
      ? fannedState
      : restState;

  return (
    <motion.div
      className={`absolute ${CARD_WIDTH_CLASS} ${CARD_HEIGHT_CLASS} rounded-2xl overflow-hidden cursor-pointer ${
        isFront ? "z-20" : ""
      }`}
      style={{
        transformStyle: "preserve-3d",
        transformOrigin: isMobile ? "top center" : "left center",
        zIndex: isFront ? 20 : 5 - index,
        filter: isFront || !anyCardIsFront ? "none" : "blur(5px)",
      }}
      initial={restState}
      animate={animate}
      onClick={() => onClick(index)}
    >
      <Image
        src={image.src}
        alt={image.alt}
        fill
        className="object-cover rounded-2xl"
        sizes="(max-width: 640px) 256px, 288px"
      />
    </motion.div>
  );
};

interface FlipCard3DProps {
  images?: FlipImage[];
}

export const FlipCard3D: React.FC<FlipCard3DProps> = ({
  images = DEFAULT_IMAGES,
}) => {
  const [isStackHovered, setIsStackHovered] = useState(false);
  const [isMobile, setIsMobile] = useState(false);
  const [frontIndex, setFrontIndex] = useState<number | null>(null);

  useEffect(() => {
    const checkScreenSize = () =>
      setIsMobile(window.innerWidth <= MOBILE_BREAKPOINT);
    checkScreenSize();
    window.addEventListener("resize", checkScreenSize);
    return () => window.removeEventListener("resize", checkScreenSize);
  }, []);

  const handleCardClick = (index: number) => {
    setFrontIndex((current) => (current === index ? null : index));
  };

  // Leaving the stack area closes a popped card AND un-fans the rest, in one place.
  // This is more reliable than listening on the popped card's own mouse-leave,
  // since that element is mid-animation (scaling/translating) right after a click —
  // tracking the whole stack's hover area avoids relying on a moving hitbox.
  const handleStackMouseLeave = () => {
    setIsStackHovered(false);
    setFrontIndex(null);
  };

  return (
    <div className="flex justify-center items-center py-24 sm:py-28 select-none">
      <div
        className={`relative ${CARD_WIDTH_CLASS} ${CARD_HEIGHT_CLASS}`}
        style={{ perspective: 1000 }}
        onMouseEnter={() => setIsStackHovered(true)}
        onMouseLeave={handleStackMouseLeave}
      >
        {images.map((image, index) => (
          <Card
            key={image.src}
            image={image}
            index={index}
            isStackHovered={isStackHovered}
            isMobile={isMobile}
            isFront={frontIndex === index}
            anyCardIsFront={frontIndex !== null}
            onClick={handleCardClick}
          />
        ))}
      </div>
    </div>
  );
};

export default FlipCard3D;

demo.tsx
'use client'

import { CardStack3D } from "@/components/ui/3d-flip-card"

export function CardStackDemo() {
  const images = [
    { 
      src: "https://cdn.21st.dev/assets/mirror/64/6425a7d62b05ac5d76c6d1179965a954ca820a1b548a111555800022eae582a1.jpg", 
      alt: "Monkey" 
    },
    { 
      src: "https://cdn.21st.dev/assets/mirror/f6/f602867d9d5796339a34d88e037860ffeb5c335dbb2b97495ed653858f3a6be5.jpg", 
      alt: "Donkey" 
    },
    { 
      src: "https://cdn.21st.dev/assets/mirror/25/2529e2d1d0276520e267c1d53887ac768389b00cd4aa7a9cc74889e064099229.jpg", 
      alt: "Cow" 
    },
    { 
      src: "https://cdn.21st.dev/assets/mirror/38/38f27cee78085da514f7e26c5089117b13b1b4ca1113716108278f608ff7a1f8.jpg", 
      alt: "Chameleon" 
    },
  ]

  return (
    <div className="min-h-screen flex items-center justify-center">
      <CardStack3D 
        images={images}
        cardWidth={320}
        cardHeight={192}
        spacing={{ x: 50, y: 50 }}
      />
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
