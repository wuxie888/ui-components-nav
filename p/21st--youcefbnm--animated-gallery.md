<!-- Animated Gallery · @youcefbnm · https://21st.dev/@youcefbnm/components/animated-gallery
     license: MIT · category: hero
     Animated Gallery with scroll trigger animation -->

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
components/blocks/animated-gallery.tsx
"use client"

import * as React from "react"
import {
  HTMLMotionProps,
  MotionValue,
  motion,
  Transition,
  useScroll,
  useTransform,
  Variants,
} from "motion/react"

import { cn } from "@/lib/utils"

const SPRING_CONFIG: Transition = {
  type: "spring",
  stiffness: 100,
  damping: 16,
  mass: 0.75,
  restDelta: 0.005,
  duration: 0.3,
}

const blurVariants: Variants = {
  hidden: {
    filter: "blur(10px)",
    opacity: 0,
  },
  visible: {
    filter: "blur(0px)",
    opacity: 1,
  },
}

interface ContainerScrollContextValue {
  scrollYProgress: MotionValue<number>
}

const ContainerScrollContext = React.createContext<
  ContainerScrollContextValue | undefined
>(undefined)

function useContainerScrollContext() {
  const context = React.useContext(ContainerScrollContext)
  if (!context) {
    throw new Error(
      "useContainerScrollContext must be used within a ContainerScroll Component"
    )
  }
  return context
}

export const ContainerScroll: React.FC<React.HTMLAttributes<HTMLDivElement>> = ({
  children,
  className,
  style,
  ...props
}) => {
  const scrollRef = React.useRef<HTMLDivElement>(null)
  const { scrollYProgress } = useScroll({
    target: scrollRef,
  })
  return (
    <ContainerScrollContext.Provider value={{ scrollYProgress }}>
      <div
        ref={scrollRef}
        className={cn("relative min-h-[120vh]", className)}
        style={{
          perspective: "1000px",
          perspectiveOrigin: "center top",
          transformStyle: "preserve-3d",
          ...style,
        }}
        {...props}
      >
        {children}
      </div>
    </ContainerScrollContext.Provider>
  )
}
ContainerScroll.displayName = "ContainerScroll"

export const ContainerSticky: React.FC<React.HTMLAttributes<HTMLDivElement>> = ({
  className,
  style,
  ...props
}) => {
  return (
    <div
      className={cn(
        "sticky left-0 top-0 min-h-[30rem] w-full overflow-hidden",
        className
      )}
      style={{
        perspective: "1000px",
        perspectiveOrigin: "center top",
        transformStyle: "preserve-3d",
        transformOrigin: "50% 50%",
        ...style,
      }}
      {...props}
    />
  )
}
ContainerSticky.displayName = "ContainerSticky"

export const GalleryContainer: React.FC<HTMLMotionProps<"div">> = ({
  children,
  className,
  style,
  ...props
}) => {
  const { scrollYProgress } = useContainerScrollContext()
  const rotateX = useTransform(scrollYProgress, [0, 0.5], [75, 0])
  const scale = useTransform(scrollYProgress, [0.5, 0.9], [1.2, 1])

  return (
    <motion.div
      className={cn(
        "relative grid size-full grid-cols-3 gap-2 rounded-2xl",
        className
      )}
      style={{
        rotateX,
        scale,
        transformStyle: "preserve-3d",
        perspective: "1000px",
        ...style,
      }}
      {...props}
    >
      {children}
    </motion.div>
  )
}
GalleryContainer.displayName = "GalleryContainer"

interface GalleryColProps extends HTMLMotionProps<"div"> {
  yRange?: string[]
}

export const GalleryCol: React.FC<GalleryColProps> = ({
  className,
  style,
  yRange = ["0%", "-10%"],
  ...props
}) => {
  const { scrollYProgress } = useContainerScrollContext()
  const y = useTransform(scrollYProgress, [0.5, 1], yRange)

  return (
    <motion.div
      className={cn("relative flex w-full flex-col gap-2 ", className)}
      style={{
        y,
        ...style,
      }}
      {...props}
    />
  )
}
GalleryCol.displayName = "GalleryCol"

export const ContainerStagger = React.forwardRef<
  HTMLDivElement,
  HTMLMotionProps<"div">
