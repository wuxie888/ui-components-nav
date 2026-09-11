<!-- Parallax Scroll · Aceternity UI · https://ui.aceternity.com/components/parallax-scroll
     license: MIT · category: gallery
     A multi-column parallax scrolling image grid for React. Columns move at different speeds as the page scrolls. Built with Tailwind CSS and Motion. -->

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
components/ui/parallax-scroll.tsx
"use client";
import { useScroll, useTransform } from "motion/react";
import { useRef } from "react";
import { motion } from "motion/react";

import { cn } from "@/lib/utils";

export const ParallaxScroll = ({
  images,
  className,
}: {
  images: string[];
  className?: string;
}) => {
  const gridRef = useRef<any>(null);
  const { scrollYProgress } = useScroll({
    container: gridRef, // remove this if your container is not fixed height
    offset: ["start start", "end start"], // remove this if your container is not fixed height
  });

  const translateFirst = useTransform(scrollYProgress, [0, 1], [0, -200]);
  const translateSecond = useTransform(scrollYProgress, [0, 1], [0, 200]);
  const translateThird = useTransform(scrollYProgress, [0, 1], [0, -200]);

  const third = Math.ceil(images.length / 3);

  const firstPart = images.slice(0, third);
  const secondPart = images.slice(third, 2 * third);
  const thirdPart = images.slice(2 * third);

  return (
    <div
      className={cn("h-[40rem] items-start overflow-y-auto w-full", className)}
      ref={gridRef}
    >
      <div
        className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 items-start  max-w-5xl mx-auto gap-10 py-40 px-10"
        ref={gridRef}
      >
        <div className="grid gap-10">
          {firstPart.map((el, idx) => (
            <motion.div
              style={{ y: translateFirst }} // Apply the translateY motion value here
              key={"grid-1" + idx}
            >
              <img
                src={el}
                className="h-80 w-full object-cover object-left-top rounded-lg gap-10 !m-0 !p-0"
                height="400"
                width="400"
                alt="thumbnail"
              />
            </motion.div>
          ))}
        </div>
        <div className="grid gap-10">
          {secondPart.map((el, idx) => (
            <motion.div style={{ y: translateSecond }} key={"grid-2" + idx}>
              <img
                src={el}
                className="h-80 w-full object-cover object-left-top rounded-lg gap-10 !m-0 !p-0"
                height="400"
                width="400"
                alt="thumbnail"
              />
            </motion.div>
          ))}
        </div>
        <div className="grid gap-10">
          {thirdPart.map((el, idx) => (
            <motion.div style={{ y: translateThird }} key={"grid-3" + idx}>
              <img
                src={el}
                className="h-80 w-full object-cover object-left-top rounded-lg gap-10 !m-0 !p-0"
                height="400"
                width="400"
                alt="thumbnail"
              />
            </motion.div>
          ))}
        </div>
      </div>
    </div>
  );
};

demo.tsx
"use client";
import { ParallaxScrollSecond } from "@/components/ui/parallax-scroll";

export function ParallaxScrollSecondDemo() {
  return <ParallaxScrollSecond images={images} />;
}

