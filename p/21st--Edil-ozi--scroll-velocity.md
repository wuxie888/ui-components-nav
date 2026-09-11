<!-- Scroll Velocity · @Edil-ozi · https://21st.dev/@Edil-ozi/components/scroll-velocity
     license: MIT · category: gallery
     A dynamic scrolling component that creates an infinite horizontal scroll effect, responding to vertical scroll velocity. The component can display both images and text in a continuous loop.

Features
- Smooth infinite horizontal scrolling
- Scroll velocity-based animation
- Support for both images and text content
- Customizable scroll direction and speed
- Responsive design
- Dark mode compatible -->

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
components/ui/scroll-velocity.tsx
"use client"

import * as React from "react"
import {
  motion,
  useAnimationFrame,
  useMotionValue,
  useScroll,
  useSpring,
  useTransform,
  useVelocity,
  wrap,
} from "framer-motion"

import { cn } from "@/lib/utils"

interface ScrollVelocityProps extends React.HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode
  velocity?: number
  movable?: boolean
  clamp?: boolean
}

const ScrollVelocity = React.forwardRef<HTMLDivElement, ScrollVelocityProps>(
  (
    {
      children,
      velocity = 5,
      movable = true,
      clamp = false,
      className,
      ...props
    },
    ref
  ) => {
    const baseX = useMotionValue(0)
    const { scrollY } = useScroll()
    const scrollVelocity = useVelocity(scrollY)
    const smoothVelocity = useSpring(scrollVelocity, {
      damping: 50,
      stiffness: 100,
    })
    const velocityFactor = useTransform(smoothVelocity, [0, 10000], [0, 5], {
      clamp,
    })

    const x = useTransform(baseX, (v) => `${wrap(0, -50, v)}%`)

    const directionFactor = React.useRef<number>(1)
    const scrollThreshold = React.useRef<number>(5)

    useAnimationFrame((_t, delta) => {
      if (movable) {
        move(delta)
      } else {
        if (Math.abs(scrollVelocity.get()) >= scrollThreshold.current) {
          move(delta)
        }
      }
    })

    function move(delta: number) {
      let moveBy = directionFactor.current * velocity * (delta / 1000)
      if (velocityFactor.get() < 0) {
        directionFactor.current = -1
      } else if (velocityFactor.get() > 0) {
        directionFactor.current = 1
      }
      moveBy += directionFactor.current * moveBy * velocityFactor.get()
      baseX.set(baseX.get() + moveBy)
    }

    return (
      <div
        ref={ref}
        className={cn(
          "relative m-0 flex flex-nowrap overflow-hidden whitespace-nowrap leading-[0.8] tracking-[-2px]",
          className
        )}
        {...props}
      >
        <motion.div
          className="flex flex-row flex-nowrap whitespace-nowrap text-xl font-semibold uppercase *:mr-6 *:block md:text-2xl xl:text-4xl"
          style={{ x }}
        >
          {typeof children === "string" ? (
            <>
              {Array.from({ length: 5 }).map((_, idx) => (
                <span key={idx}>{children}</span>
              ))}
            </>
          ) : (
            children
          )}
        </motion.div>
      </div>
    )
  }
)
ScrollVelocity.displayName = "ScrollVelocity"

export { ScrollVelocity }
export type { ScrollVelocityProps }

demo.tsx
"use client"

import { ScrollVelocity } from "@/components/ui/scroll-velocity"
import Image from "next/image"

const images = [
  {
    title: "Moonbeam",
    thumbnail: "https://cdn.21st.dev/assets/mirror/b4/b493a13f9145ec00ad856a1e4f957fb43cc9cc96b77d4083897dc61d53bc823e.jpg",
  },
  {
    title: "Cursor",
    thumbnail: "https://cdn.21st.dev/assets/mirror/11/11f69b7de2dc6933e8b291bbd959534c500a993c41c2a52bde7d1ca50c20d659.jpg",
  },
  {
    title: "Rogue",
    thumbnail: "https://cdn.21st.dev/assets/mirror/71/71299170ad9fee3c654ef7a73edc7e827a1688ee578545ad0b8096b5652750ce.jpg",
  },
  {
    title: "Editorially",
    thumbnail: "https://cdn.21st.dev/assets/mirror/5f/5fe0d32ab00ecf0bb8ac509cb9aafa2cc55fa7c54b8c905dcdfc946ff30936f7.jpg",
  },
  {
    title: "Editrix AI",
    thumbnail: "https://cdn.21st.dev/assets/mirror/55/55a9d54cba84561cab160b55f06311fcdab76cd5803ec6361bce19113596dd13.jpg",
  },
  {
    title: "Moonbeam 2",
    thumbnail: "https://cdn.21st.dev/assets/mirror/b4/b493a13f9145ec00ad856a1e4f957fb43cc9cc96b77d4083897dc61d53bc823e.jpg",
  },
  {
    title: "Cursor 2",
    thumbnail: "https://cdn.21st.dev/assets/mirror/11/11f69b7de2dc6933e8b291bbd959534c500a993c41c2a52bde7d1ca50c20d659.jpg",
  },
  {
    title: "Rogue 2",
    thumbnail: "https://cdn.21st.dev/assets/mirror/71/71299170ad9fee3c654ef7a73edc7e827a1688ee578545ad0b8096b5652750ce.jpg",
  },
  {
    title: "Editorially 2",
    thumbnail: "https://cdn.21st.dev/assets/mirror/5f/5fe0d32ab00ecf0bb8ac509cb9aafa2cc55fa7c54b8c905dcdfc946ff30936f7.jpg",
  },
  {
    title: "Editrix AI 2",
    thumbnail: "https://cdn.21st.dev/assets/mirror/55/55a9d54cba84561cab160b55f06311fcdab76cd5803ec6361bce19113596dd13.jpg",
  },
]

const velocity = [3, -3]

function ScrollVelocityDemo() {
  return (
    <div className="w-full">
      <div className="flex flex-col space-y-5 py-10">
        {velocity.map((v, index) => (
          <ScrollVelocity key={index} velocity={v}>
            {images.map(({ title, thumbnail }) => (
              <div
                key={title}
                className="relative h-[6rem] w-[9rem] md:h-[8rem] md:w-[12rem] xl:h-[12rem] xl:w-[18rem]"
              >
                <Image
                  src={thumbnail}
                  alt={title}
                  fill
                  className="h-full w-full object-cover object-center"
                />
              </div>
            ))}
          </ScrollVelocity>
        ))}
        <ScrollVelocity velocity={5}>You can also use a text!</ScrollVelocity>
      </div>
    </div>
  )
}

export { ScrollVelocityDemo }
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