>(({ className, viewport, transition, ...props }, ref) => {
  return (
    <motion.div
      className={cn("relative", className)}
      ref={ref}
      initial="hidden"
      whileInView="visible"
      viewport={{ once: true, ...viewport }}
      transition={{
        staggerChildren: transition?.staggerChildren || 0.2,
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
>(({ className, transition, ...props }, ref) => {
  return (
    <motion.div
      ref={ref}
      className={cn(className)}
      variants={blurVariants}
      // NOTE: bundle reads `SPRING_CONFIG || transition`, so the spring config always wins; kept as-is.
      transition={SPRING_CONFIG || transition}
      {...props}
    />
  )
})
ContainerAnimated.displayName = "ContainerAnimated"

demo.tsx
import { ContainerAnimated,
  ContainerScroll,
  ContainerStagger,
  ContainerSticky,
  GalleryCol,
  GalleryContainer } from "@/components/blocks/animated-gallery"
import { Button } from "@/components/ui/button"
import { VideoIcon } from "lucide-react"

const IMAGES_1 = [
  "https://cdn.21st.dev/assets/mirror/db/db8e72b6f6e2f325ec74898fdab6a02f3c0ba7962f3cf0b89f0ee3b22aa2a083.jpg",
  "https://cdn.21st.dev/assets/mirror/77/777c9bd220f0c47c9eb699ebbd77fb0c6c9a8d8b0cd77f089bab93a18586d578.jpg",
  "https://cdn.21st.dev/assets/mirror/f9/f992831c368ea7e12c51417be55fda812d1502e9bb6730d94bc6b1e0c6a2ae57.jpg",
  "https://cdn.21st.dev/assets/mirror/8c/8c0a104646b9b9d6680c2222cf84cfd3e4e10ada11fd0d0759093db9a69cfd3d.jpg",
]
const IMAGES_2 = [
  "https://cdn.21st.dev/assets/mirror/4e/4eb85747c8113c6edcbec2671a5aa4e62d0569488ad75652c16dd7598a1196e1.jpg",
  "https://cdn.21st.dev/assets/mirror/ab/ab1fd4fd007ecad2ad9a5350341b1013589f05f8f30b8fdd4a35728a800e9fce.jpg",
  "https://cdn.21st.dev/assets/mirror/4d/4de1f4952d0420f95ade25fc723d8042ece00762429cdccb79fd3a29ffe5f33d.jpg",
  "https://cdn.21st.dev/assets/mirror/53/53f281293f06536d7f60b1786b0a39404390a55e1675e99e089d2b72234cf8cb.jpg",
]
const IMAGES_3 = [
  "https://cdn.21st.dev/assets/mirror/35/358a63f0c4cb478488bdf1bfb90ed5fd785c28bcddbf058b4a7287fe8ed75fff.jpg",
  "https://cdn.21st.dev/assets/mirror/e8/e81126a3c16766e36ed84d2226b0b11507e86b999d4d07cd7e88c0f04e14c0eb.jpg",
  "https://cdn.21st.dev/assets/mirror/77/777c9bd220f0c47c9eb699ebbd77fb0c6c9a8d8b0cd77f089bab93a18586d578.jpg",
  "https://cdn.21st.dev/assets/mirror/85/85a98f8253097be06cfdaa5a312c644d4df00749aad17dda4468ab8d3dce7bd0.jpg",
]

export const DemoVariant1 = () => {
  return (
    <div className="relative bg-white ">
      <ContainerStagger className="relative z-[9999] -mb-12 place-self-center px-6 pt-12 text-center">
        <ContainerAnimated>
          <h1 className="font-serif text-4xl font-extralight  md:text-5xl">
            Your{" "}
            <span className=" font-serif font-extralight text-indigo-600">
              one source
            </span>
          </h1>
        </ContainerAnimated>
        <ContainerAnimated>
          <h1 className="font-serif text-4xl font-extralight md:text-5xl">
            for all your designs
          </h1>
        </ContainerAnimated>

        <ContainerAnimated className="my-4">
          <p className="leading-normal tracking-tight text-muted-foreground">
            No waste of time and money, we provide you with
            <br /> collection of designs to plan your next project.
          </p>
        </ContainerAnimated>

        <ContainerAnimated>
          <Button
            className="gap-1 bg-indigo-700"
          >
            Book free call <VideoIcon className="size-4  " />
          </Button>
          <Button variant={"link"} className="text-sencondary">
            About Us
          </Button>
        </ContainerAnimated>
      </ContainerStagger>
      <div className="pointer-events-none absolute z-10 h-[70vh] w-full "
      style={{
            background: "linear-gradient(to right, gray, rebeccapurple, blue)",
            filter: "blur(84px)",
            mixBlendMode: "screen",
          }}
      />

      <ContainerScroll className="relative h-[350vh]">
        <ContainerSticky className="h-svh">
          <GalleryContainer className="">
            <GalleryCol yRange={["-10%", "2%"]} className="-mt-2">
              {IMAGES_1.map((imageUrl, index) => (
                // eslint-disable-next-line @next/next/no-img-element
                <img
                  key={index}
                  className="aspect-video block h-auto max-h-full w-full  rounded-md  object-cover shadow"
                  src={imageUrl}
                  alt="gallery item"
                />
              ))}
            </GalleryCol>
            <GalleryCol className="mt-[-50%]" yRange={["15%", "5%"]}>
              {IMAGES_2.map((imageUrl, index) => (
                // eslint-disable-next-line @next/next/no-img-element
                <img
                  key={index}
                  className="aspect-video block h-auto max-h-full w-full  rounded-md  object-cover shadow"
                  src={imageUrl}
                  alt="gallery item"
                />
              ))}
            </GalleryCol>
            <GalleryCol yRange={["-10%", "2%"]} className="-mt-2">
              {IMAGES_3.map((imageUrl, index) => (
                // eslint-disable-next-line @next/next/no-img-element
                <img
                  key={index}
                  className="aspect-video block h-auto max-h-full w-full  rounded-md  object-cover shadow"
                  src={imageUrl}
                  alt="gallery item"
                />
              ))}
            </GalleryCol>
          </GalleryContainer>
        </ContainerSticky>
      </ContainerScroll>
    </div>
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
