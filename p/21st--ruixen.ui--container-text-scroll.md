<!-- Container Text Scroll · @ruixen.ui · https://21st.dev/@ruixen.ui/components/container-text-scroll
     license: unspecified · category: hero
     The ContainerTextScroll component is a dynamic React/Next.js UI element designed to create visually engaging scroll-based animations for text content. It leverages framer-motion to animate the text’s position, scale, and tilt as the user scrolls, creating a smooth parallax-like effect. The component is fully responsive, adjusting animation parameters for mobile and desktop screens, and allows placing titles or paragraphs directly over images or backgrounds. It is ideal for hero sections, landing pages, or any interface where immersive, interactive scrolling experiences are desired. By combining motion, scaling, and rotation effects, ContainerTextScroll helps developers deliver modern, eye-catching UI with minimal setup. -->

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
components/ui/container-text-scroll.tsx
"use client";
import React, { useRef } from "react";
import { useScroll, useTransform, motion, MotionValue } from "motion/react";

export const ContainerTextScroll = ({
  titleComponent,
  children,
}: {
  titleComponent: string | React.ReactNode;
  children: React.ReactNode;
}) => {
  const containerRef = useRef<HTMLDivElement>(null);
  const { scrollYProgress } = useScroll({ target: containerRef });

  const [isMobile, setIsMobile] = React.useState(false);
  React.useEffect(() => {
    const checkMobile = () => setIsMobile(window.innerWidth <= 768);
    checkMobile();
    window.addEventListener("resize", checkMobile);
    return () => window.removeEventListener("resize", checkMobile);
  }, []);

  // Scale from smaller to full size as we scroll
  const scaleDimensions = () => (isMobile ? [0.8, 1] : [0.9, 1]);

  // Tilt forward from bottom: start 20°, end 0° (straight)
  const rotate = useTransform(scrollYProgress, [0, 1], [-20, 0]);
  const scale = useTransform(scrollYProgress, [0, 1], scaleDimensions());
  const translateY = useTransform(scrollYProgress, [0, 1], [100, 0]); // moves up
  const titleTranslateY = useTransform(scrollYProgress, [0, 1], [100, 0]);
  const titleScale = useTransform(scrollYProgress, [0, 1], [0.8, 1]);

  return (
    <div
      ref={containerRef}
      className="h-[60rem] md:h-[80rem] flex items-center justify-center relative px-8"
    >
      <motion.div
        style={{ translateY }}
        className="relative w-full max-w-5xl flex flex-col items-center justify-center"
      >
        {/* Card */}
        <Card rotate={rotate} scale={scale}>
          {children}

          {/* Title inside card */}
          <motion.div
            style={{ translateY: titleTranslateY, scale: titleScale }}
            className="absolute inset-0 flex items-center justify-center text-center px-4"
          >
            <div className="text-white">{titleComponent}</div>
          </motion.div>
        </Card>
      </motion.div>
    </div>
  );
};

export const Card = ({
  rotate,
  scale,
  children,
}: {
  rotate: MotionValue<number>;
  scale: MotionValue<number>;
  children: React.ReactNode;
}) => {
  return (
    <motion.div
      style={{
        rotateX: rotate,
        scale,
        boxShadow:
          "0 0 #0000004d, 0 9px 20px #0000004a, 0 37px 37px #00000042, 0 84px 50px #00000026, 0 149px 60px #0000000a, 0 233px 65px #00000003",
      }}
      className="relative h-[30rem] md:h-[40rem] w-full border-4 border-[#6C6C6C] bg-[#222222] rounded-[30px] shadow-2xl overflow-hidden"
    >
      <div className="h-full w-full rounded-2xl bg-gray-100 dark:bg-zinc-900">
        {children}
      </div>
    </motion.div>
  );
};

demo.tsx
"use client";
import React from "react";
import { ContainerTextScroll } from "@/components/ui/container-text-scroll";
import Image from "next/image";

export default function ContainerTextScrollDemo() {
  return (
    <div className="flex flex-col overflow-hidden pb-[300px]">
      <ContainerTextScroll
        titleComponent={
          <>
            <h1 className="text-4xl font-semibold text-white">
              Explore the realm of <br />
              <span className="text-7xl md:text-[8rem] font-bold mt-1 leading-none">
                Infinite Patterns
              </span>
              <br />
              <a
                href="https://www.ruixen.com/"
                target="_blank"
                rel="noopener noreferrer"
                className="block mt-4 text-2xl font-medium text-gray-300 hover:text-white transition-colors"
              >
                Ruixen UI
              </a>
            </h1>
          </>
        }
      >
        <Image
          src={`https://cdn.21st.dev/assets/mirror/61/6138ba1bccd2ae7cc89a81c656e36febe6fb4a3166232accf821861e8f8d86a1.jpg`}
          alt="hero"
          height={720}
          width={1400}
          className="mx-auto rounded-2xl object-cover h-full object-left-top"
          draggable={false}
        />
      </ContainerTextScroll>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion motion
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
