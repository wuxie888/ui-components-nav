<!-- Footer Section 3 · @solaceui · https://21st.dev/@solaceui/components/footer-section-3
     license: no-license · category: footer
     Animated footer section with a noise-textured headline banner, newsletter signup, social icons, and navigation link columns. -->

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
"use client";
import React, { useRef } from "react";
import Link from "next/link";
import { motion, useInView } from "motion/react";
import { SocialCloud } from "@/registry/blocks/footer-section/social-cloud";
import { AnimatedGroup } from "@/components/ui/animated-group"; //from motion-primitives

export default function Footer3({
  title = "Build Tastefully Crafted UIs",
}: {
  title?: string;
}) {
  const containerVariants = {
    // ... existing variants ...
    hidden: { opacity: 0 },
    visible: {
      opacity: 1,
      transition: {
        staggerChildren: 0.1,
        delayChildren: 0.1,
      },
    },
  };

  const itemVariants = {
    hidden: { opacity: 0, y: 20 },
    visible: {
      opacity: 1,
      y: 0,
      transition: {
        duration: 0.5,
      },
    },
  };

  const titleRef = useRef(null);
  const isTitleInView = useInView(titleRef, { once: true, margin: "-100px" });

  return (
    <section className="py-12 px-4 [--color-primary:#003AF9]">
      <motion.div
        className="container mx-auto max-w-7xl"
        variants={containerVariants}
        initial="hidden"
        whileInView="visible"
        viewport={{ once: true }}
      >
        {/* 1. Upper div with noise and text */}
        <motion.div
          className="relative w-full overflow-hidden rounded-xl bg-(--color-primary) h-[100px] md:h-[300px]"
          variants={itemVariants}
        >
          {/* SVG Noise Overlay */}
          <svg
            className="absolute inset-0 w-full h-full opacity-90 pointer-events-none mix-blend-multiply"
            xmlns="http://www.w3.org/2000/svg"
          >
            <filter id="noiseFilter">
              <feTurbulence
                type="fractalNoise"
                baseFrequency="0.65"
                numOctaves="4"
                stitchTiles="stitch"
              />
            </filter>
            <rect width="100%" height="100%" filter="url(#noiseFilter)" />
          </svg>

          {/* Centered Text */}
          <div
            ref={titleRef}
            className="absolute inset-0 flex items-center justify-center pointer-events-none"
          >
            {isTitleInView && (
              <AnimatedGroup
                className="text-2xl md:text-4xl lg:text-5xl font-bold text-white text-center flex flex-wrap justify-center gap-x-3"
                preset="slide"
              >
                {title.split(" ").map((word) => (
                  <span key={word} className="inline-block">
                    {word}
                  </span>
                ))}
              </AnimatedGroup>
            )}
          </div>
        </motion.div>

        {/* 2 & 3. Bottom Section */}
        <div className="mt-8 md:mt-16 flex flex-col md:flex-row justify-between gap-12 lg:gap-8">
          {/* 2. Below Left Section */}
          <motion.div
            className="flex flex-col justify-between space-y-12 lg:w-1/3"
            variants={itemVariants}
          >
            <div className="space-y-4">
              <h3 className="text-xl font-bold text-black dark:text-white">
                Newsletter
              </h3>
              <div className="flex items-center gap-4 max-w-md">
                <input
                  type="email"
                  placeholder="Enter Your Email"
                  className="flex-1 rounded-lg border border-neutral-300 dark:border-neutral-700 px-4 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-black dark:focus:ring-white bg-transparent dark:bg-neutral-900 text-black dark:text-white"
                />
                <button className="rounded-lg bg-black text-white dark:bg-white dark:text-black px-6 py-2 text-sm font-medium hover:opacity-90 transition-opacity">
                  Submit
                </button>
              </div>
            </div>

            <div className="space-y-4 md:space-y-6">
              <SocialCloud className="text-neutral-500 dark:text-neutral-400 gap-4" />
              <p className="text-sm text-neutral-400 dark:text-neutral-600">
                &copy; {new Date().getFullYear()} SolaceUI, All rights reserved
              </p>
            </div>
          </motion.div>

          {/* 3. Below Right Section */}
          <motion.div
            className="flex flex-wrap gap-8 lg:gap-24 lg:w-1/2 lg:justify-end"
            variants={itemVariants}
          >
            {/* Column 1: Product */}
            <div className="flex flex-col space-y-4">
              <h4 className="text-lg font-bold text-black dark:text-white">
                Product
              </h4>
              <ul className="flex flex-col space-y-2 text-neutral-600 dark:text-neutral-400">
                {[
                  "Features",
                  "Solution",
                  "Customers",
                  "Pricing",
                  "Help",
                  "Terms",
                ].map((item) => (
                  <li key={item}>
                    <Link
                      href="#"
                      className="hover:text-black dark:hover:text-white transition-colors"
                    >
                      {item}
                    </Link>
                  </li>
                ))}
              </ul>
            </div>

            {/* Column 2: Company */}
            <div className="flex flex-col space-y-4">
              <h4 className="text-lg font-bold text-black dark:text-white">
                Company
              </h4>
              <ul className="flex flex-col space-y-2 text-neutral-600 dark:text-neutral-400">
                {[
                  "About",
                  "Careers",
                  "Blogs",
                  "Press",
                  "Contact",
                  "Privacy",
                ].map((item) => (
                  <li key={item}>
                    <Link
                      href="#"
                      className="hover:text-black dark:hover:text-white transition-colors"
                    >
                      {item}
                    </Link>
                  </li>
                ))}
              </ul>
            </div>

            {/* Column 3: Social */}
            <div className="flex flex-col space-y-4">
              <h4 className="text-lg font-bold text-black dark:text-white">
                Social
              </h4>
              <ul className="flex flex-col space-y-2 text-neutral-600 dark:text-neutral-400">
                {[
                  "X",
                  "LinkedIn",
                  "Facebook",
                  "Threads",
                  "Instagram",
                  "Youtube",
                ].map((item) => (
                  <li key={item}>
                    <Link
                      href="#"
                      className="hover:text-black dark:hover:text-white transition-colors"
                    >
                      {item}
                    </Link>
                  </li>
                ))}
              </ul>
            </div>
          </motion.div>
        </div>
      </motion.div>
    </section>
  );
}

