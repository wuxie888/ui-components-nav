<!-- Hero Video · @youcefbnm · https://21st.dev/@youcefbnm/components/hero-video
     license: mpl-2.0 · category: hero
     ✔️ ContainerScroll component props: accepts react html attributes
✔️ ContainerStagger component: staggers/delayed the animated children by default <<transition staggerChildren is set to 0.2>> accepts html motion props
✔️ ContainerAnimated component: (⚠️ to get orchestrated animations make sure to make it inside <ContainerStagger /> component) accepts "animation" prop define the animation set type: "left" | "right" | "top" | "bottom" | "z" | "blur" | undefined (set to opacity by default), accepts html motion props
✔️ ContainerInset component:  {
  translateYRange?: [string, string] 👉 default:["-25%", "50%"]
  insetYRange?: [number, number] 👉 default: [35, 0]
  insetXRange?: [number, number] 👉 default:👉 default :[35, 0]
  roundednessRange?: [number, number] 👉 default: [1000, 16]
}
⚠️ <ContainerInset /> can be used only inside <ContainerScroll /> component -->

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
'use client';
import { HTMLMotionProps, motion } from 'motion/react';
import * as React from 'react';

interface ContainerStaggerProps extends HTMLMotionProps<'div'> {
  staggerChildren?: number;
  delayChildren?: number;
  staggerDirection?: 1 | -1;
}

export const ContainerStagger = React.forwardRef<
  HTMLDivElement,
  ContainerStaggerProps
>(
  (
    {
      staggerChildren = 0.2,
      delayChildren = 0.2,
      staggerDirection = 1,
      className,
      transition,
      ...props
    },
    ref,
  ) => {
    return (
      <motion.div
        ref={ref}
        className={className}
        initial="hidden"
        whileInView="visible"
        viewport={{ once: true }}
        transition={{
          staggerChildren,
          delayChildren,
          staggerDirection,
          ...transition,
        }}
        {...props}
      />
    );
  },
);
ContainerStagger.displayName = 'ContainerStagger';

demo.tsx
import { ContainerAnimated,
  ContainerInset,
  ContainerScroll,
  ContainerStagger, } from "@/components/blocks/hero-video"
import { Button } from "@/components/ui/button"

const DemoVariant1 = () => {
  return (
    <ContainerScroll className="bg-stone-100 text-center text-stone-800">
      <ContainerStagger viewport={{ once: false }}>
        <ContainerAnimated animation="top">
          <h1 className=" text-4xl font-black leading-none tracking-tighter">
            Wait! what is this?,
          </h1>
        </ContainerAnimated>
        <ContainerAnimated animation="bottom">
          <h1 className=" text-4xl font-black leading-none tracking-tighter">
            Scroll and see.
          </h1>
        </ContainerAnimated>

        <ContainerAnimated animation="blur" className="my-4">
          <p className=" text-xl leading-normal tracking-tight">
            🎯 One component with diffrent animations,
            <br />
            ✨ using &quot;animation&quot; prop to animate the component.,
            <br />
            🪄 opacity(default) top bottom left right blur z.
          </p>
        </ContainerAnimated>

        <ContainerAnimated
          animation="blur"
          className="flex justify-center gap-2"
        >
          <Button
            variant={"secondary"}
            className="rounded-full bg-stone-500 text-white hover:bg-stone-400"
            size="lg"
          >
            Get Started
          </Button>
          <Button
            variant={"outline"}
            className="rounded-full border-stone-500 text-stone-600 hover:bg-stone-300"
            size="lg"
          >
            Learn more
          </Button>
        </ContainerAnimated>
      </ContainerStagger>

      <ContainerInset className=" mx-8">
        <video
          width="100%"
          height="100%"
          loop
          playsInline
          autoPlay
          muted
          className="relative z-10 block h-auto max-h-full max-w-full object-contain align-middle"
        >
          <source
            src="https://cdn.21st.dev/assets/mirror/8f/8face727d1504c6ccebd873ec9ce02cd34fd7c8b5be6833f410168115a377b6b.mp4"
            type="video/mp4"
          />
        </video>
      </ContainerInset>
    </ContainerScroll>
  )
}

const DemoVariant2 = () => {
  return (
    <ContainerScroll
      style={{
        background: "#434343",
        backgroundImage: "linear-gradient(to right, #434343 0%, #000 100%)",
      }}
      className="text-center text-stone-100"
    >
      <ContainerStagger>
        <ContainerAnimated animation="top">
          <h1 className=" text-4xl font-black leading-none tracking-tighter">
            Wait! what is this?,
          </h1>
        </ContainerAnimated>
        <ContainerAnimated animation="top">
          <h1 className=" text-4xl font-black leading-none tracking-tighter">
            Scroll and see.
          </h1>
        </ContainerAnimated>

        <ContainerAnimated animation="z" className="my-4">
          <p className=" text-xl leading-normal tracking-tight">
            🎯 One component with diffrent animations,
            <br />
            ✨ using &quot;animation&quot; prop to animate the component.,
            <br />
            🪄 opacity(default) top bottom left right blur z.
          </p>
        </ContainerAnimated>

        <ContainerAnimated
          animation="bottom"
          className="flex justify-center gap-2"
        >
          <Button
            variant={"secondary"}
            className="rounded-full bg-indigo-400 text-white hover:bg-indigo-300"
            size="lg"
          >
            Get Started
          </Button>
          <Button
            variant={"outline"}
            className="rounded-full border-indigo-300 text-indigo-300 bg-transparent hover:bg-indigo-200"
            size="lg"
          >
            Learn more
          </Button>
        </ContainerAnimated>
      </ContainerStagger>

      <ContainerInset insetXRange={[30, 0]} className=" mx-8">
        <video
          width="100%"
          height="100%"
          loop
          playsInline
          autoPlay
          muted
          className="relative z-10 block h-auto max-h-full max-w-full object-contain align-middle"
        >
          <source
            src="https://videos.pexels.com/video-files/8086707/8086707-uhd_2560_1440_25fps.mp4"
            type="video/mp4"
          />
        </video>
      </ContainerInset>
    </ContainerScroll>
  )
}
 
export { DemoVariant1, DemoVariant2 }
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