const images = [
  "https://cdn.21st.dev/assets/mirror/38/3824fce0f86b8c7b341a2a8541dd58e4bfe54ce40608dc3ff8716fe152740d68.jpg",
  "https://cdn.21st.dev/assets/mirror/0a/0a753d1d2c89c21c1936a906b7570a6ace0bb6e98e703d4b6e7a8ba6ca60ea9e.jpg",
  "https://cdn.21st.dev/assets/mirror/62/623fd1531de4b4116b5ccb9e26eaa9ffc27395a79c0d280ca6db8a692ce6720c.jpg",
  "https://cdn.21st.dev/assets/mirror/38/3824fce0f86b8c7b341a2a8541dd58e4bfe54ce40608dc3ff8716fe152740d68.jpg",
  "https://cdn.21st.dev/assets/mirror/0a/0a753d1d2c89c21c1936a906b7570a6ace0bb6e98e703d4b6e7a8ba6ca60ea9e.jpg",
  "https://cdn.21st.dev/assets/mirror/62/623fd1531de4b4116b5ccb9e26eaa9ffc27395a79c0d280ca6db8a692ce6720c.jpg",
  "https://cdn.21st.dev/assets/mirror/c6/c6e571a103f505fd959c7828bbc50452b0571806a9ae6b1d6c7b2200966eb165.jpg",
  "https://cdn.21st.dev/assets/mirror/ba/baf57bef7738ac27ff3fb5715e536e40f9b9b04cc5fbb83713df1ad31a2578ef.jpg",
  "https://cdn.21st.dev/assets/mirror/69/69513e8df47a4a11cd4522e21a473f4f568454df87e2ca28f17e403ce281e76e.jpg",
  "https://cdn.21st.dev/assets/mirror/c3/c30cc786bc1b22c2e4c4f12e4ab819c61d8e8e80269e3df4596c7b684ffe865d.jpg",
  "https://cdn.21st.dev/assets/mirror/38/3824fce0f86b8c7b341a2a8541dd58e4bfe54ce40608dc3ff8716fe152740d68.jpg",
  "https://cdn.21st.dev/assets/mirror/0a/0a753d1d2c89c21c1936a906b7570a6ace0bb6e98e703d4b6e7a8ba6ca60ea9e.jpg",
  "https://cdn.21st.dev/assets/mirror/62/623fd1531de4b4116b5ccb9e26eaa9ffc27395a79c0d280ca6db8a692ce6720c.jpg",
  "https://cdn.21st.dev/assets/mirror/38/3824fce0f86b8c7b341a2a8541dd58e4bfe54ce40608dc3ff8716fe152740d68.jpg",
  "https://cdn.21st.dev/assets/mirror/0a/0a753d1d2c89c21c1936a906b7570a6ace0bb6e98e703d4b6e7a8ba6ca60ea9e.jpg",
  "https://cdn.21st.dev/assets/mirror/38/3824fce0f86b8c7b341a2a8541dd58e4bfe54ce40608dc3ff8716fe152740d68.jpg",
  "https://cdn.21st.dev/assets/mirror/0a/0a753d1d2c89c21c1936a906b7570a6ace0bb6e98e703d4b6e7a8ba6ca60ea9e.jpg",
  "https://cdn.21st.dev/assets/mirror/62/623fd1531de4b4116b5ccb9e26eaa9ffc27395a79c0d280ca6db8a692ce6720c.jpg",
  "https://cdn.21st.dev/assets/mirror/38/3824fce0f86b8c7b341a2a8541dd58e4bfe54ce40608dc3ff8716fe152740d68.jpg",
  "https://cdn.21st.dev/assets/mirror/0a/0a753d1d2c89c21c1936a906b7570a6ace0bb6e98e703d4b6e7a8ba6ca60ea9e.jpg",
  "https://cdn.21st.dev/assets/mirror/38/3824fce0f86b8c7b341a2a8541dd58e4bfe54ce40608dc3ff8716fe152740d68.jpg",
  "https://cdn.21st.dev/assets/mirror/0a/0a753d1d2c89c21c1936a906b7570a6ace0bb6e98e703d4b6e7a8ba6ca60ea9e.jpg",
  "https://cdn.21st.dev/assets/mirror/62/623fd1531de4b4116b5ccb9e26eaa9ffc27395a79c0d280ca6db8a692ce6720c.jpg",
  "https://cdn.21st.dev/assets/mirror/38/3824fce0f86b8c7b341a2a8541dd58e4bfe54ce40608dc3ff8716fe152740d68.jpg",
  "https://cdn.21st.dev/assets/mirror/0a/0a753d1d2c89c21c1936a906b7570a6ace0bb6e98e703d4b6e7a8ba6ca60ea9e.jpg",
];
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
