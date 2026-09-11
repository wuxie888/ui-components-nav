<!-- cta section with gallery · @youcefbnm · https://21st.dev/@youcefbnm/components/cta-section-with-gallery
     license: MIT · category: hero
     call to action section with stagger animation and image gallery -->

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
components/blocks/cta-section-with-gallery.tsx
"use client"

import * as React from "react"
import { motion, type HTMLMotionProps, type Transition } from "motion/react"

import { cn } from "@/lib/utils"

const SPRING_CONFIG: Transition = {
  type: "spring",
  stiffness: 100,
  damping: 16,
  mass: 0.75,
  restDelta: 0.005,
}

const blurVariants = {
  hidden: {
    opacity: 0,
    filter: "blur(10px)",
  },
  visible: {
    opacity: 1,
    filter: "blur(0px)",
  },
}

const GRID_LAYOUT = [
  "col-start-2 col-end-3 row-start-1 row-end-3",
  "col-start-1 col-end-2 row-start-2 row-end-4",
  "col-start-1 col-end-2 row-start-4 row-end-6",
  "col-start-2 col-end-3 row-start-3 row-end-5",
]

export const ContainerStagger = React.forwardRef<
  HTMLDivElement,
  HTMLMotionProps<"div">
>(({ transition, ...props }, ref) => {
  return (
    <motion.div
      ref={ref}
      initial="hidden"
      whileInView="visible"
      viewport={{ once: true }}
      transition={{
        staggerChildren: transition?.staggerChildren ?? 0.2,
        delayChildren: transition?.delayChildren ?? 0.2,
        duration: 0.3,
        ...transition,
      }}
      {...props}
    />
  )
})
ContainerStagger.displayName = "ContainerStagger"

export const ContainerAnimated = React.forwardRef<
  HTMLDivElement,
  HTMLMotionProps<"div">
>(({ transition, ...props }, ref) => {
  return (
    <motion.div
      ref={ref}
      variants={blurVariants}
      transition={{
        ...SPRING_CONFIG,
        duration: 0.3,
        ...transition,
      }}
      {...props}
    />
  )
})
ContainerAnimated.displayName = "ContainerAnimated"

export const GalleryGrid = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => {
  return (
    <div
      ref={ref}
      className={cn(
        "grid grid-cols-2 grid-rows-[50px_150px_50px_150px_50px] gap-4",
        className
      )}
      {...props}
    />
  )
})
// NOTE: the bundle sets displayName to "ContainerSticky" for the GalleryGrid export; preserved as-is.
GalleryGrid.displayName = "ContainerSticky"

interface GalleryGridCellProps extends HTMLMotionProps<"div"> {
  index: number
}

export const GalleryGridCell = React.forwardRef<
  HTMLDivElement,
  GalleryGridCellProps
>(({ className, transition, index, ...props }, ref) => {
  return (
    <motion.div
      ref={ref}
      initial={{ opacity: 0 }}
      whileInView={{ opacity: 1 }}
      viewport={{ once: true }}
      transition={{
        duration: 0.3,
        delay: index * 0.2,
        delayChildren: transition?.delayChildren ?? 0.2,
      }}
      className={`relative overflow-hidden rounded-xl shadow-xl ${GRID_LAYOUT[index]}`}
      {...props}
    />
  )
})
GalleryGridCell.displayName = "GalleryGridCell"

demo.tsx
import {   ContainerAnimated,
  ContainerStagger,
  GalleryGrid,
  GalleryGridCell,
 } from "@/components/blocks/cta-section-with-gallery"
import { Button } from "@/components/ui/button" 

const IMAGES = [
  "https://cdn.21st.dev/assets/mirror/b4/b489547220e05476a6e9252ee770ceaefcfc04983b0c588116335d048ce1e2c7.jpg",
  "https://cdn.21st.dev/assets/mirror/19/195fc68ab1924d31b9603423fec192334039d7aca6537f284f280ac18b7fd579.jpg",
  "https://cdn.21st.dev/assets/mirror/5e/5e6145c62a3957eaff5285a8fd9aeaad17459c835ae6792f6ce48df1b7c093c4.jpg",
  "https://cdn.21st.dev/assets/mirror/44/44fd2efacd2545fadbf51b1fde849b93c0f1df3bb63e51b3f58989765a0d301f.jpg",
]

export const AboutDemo = () => {
  return (
    <section>
      <div className="mx-auto grid w-full max-w-6xl grid-cols-1 items-center gap-8 px-8 py-12 md:grid-cols-2">
        <ContainerStagger>
          <ContainerAnimated className="mb-4 block text-xs font-medium text-rose-500 md:text-sm">
            Innovate & Grow
          </ContainerAnimated>
          <ContainerAnimated className="text-4xl font-semibold md:text-[2.4rem] tracking-tight">
            Scale Your Business Through Innovation
          </ContainerAnimated>
          <ContainerAnimated className="my-4 text-base text-slate-700 md:my-6 md:text-lg">
            Transform your startup&apos;s potential through innovative solutions
            and strategic growth. We help businesses adapt, evolve, and thrive
            in today&apos;s competitive marketplace.
          </ContainerAnimated>
          <ContainerAnimated>
            <Button className=" bg-rose-500 ">Start Scaling Today</Button>
          </ContainerAnimated>
        </ContainerStagger>

        <GalleryGrid>
          {IMAGES.map((imageUrl, index) => (
            <GalleryGridCell index={index} key={index}>
              <img
                className="size-full object-cover object-center"
                width="100%"
                height="100%"
                src={imageUrl}
                alt=""
              />
            </GalleryGridCell>
          ))}
        </GalleryGrid>
      </div>
    </section>
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
