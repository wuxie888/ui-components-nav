<!-- Text Animate · cult-ui · https://www.cult-ui.com/docs/components/text-animate
     license: MIT · category: text
     Animated text component with customizable reveal effects and timing -->

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
components/ui/text-animate.tsx
"use client"

import { FC, useEffect, useRef } from "react"
import { HTMLMotionProps, motion, useAnimation, useInView } from "motion/react"

type AnimationType =
  | "fadeIn"
  | "fadeInUp"
  | "popIn"
  | "shiftInUp"
  | "rollIn"
  | "whipIn"
  | "whipInUp"
  | "calmInUp"

interface Props extends HTMLMotionProps<"div"> {
  text: string
  type?: AnimationType
  delay?: number
  duration?: number
}

const animationVariants = {
  fadeIn: {
    container: {
      hidden: { opacity: 0 },
      visible: (i: number = 1) => ({
        opacity: 1,
        transition: { staggerChildren: 0.05, delayChildren: i * 0.3 },
      }),
    },
    child: {
      visible: {
        opacity: 1,
        y: 0,
        transition: {
          type: "spring" as const,
          damping: 12,
          stiffness: 100,
        },
      },
      hidden: { opacity: 0, y: 10 },
    },
  },
  fadeInUp: {
    container: {
      hidden: { opacity: 0 },
      visible: {
        opacity: 1,
        transition: { staggerChildren: 0.1, delayChildren: 0.2 },
      },
    },
    child: {
      visible: { opacity: 1, y: 0, transition: { duration: 0.5 } },
      hidden: { opacity: 0, y: 20 },
    },
  },
  popIn: {
    container: {
      hidden: { scale: 0 },
      visible: {
        scale: 1,
        transition: { staggerChildren: 0.05, delayChildren: 0.2 },
      },
    },
    child: {
      visible: {
        opacity: 1,
        scale: 1.1,
        transition: { type: "spring" as const, damping: 15, stiffness: 400 },
      },
      hidden: { opacity: 0, scale: 0 },
    },
  },
  calmInUp: {
    container: {
      hidden: {},
      visible: (i: number = 1) => ({
        transition: { staggerChildren: 0.01, delayChildren: 0.2 * i },
      }),
    },
    child: {
      hidden: {
        y: "200%",
        transition: {
          ease: [0.455, 0.03, 0.515, 0.955] as const,
          duration: 0.85,
        },
      },
      visible: {
        y: 0,
        transition: {
          ease: [0.125, 0.92, 0.69, 0.975] as const, //  Drawing attention to dynamic content or interactive elements, where the animation needs to be engaging but not abrupt
          duration: 0.75,
          //   ease: [0.455, 0.03, 0.515, 0.955], // smooth and gradual acceleration followed by a steady deceleration towards the end of the animation
          //   ease: [0.115, 0.955, 0.655, 0.939], // smooth and gradual acceleration followed by a steady deceleration towards the end of the animation
          //   ease: [0.09, 0.88, 0.68, 0.98], // Very Gentle Onset, Swift Mid-Section, Soft Landing
          //   ease: [0.11, 0.97, 0.64, 0.945], // Minimal Start, Energetic Acceleration, Smooth Closure
        },
      },
    },
  },
  shiftInUp: {
    container: {
      hidden: {},
      visible: (i: number = 1) => ({
        transition: { staggerChildren: 0.01, delayChildren: 0.2 * i },
      }),
    },
    child: {
      hidden: {
        y: "100%", // Starting from below but not too far to ensure a dramatic but manageable shift.
        transition: {
          ease: [0.75, 0, 0.25, 1] as const, // Starting quickly
          duration: 0.6, // Shortened duration for a more dramatic start
        },
      },
      visible: {
        y: 0,
        transition: {
          duration: 0.8, // Slightly longer to accommodate the slow middle and swift end
          ease: [0.22, 1, 0.36, 1] as const, // This easing function starts quickly (dramatic shift), slows down (slow middle), and ends quickly (clean swift end)
        },
      },
    },
  },

  whipInUp: {
    container: {
      hidden: {},
      visible: (i: number = 1) => ({
        transition: { staggerChildren: 0.01, delayChildren: 0.2 * i },
      }),
    },
    child: {
      hidden: {
        y: "200%",
        transition: {
          ease: [0.455, 0.03, 0.515, 0.955] as const,
          duration: 0.45,
        },
      },
      visible: {
        y: 0,
        transition: {
          ease: [0.5, -0.15, 0.25, 1.05] as const,
          duration: 0.75,
        },
      },
    },
  },
  rollIn: {
    container: {
      hidden: {},
      visible: {},
    },
    child: {
      hidden: {
        opacity: 0,
        y: `0.25em`,
      },
      visible: {
        opacity: 1,
        y: `0em`,
        transition: {
          duration: 0.65,
          ease: [0.65, 0, 0.75, 1] as const, // Great! Swift Beginning, Prolonged Ease, Quick Finish
          //   ease: [0.75, 0.05, 0.85, 1], // Quick Start, Smooth Middle, Sharp End
          //   ease: [0.7, -0.25, 0.9, 1.25], // Fast Acceleration, Gentle Slowdown, Sudden Snap
          //   ease: [0.7, -0.5, 0.85, 1.5], // Quick Leap, Soft Glide, Snappy Closure
        },
      },
    },
  },
  whipIn: {
    container: {
      hidden: {},
      visible: {},
    },
    child: {
      hidden: {
        opacity: 0,
        y: `0.35em`,
      },
      visible: {
        opacity: 1,
        y: `0em`,
        transition: {
          duration: 0.45,
          //   ease: [0.75, 0.05, 0.85, 1], // Quick Start, Smooth Middle, Sharp End
          //   ease: [0.7, -0.25, 0.9, 1.25], // Fast Acceleration, Gentle Slowdown, Sudden Snap
          //   ease: [0.65, 0, 0.75, 1], // Great! Swift Beginning, Prolonged Ease, Quick Finish
          ease: [0.85, 0.1, 0.9, 1.2] as const, // Rapid Initiation, Subtle Slow, Sharp Conclusion
        },
      },
    },
  },
}