components/ui/social-cloud.tsx
"use client";
import React from "react";
import { cn } from "@/lib/utils";
import { motion, MotionProps } from "motion/react";

interface SocialIconProps extends React.SVGProps<SVGSVGElement> {}

export const XIcon = ({ className, ...props }: SocialIconProps) => (
  <motion.svg
    className={cn("h-5 w-5", className)}
    width="23"
    height="20"
    viewBox="0 0 23 20"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    whileHover={{ scale: 1.2, rotate: 5 }}
    whileTap={{ scale: 0.9 }}
    transition={{ type: "spring", stiffness: 400, damping: 17 }}
  >
    <path
      d="M9.46617 12.6219L14.625 19.5H22.2083L13.6955 8.14883L20.7783 0H17.9075L12.3641 6.3765L7.58333 0H0L8.13583 10.8496L0.6175 19.5H3.48833L9.46617 12.6219ZM15.7083 17.3333L4.33333 2.16667H6.5L17.875 17.3333H15.7083Z"
      fill="currentColor"
    />
  </motion.svg>
);

export const LinkedInIcon = ({ className, ...props }: SocialIconProps) => (
  <motion.svg
    className={cn("h-5 w-5", className)}
    width="23"
    height="23"
    viewBox="0 0 23 23"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    whileHover={{ scale: 1.2, rotate: -5 }}
    whileTap={{ scale: 0.9 }}
    transition={{ type: "spring", stiffness: 400, damping: 17 }}
  >
    <path
      d="M20 0C20.663 0 21.2989 0.263392 21.7678 0.732233C22.2366 1.20107 22.5 1.83696 22.5 2.5V20C22.5 20.663 22.2366 21.2989 21.7678 21.7678C21.2989 22.2366 20.663 22.5 20 22.5H2.5C1.83696 22.5 1.20107 22.2366 0.732233 21.7678C0.263392 21.2989 0 20.663 0 20V2.5C0 1.83696 0.263392 1.20107 0.732233 0.732233C1.20107 0.263392 1.83696 0 2.5 0H20ZM19.375 19.375V12.75C19.375 11.6692 18.9457 10.6328 18.1815 9.86854C17.4172 9.10433 16.3808 8.675 15.3 8.675C14.2375 8.675 13 9.325 12.4 10.3V8.9125H8.9125V19.375H12.4V13.2125C12.4 12.25 13.175 11.4625 14.1375 11.4625C14.6016 11.4625 15.0467 11.6469 15.3749 11.9751C15.7031 12.3033 15.8875 12.7484 15.8875 13.2125V19.375H19.375ZM4.85 6.95C5.40695 6.95 5.9411 6.72875 6.33492 6.33492C6.72875 5.9411 6.95 5.40695 6.95 4.85C6.95 3.6875 6.0125 2.7375 4.85 2.7375C4.28973 2.7375 3.75241 2.96007 3.35624 3.35624C2.96007 3.75241 2.7375 4.28973 2.7375 4.85C2.7375 6.0125 3.6875 6.95 4.85 6.95ZM6.5875 19.375V8.9125H3.125V19.375H6.5875Z"
      fill="currentColor"
    />
  </motion.svg>
);

