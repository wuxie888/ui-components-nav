<!-- Number Ticker · @danielpetho · https://21st.dev/@danielpetho/components/basic-number-ticker
     license: MIT · category: stat
     An animated number counter that smoothly tweens from a starting value to a target, with ref-based control to trigger the count on demand. -->

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
components/ui/basic-number-ticker.tsx
"use client"

import {
  forwardRef,
  useCallback,
  useEffect,
  useImperativeHandle,
  useState,
} from "react"
import {
  animate,
  AnimationPlaybackControls,
  motion,
  useMotionValue,
  useTransform,
  ValueAnimationTransition,
} from "motion/react"

import { cn } from "@/lib/utils"

interface NumberTickerProps {
  from: number // Starting value of the animation
  target: number // End value of the animation
  transition?: ValueAnimationTransition // Animation configuration, refer to motion docs for more details
  className?: string // additionl CSS classes for styling
  onStart?: () => void // Callback function when animation starts
  onComplete?: () => void // Callback function when animation completes
  autoStart?: boolean // Whether to start the animation automatically
}

// Ref interface to allow external control of the animation
export interface NumberTickerRef {
  startAnimation: () => void
}

const NumberTicker = forwardRef<NumberTickerRef, NumberTickerProps>(
  (
    {
      from = 0,
      target = 100,
      transition = {
        duration: 3,
        type: "tween",
        ease: "easeInOut",
      },
      className,
      onStart,
      onComplete,
      autoStart = true,
      ...props
    },
    ref
  ) => {
    const count = useMotionValue(from)
    const rounded = useTransform(count, (latest) => Math.round(latest))
    const [controls, setControls] = useState<AnimationPlaybackControls | null>(
      null
    )

    // Function to start the animation
    const startAnimation = useCallback(() => {
      if (controls) controls.stop()
      onStart?.()

      count.set(from)

      const newControls = animate(count, target, {
        ...transition,
        onComplete: () => {
          onComplete?.()
        },
      })
      setControls(newControls)
    }, [])

    // Expose the startAnimation function via ref
    useImperativeHandle(ref, () => ({
      startAnimation,
    }))

    useEffect(() => {
      if (autoStart) {
        startAnimation()
      }
      return () => controls?.stop()
    }, [autoStart])

    return (
      <motion.span className={cn(className)} {...props}>
        {rounded}
      </motion.span>
    )
  }
)

NumberTicker.displayName = "NumberTicker"

export default NumberTicker

// Usage example:
// To start the animation from outside the component:
// 1. Create a ref:
//    const tickerRef = useRef<NumberTickerRef>(null);
// 2. Pass the ref to the NumberTicker component:
//    <NumberTicker ref={tickerRef} from={0} target={100} autoStart={false} />
// 3. Call the startAnimation function:
//    tickerRef.current?.startAnimation();

demo.tsx
"use client";

import NumberTicker, {
  NumberTickerRef,
} from "@/components/ui/basic-number-ticker";
import { useEffect, useRef } from "react";
import {
  Activity,
  ArrowDownRight,
  DollarSign,
  LucideIcon,
  TrendingUp,
  Zap,
} from "lucide-react";
import { motion, useInView } from "motion/react";

const cards = [
  {
    title: "Revenue",
    icon: DollarSign,
    from: 0,
    target: 1250321,
    prefix: "$",
    suffix: "",
    gradient: "from-gray-100 to-blue-400",
    size: "large",
  },
  {
    title: "Conversion Rate",
    icon: TrendingUp,
    from: 0,
    target: 12.5,
    prefix: "",
    suffix: "%",
    gradient: "from-gray-100 to-purple-200",
    size: "small",
  },
  {
    title: "Bounce Rate",
    icon: ArrowDownRight,
    from: 100,
    target: 35.8,
    prefix: "",
    suffix: "%",
    gradient: "from-gray-100 to-orange-200",
    size: "small",
  },
  {
    title: "Avg. Session Duration",
    icon: Zap,
    from: 0,
    target: 245,
    prefix: "",
    suffix: "s",
    gradient: "from-gray-100 to-purple-200",
    size: "small",
  },
  {
    title: "New Users",
    icon: TrendingUp,
    from: 0,
    target: 15420,
    prefix: "",
    suffix: "",
    gradient: "from-gray-100 to-orange-200",
    size: "small",
  },
  {
    title: "Active Users",
    icon: Activity,
    from: 0,
    target: 8750,
    prefix: "",
    suffix: "",
    gradient: "from-gray-100 to-blue-200",
    size: "small",
  },
];

interface CardProps {
  title: string;
  icon: LucideIcon;
  from: number;
  target: number;
  prefix: string;
  suffix: string;
  gradient: string;
  size: string;
}

const Card = ({ card, index }: { card: CardProps; index: number }) => {
  const cardRef = useRef<HTMLDivElement>(null);
  const tickerRef = useRef<NumberTickerRef>(null);
  const inView = useInView(cardRef, { once: false });

  useEffect(() => {
    if (inView) {
      tickerRef.current?.startAnimation();
    }
  }, [inView]);

  return (
    <motion.div
      ref={cardRef}
      initial={{ opacity: 0, y: 10 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.5, delay: index * 0.1 }}
      className={`p-6 bg-linear-to-b from-50% to-130% flex justify-between flex-col text-foreground dark:text-muted ${card.gradient} ${card.size === "large" ? "col-span-2 row-span-2" : ""}`}
    >
      <div className="flex justify-between items-center mb-4">
        <h3 className="text-xs md:text-sm">{card.title}</h3>
        <card.icon className={`h-4 w-4`} />
      </div>
      <div
        className={`${card.size === "large" ? "text-2xl md:text-5xl" : "text-xl md:text-3xl"}`}
      >
        {card.prefix}
        <NumberTicker
          ref={tickerRef}
          from={card.from}
          target={card.target}
          transition={{
            duration: 3,
            ease: "easeInOut",
            type: "tween",
            delay: index * 0.2,
          }}
          className="tabular-nums"
          autoStart={false}
        />
        {card.suffix}
      </div>
    </motion.div>
  );
};

export default function FancyNumberTickerDemo() {
  return (
    <div className="w-full h-full min-h-[500px] font-mono bg-background">
      <div className="grid grid-cols-3 grid-rows-2 h-full">
        {cards.map((card, index) => (
          <Card key={index} card={card} index={index} />
        ))}
      </div>
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