const TextAnimate: FC<Props> = ({
  text,
  type = "whipInUp",
  ...props
}: Props) => {
  //   const { ref, inView } = useInView({
  //     threshold: 0.5,
  //     triggerOnce: true,
  //   });

  const ref = useRef(null)
  const isInView = useInView(ref, { once: true })

  const letters = Array.from(text)
  const { container, child } = animationVariants[type]

  const ctrls = useAnimation()

  //   useEffect(() => {
  //     if (isInView) {
  //       ctrls.start("visible");
  //     }
  //     if (!isInView) {
  //       ctrls.start("hidden");
  //     }
  //   }, [ctrls, isInView]);

  if (type === "rollIn" || type === "whipIn") {
    return (
      <h2 className="mt-10 text-3xl font-black text-black dark:text-neutral-100 py-5 pb-8 px-8 md:text-5xl">
        {text.split(" ").map((word, index) => {
          return (
            <motion.span
              ref={ref}
              className="inline-block mr-[0.25em] whitespace-nowrap"
              aria-hidden="true"
              key={index}
              initial="hidden"
              animate="visible"
              variants={container}
              transition={{
                delayChildren: index * 0.13,
                // delayChildren: index * 0.35,
                staggerChildren: 0.025,
                // staggerChildren: 0.05,
              }}
            >
              {word.split("").map((character, index) => {
                return (
                  <motion.span
                    aria-hidden="true"
                    key={index}
                    variants={child}
                    className="inline-block -mr-[0.01em]"
                  >
                    {character}
                  </motion.span>
                )
              })}
            </motion.span>
          )
        })}
      </h2>
    )
  }

  return (
    <motion.h2
      style={{ display: "flex", overflow: "hidden" }}
      role="heading"
      variants={container}
      initial="hidden"
      animate="visible"
      className="mt-10 text-4xl font-black text-black dark:text-neutral-100 py-5 pb-8 px-8 md:text-5xl"
      {...props}
    >
      {letters.map((letter, index) => (
        <motion.span key={index} variants={child}>
          {letter === " " ? "\u00A0" : letter}
        </motion.span>
      ))}
    </motion.h2>
  )
}

export { TextAnimate }
export default TextAnimate

demo.tsx
"use client"

import { useRef, useState } from "react"
import { motion, useInView } from "motion/react"