export const FacebookIcon = ({ className, ...props }: SocialIconProps) => (
  <motion.svg
    className={cn("h-5 w-5", className)}
    width="29"
    height="29"
    viewBox="0 0 29 29"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    whileHover={{ scale: 1.2, rotate: 5 }}
    whileTap={{ scale: 0.9 }}
    transition={{ type: "spring", stiffness: 400, damping: 17 }}
  >
    <path
      d="M28.3333 14.1667C28.3333 6.34667 21.9867 0 14.1667 0C6.34667 0 0 6.34667 0 14.1667C0 21.0233 4.87333 26.7325 11.3333 28.05V18.4167H8.5V14.1667H11.3333V10.625C11.3333 7.89083 13.5575 5.66667 16.2917 5.66667H19.8333V9.91667H17C16.2208 9.91667 15.5833 10.5542 15.5833 11.3333V14.1667H19.8333V18.4167H15.5833V28.2625C22.7375 27.5542 28.3333 21.5192 28.3333 14.1667Z"
      fill="currentColor"
    />
  </motion.svg>
);

export const ThreadsIcon = ({ className, ...props }: SocialIconProps) => (
  <motion.svg
    className={cn("h-5 w-5", className)}
    width="18"
    height="22"
    viewBox="0 0 18 22"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    whileHover={{ scale: 1.2, rotate: -5 }}
    whileTap={{ scale: 0.9 }}
    transition={{ type: "spring", stiffness: 400, damping: 17 }}
  >
    <path
      d="M17.4167 6.92196C15.5768 0.0771293 9.25 0.505296 9.25 0.505296C9.25 0.505296 0.5 -0.0780373 0.5 10.9995C0.5 22.077 9.25 21.4948 9.25 21.4948C9.25 21.4948 14.451 21.8401 16.8333 16.9238C17.6115 14.7561 17.4167 10.422 9.83333 10.422C9.83333 10.422 6.33333 10.422 6.33333 13.3386C6.33333 14.4773 7.5 15.672 9.25 15.672C11 15.672 12.9495 14.4738 13.3333 12.172C14.5 5.17196 8.08333 4.58863 6.33333 7.5053"
      stroke="currentColor"
      strokeLinecap="round"
      strokeLinejoin="round"
    />
  </motion.svg>
);