import { FadeIn } from "@/components/fade-in"

import TextAnimate from "../ui/text-animate"

// @ts-ignore
const AnimationDemo = ({ type, children }) => {
  const ref = useRef(null)
  const isInView = useInView(ref, { once: true })
  const [count, setCount] = useState(0)

  return (
    <div className="flex flex-col relative" ref={ref}>
      <div className="mt-8">
        <FadeIn key={count}>{children}</FadeIn>
      </div>
      <div className="absolute top-0 -right-5 md:right-5">
        <Refresh onClick={() => setCount(count + 1)} />
      </div>
      <div className="absolute bottom-0 -left-5 md:hidden">
        <Refresh onClick={() => setCount(count + 1)} />
      </div>
    </div>
  )
}

const button = {
  rest: { scale: 1 },
  hover: { scale: 1.1 },
  pressed: { scale: 0.95 },
}
const arrow = {
  rest: { rotate: 0 },
  hover: { rotate: 360, transition: { duration: 0.4 } },
}

// @ts-ignore
const Refresh = ({ onClick }) => {
  return (
    <motion.div
      className="p-1 border border-dotted rounded w-7 h-7  flex justify-center items-center cursor-pointer"
      onClick={onClick}
      variants={button}
      initial="rest"
      whileHover="hover"
      whileTap="pressed"
    >
      <motion.svg
        width="16"
        height="16"
        xmlns="http://www.w3.org/2000/svg"
        variants={arrow}
      >
        <path
          d="M12.8 5.1541V2.5a.7.7 0 0 1 1.4 0v5a.7.7 0 0 1-.7.7h-5a.7.7 0 0 1 0-1.4h3.573c-.7005-1.8367-2.4886-3.1-4.5308-3.1C4.8665 3.7 2.7 5.85 2.7 8.5s2.1665 4.8 4.8422 4.8c1.3035 0 2.523-.512 3.426-1.4079a.7.7 0 0 1 .986.9938C10.7915 14.0396 9.2186 14.7 7.5422 14.7 4.0957 14.7 1.3 11.9257 1.3 8.5s2.7957-6.2 6.2422-6.2c2.1801 0 4.137 1.1192 5.2578 2.8541z"
          className="fill-black dark:fill-white"
          fillRule="nonzero"
        />
      </motion.svg>
    </motion.div>
  )
}

export default function TextAnimationDemo() {
  return (
    <AnimationDemo type="TextAnimate">
      <div>
        <div className="flex flex-col">
          <div className="flex flex-col py-4">
            <div className="grid grid-cols-1 md:grid-cols-2">
              <div>
                <p className="text-sm font-semibold">Roll In</p>
                <div className="md:mt-12">
                  <TextAnimate text="Roll In" type="rollIn" />
                </div>
              </div>

              <div>
                <p className="text-sm font-semibold">Whip In</p>
                <div className="md:mt-12">
                  <TextAnimate
                    text="Whip In"
                    type="whipIn"
                    className="text-lg"
                  />
                </div>
              </div>
              <div>
                <p className="text-sm font-semibold">fade In</p>
                <div className="md:mt-12">
                  <TextAnimate text="Fade In" type="fadeIn" />
                </div>
              </div>

              <div>
                <p className="text-sm font-semibold">Pop In</p>
                <div className="md:mt-12">
                  <TextAnimate text="Pop In" type="popIn" />
                </div>
              </div>

              <div>
                <p className="text-sm font-semibold">Fade In Up</p>
                <div className="md:mt-12">
                  <TextAnimate text="Fade In Up" type="fadeInUp" />
                </div>
              </div>

              <div>
                <p className="text-sm font-semibold">Shift In Up</p>
                <div className="md:mt-12">
                  <TextAnimate text="Shift In Up" type="shiftInUp" />
                </div>
              </div>

              <div>
                <p className="text-sm font-semibold">Whip In Up</p>
                <div className="md:mt-12">
                  <TextAnimate text="Whip In Up" type="whipInUp" />
                </div>
              </div>

              <div>
                <p className="text-sm font-semibold">Calm In Up</p>
                <div className="md:mt-12">
                  <TextAnimate text="Calm In Up" type="calmInUp" />
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </AnimationDemo>
  )
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