export const InstagramIcon = ({ className, ...props }: SocialIconProps) => (
  <motion.svg
    className={cn("h-5 w-5", className)}
    width="24"
    height="24"
    viewBox="0 0 24 24"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    whileHover={{ scale: 1.2, rotate: 5 }}
    whileTap={{ scale: 0.9 }}
    transition={{ type: "spring", stiffness: 400, damping: 17 }}
  >
    <path
      d="M6.76667 0H16.5667C20.3 0 23.3333 3.03333 23.3333 6.76667V16.5667C23.3333 18.3613 22.6204 20.0824 21.3514 21.3514C20.0824 22.6204 18.3613 23.3333 16.5667 23.3333H6.76667C3.03333 23.3333 0 20.3 0 16.5667V6.76667C0 4.97204 0.712915 3.25091 1.98191 1.98191C3.25091 0.712915 4.97204 0 6.76667 0ZM6.53333 2.33333C5.41942 2.33333 4.35114 2.77583 3.56349 3.56349C2.77583 4.35114 2.33333 5.41942 2.33333 6.53333V16.8C2.33333 19.1217 4.21167 21 6.53333 21H16.8C17.9139 21 18.9822 20.5575 19.7699 19.7699C20.5575 18.9822 21 17.9139 21 16.8V6.53333C21 4.21167 19.1217 2.33333 16.8 2.33333H6.53333ZM17.7917 4.08333C18.1784 4.08333 18.5494 4.23698 18.8229 4.51047C19.0964 4.78396 19.25 5.15489 19.25 5.54167C19.25 5.92844 19.0964 6.29937 18.8229 6.57286C18.5494 6.84635 18.1784 7 17.7917 7C17.4049 7 17.034 6.84635 16.7605 6.57286C16.487 6.29937 16.3333 5.92844 16.3333 5.54167C16.3333 5.15489 16.487 4.78396 16.7605 4.51047C17.034 4.23698 17.4049 4.08333 17.7917 4.08333ZM11.6667 5.83333C13.2138 5.83333 14.6975 6.44792 15.7915 7.54188C16.8854 8.63584 17.5 10.1196 17.5 11.6667C17.5 13.2138 16.8854 14.6975 15.7915 15.7915C14.6975 16.8854 13.2138 17.5 11.6667 17.5C10.1196 17.5 8.63584 16.8854 7.54188 15.7915C6.44792 14.6975 5.83333 13.2138 5.83333 11.6667C5.83333 10.1196 6.44792 8.63584 7.54188 7.54188C8.63584 6.44792 10.1196 5.83333 11.6667 5.83333ZM11.6667 8.16667C10.7384 8.16667 9.84817 8.53542 9.19179 9.19179C8.53542 9.84817 8.16667 10.7384 8.16667 11.6667C8.16667 12.5949 8.53542 13.4852 9.19179 14.1415C9.84817 14.7979 10.7384 15.1667 11.6667 15.1667C12.5949 15.1667 13.4852 14.7979 14.1415 14.1415C14.7979 13.4852 15.1667 12.5949 15.1667 11.6667C15.1667 10.7384 14.7979 9.84817 14.1415 9.19179C13.4852 8.53542 12.5949 8.16667 11.6667 8.16667Z"
      fill="currentColor"
    />
  </motion.svg>
);

export const TikTokIcon = ({ className, ...props }: SocialIconProps) => (
  <motion.svg
    className={cn("h-5 w-5", className)}
    width="19"
    height="21"
    viewBox="0 0 19 21"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    whileHover={{ scale: 1.2, rotate: -5 }}
    whileTap={{ scale: 0.9 }}
    transition={{ type: "spring", stiffness: 400, damping: 17 }}
  >
    <path
      d="M14.5133 3.29C13.716 2.37945 13.2765 1.2103 13.2767 0H9.67167V14.4667C9.64444 15.2497 9.31411 15.9916 8.75036 16.5357C8.18661 17.0799 7.43353 17.3838 6.65 17.3833C4.99333 17.3833 3.61667 16.03 3.61667 14.35C3.61667 12.3433 5.55333 10.8383 7.54833 11.4567V7.77C3.52333 7.23333 0 10.36 0 14.35C0 18.235 3.22 21 6.63833 21C10.3017 21 13.2767 18.025 13.2767 14.35V7.01167C14.7385 8.06149 16.4936 8.62475 18.2933 8.62167V5.01667C18.2933 5.01667 16.1 5.12167 14.5133 3.29Z"
      fill="currentColor"
    />
  </motion.svg>
);

export const YouTubeIcon = ({ className, ...props }: SocialIconProps) => (
  <motion.svg
    className={cn("h-5 w-5", className)}
    width="29"
    height="21"
    viewBox="0 0 29 21"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    whileHover={{ scale: 1.2, rotate: 5 }}
    whileTap={{ scale: 0.9 }}
    transition={{ type: "spring", stiffness: 400, damping: 17 }}
  >
    <path
      d="M28.0501 3.13163C27.7206 1.8991 26.7497 0.928222 25.5172 0.59876C23.2832 -1.51787e-07 14.3244 0 14.3244 0C14.3244 0 5.36561 -1.51787e-07 3.13164 0.59876C1.8991 0.928222 0.928222 1.8991 0.59876 3.13163C0 5.36561 0 10.0271 0 10.0271C0 10.0271 0 14.6886 0.59876 16.9225C0.928222 18.1551 1.8991 19.126 3.13164 19.4554C5.36561 20.0542 14.3244 20.0542 14.3244 20.0542C14.3244 20.0542 23.2832 20.0542 25.5172 19.4554C26.7497 19.126 27.7206 18.1551 28.0501 16.9225C28.6488 14.6886 28.6488 10.0271 28.6488 10.0271C28.6488 10.0271 28.6488 5.36561 28.0501 3.13163ZM11.4595 14.3244V5.72977L18.9025 10.0271L11.4595 14.3244Z"
      fill="currentColor"
    />
  </motion.svg>
);

interface SocialCloudProps
  extends
    Omit<React.HTMLAttributes<HTMLDivElement>, keyof MotionProps>,
    MotionProps {}

export function SocialCloud({ className, ...props }: SocialCloudProps) {
  return (
    <motion.div
      className={cn(
        "flex flex-wrap items-center gap-6 cursor-pointer",
        className,
      )}
      {...props}
    >
      <XIcon />
      <LinkedInIcon />
      <FacebookIcon />
      <ThreadsIcon />
      <InstagramIcon />
      <TikTokIcon />
      <YouTubeIcon />
    </motion.div>
  );
}

components/ui/animated-group.tsx
// Component From Motion Primitives

"use client";
import { ReactNode } from "react";
import { motion, Variants, HTMLMotionProps } from "motion/react";
import React from "react";

export type PresetType =
  | "fade"
  | "slide"
  | "scale"
  | "blur"
  | "blur-slide"
  | "zoom"
  | "flip"
  | "bounce"
  | "rotate"
  | "swing";

export type AnimatedGroupProps = {
  children: ReactNode;
  className?: string;
  variants?: {
    container?: Variants;
    item?: Variants;
  };
  preset?: PresetType;
  as?: React.ElementType;
  asChild?: React.ElementType;
};

const defaultContainerVariants: Variants = {
  visible: {
    transition: {
      staggerChildren: 0.1,
    },
  },
};

const defaultItemVariants: Variants = {
  hidden: { opacity: 0 },
  visible: { opacity: 1 },
};

const presetVariants: Record<PresetType, Variants> = {
  fade: {},
  slide: {
    hidden: { y: 20 },
    visible: { y: 0 },
  },
  scale: {
    hidden: { scale: 0.8 },
    visible: { scale: 1 },
  },
  blur: {
    hidden: { filter: "blur(4px)" },
    visible: { filter: "blur(0px)" },
  },
  "blur-slide": {
    hidden: { filter: "blur(4px)", y: 20 },
    visible: { filter: "blur(0px)", y: 0 },
  },
  zoom: {
    hidden: { scale: 0.5 },
    visible: {
      scale: 1,
      transition: { type: "spring", stiffness: 300, damping: 20 },
    },
  },
  flip: {
    hidden: { rotateX: -90 },
    visible: {
      rotateX: 0,
      transition: { type: "spring", stiffness: 300, damping: 20 },
    },
  },
  bounce: {
    hidden: { y: -50 },
    visible: {
      y: 0,
      transition: { type: "spring", stiffness: 400, damping: 10 },
    },
  },
  rotate: {
    hidden: { rotate: -180 },
    visible: {
      rotate: 0,
      transition: { type: "spring", stiffness: 200, damping: 15 },
    },
  },
  swing: {
    hidden: { rotate: -10 },
    visible: {
      rotate: 0,
      transition: { type: "spring", stiffness: 300, damping: 8 },
    },
  },
};

const mergeVariants = (
  defaultVariants: Variants,
  customVariants: Partial<Variants> = {},
): Variants => {
  return {
    hidden: {
      ...defaultVariants.hidden,
      ...customVariants.hidden,
    },
    visible: {
      ...defaultVariants.visible,
      ...customVariants.visible,
    },
  };
};

function AnimatedGroup({
  children,
  className,
  variants,
  preset = "fade",
  as = "div",
  asChild = "div",
}: AnimatedGroupProps) {
  const MotionContainer = motion.div;
  const MotionChild = motion.div;

  const presetVariant = preset ? presetVariants[preset] : {};
  const itemVariants = variants?.item
    ? mergeVariants(defaultItemVariants, variants.item)
    : mergeVariants(defaultItemVariants, presetVariant);

  const containerVariants = variants?.container || defaultContainerVariants;

  return (
    <MotionContainer
      initial="hidden"
      animate="visible"
      variants={containerVariants}
      className={className}
    >
      {React.Children.map(children, (child, index) => (
        <MotionChild key={index} variants={itemVariants}>
          {child}
        </MotionChild>
      ))}
    </MotionContainer>
  );
}

export { AnimatedGroup };

demo.tsx
import Footer3 from "@/components/ui/footer-section-3";

export default function Default() {
  return <Footer3 title="Build Tastefully Crafted UIs" />;
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add animated-group animated-group?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068
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
