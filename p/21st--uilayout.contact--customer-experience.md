<!-- Customer Experience · @uilayout.contact · https://21st.dev/@uilayout.contact/components/customer-experience
     license: MIT · category: hero
     An interactive experience section that reveals a floating preview image following the cursor as you hover over each list row. -->

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
components/ui/customer-experience.tsx
'use client'

import { motion, useSpring } from 'motion/react'
import React, { useState, MouseEvent, useRef } from 'react'

interface ImageItem {
  img: string
  label: string
  title: string
  description: string
}
const list: ImageItem[] = [
  {
    img: 'https://images.unsplash.com/photo-1682806816936-c3ac11f65112?q=80&w=1274&auto=format&fit=crop',
    label: 'Urban Landscapes',
    title: 'City Photography',
    description:
      'A visual exploration of modern architectural wonders, iconic city skylines, and the rhythm of urban life captured through bold compositions.',
  },
  {
    img: 'https://images.unsplash.com/photo-1681063762354-d542c03bbfc5?q=80&w=1274&auto=format&fit=crop',
    label: 'Nature Portraits',
    title: 'Outdoor Photography',
    description:
      'Intimate moments of wildlife in their natural habitat, showcasing raw beauty, movement, and the quiet balance of the outdoors.',
  },
  {
    img: 'https://images.unsplash.com/photo-1679640034489-a6db1f096b70?q=80&w=1274&auto=format&fit=crop',
    label: 'Abstract Art',
    title: 'Creative Photography',
    description:
      'Contemporary artistic expressions that blend color, texture, and form to create visually striking and thought-provoking compositions.',
  },
  {
    img: 'https://images.unsplash.com/photo-1679482451632-b2e126da7142?q=80&w=1274&auto=format&fit=crop',
    label: 'Fashion Editorial',
    title: 'Style Photography',
    description:
      'High-fashion editorials highlighting runway trends, expressive styling, and editorial storytelling through bold visuals.',
  },

  {
    img: 'https://images.unsplash.com/photo-1682686580024-580519d4b2d2?q=80&w=1274&auto=format&fit=crop',
    label: 'Minimal Spaces',
    title: 'Interior Photography',
    description:
      'Calm and intentional interior scenes that focus on light, texture, and spatial balance, capturing the beauty of simplicity.',
  },
  {
    img: 'https://images.unsplash.com/photo-1683009427042-e094996f9780?q=80&w=1274&auto=format&fit=crop',
    label: 'Portrait Studies',
    title: 'Human Expression',
    description:
      'Expressive portraits that highlight emotion, personality, and subtle storytelling through light, framing, and composition.',
  },
  {
    img: 'https://images.unsplash.com/photo-1768248855015-36e55c07ae98?q=80&w=687&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    label: 'Motion & Speed',
    title: 'Action Photography',
    description:
      'Dynamic captures of movement and energy, freezing fast-paced moments while preserving a strong sense of momentum.',
  },
  {
    img: 'https://images.unsplash.com/photo-1769251845951-c271e703f047?q=80&w=687&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    label: 'Aerial Perspectives',
    title: 'Drone Photography',
    description:
      'Unique top-down viewpoints revealing patterns, scale, and geometry that are often hidden from ground level.',
  },
]

export function CustomerExperience() {
  const [img, setImg] = useState<{ src: string; alt: string; opacity: number }>(
    {
      src: '',
      alt: '',
      opacity: 0,
    }
  )

  const imageRef = useRef<HTMLImageElement>(null)
  const containerRef = useRef<HTMLDivElement>(null)

  const spring = {
    stiffness: 150,
    damping: 15,
    mass: 0.1,
  }

  const imagePos = {
    x: useSpring(0, spring),
    y: useSpring(0, spring),
  }

  const handleMove = (e: MouseEvent<HTMLDivElement>) => {
    if (!imageRef.current || !containerRef.current) return

    const containerRect = containerRef.current.getBoundingClientRect()
    const { clientX, clientY } = e
    const relativeX = clientX - containerRect.left
    const relativeY = clientY - containerRect.top

    imagePos.x.set(relativeX - imageRef.current.offsetWidth / 2)
    imagePos.y.set(relativeY - imageRef.current.offsetHeight / 2)
  }

  const handleImageInteraction = (item: ImageItem, opacity: number) => {
    setImg({ src: item.img, alt: item.label, opacity })
  }

  return (
    <section className="bg-orange-200 font-manrope overflow-x-hidden">
      <div
        ref={containerRef}
        onMouseMove={handleMove}
        className="relative max-w-6xl mx-auto border-x border-orange-500"
      >
        <h1 className="lg:text-9xl sm:text-8xl px-5 text-7xl border-b border-orange-500 font-bold py-10 text-orange-500 font-spaceGrotesk">
          EXPERIENCE
        </h1>
        {list.map((item) => (
          <div
            key={item.label}
            onMouseEnter={() => handleImageInteraction(item, 1)}
            onMouseMove={() => handleImageInteraction(item, 1)}
            onMouseLeave={() => handleImageInteraction(item, 0)}
            className="w-full py-5 px-5 cursor-pointer relative text-center md:flex justify-between items-center text-primary border-b border-orange-500 last:border-none"
          >
            <div className="flex gap-2 items-center">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                className="w-14 h-14 text-orange-500"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
              >
                <path d="M9 17.3497C9 17.3497 15.9383 17.8924 16.9154 16.9154C17.8924 15.9383 17.3496 9 17.3496 9M16.5 16.5L6.5 6.5" />
              </svg>
              <div className="flex flex-col items-start">
                <h2 className="lg:text-4xl text-xl font-bold">{item.label}</h2>
                <span>{item?.title}</span>
              </div>
            </div>
            <p className="xl:max-w-xl md:max-w-96 ml-auto text-right md:pt-0 pt-5">
              {item.description}{' '}
            </p>
          </div>
        ))}

        <motion.img
          ref={imageRef}
          src={img.src}
          alt={img.alt}
          className="w-[300px] h-[220px] rounded-lg object-cover absolute top-0 left-0 transition-opacity duration-200 ease-in-out pointer-events-none"
          style={{
            x: imagePos.x,
            y: imagePos.y,
            opacity: img.opacity,
          }}
        />
      </div>
    </section>
  )
}

components/ui/timeline-animation.tsx
import type { Variants } from 'motion/react';
import { type HTMLMotionProps, motion, useInView } from 'motion/react';
import type React from 'react';

type TimelineContentProps<T extends keyof HTMLElementTagNameMap> = {
  children?: React.ReactNode;
  animationNum: number;
  className?: string;
  timelineRef: React.RefObject<HTMLElement | null>;
  as?: T;
  customVariants?: Variants;
  once?: boolean;
} & HTMLMotionProps<T>;

export const TimelineAnimation = <T extends keyof HTMLElementTagNameMap = 'div'>({
  children,
  animationNum,
  timelineRef,
  className,
  as,
  customVariants,
  once = true,
  ...props
}: TimelineContentProps<T>) => {
  const defaultSequenceVariants = {
    visible: (i: number) => ({
      filter: 'blur(0px)',
      y: 0,
      opacity: 1,
      transition: {
        delay: i * 0.5,
        duration: 0.5,
      },
    }),
    hidden: {
      filter: 'blur(20px)',
      y: 0,
      opacity: 0,
    },
  };

  const sequenceVariants = customVariants || defaultSequenceVariants;

  const isInView = useInView(timelineRef, {
    once,
  });

  const MotionComponent = motion[as || 'div'] as React.ElementType;

  return (
    <MotionComponent
      initial='hidden'
      animate={isInView ? 'visible' : 'hidden'}
      custom={animationNum}
      variants={sequenceVariants}
      className={className}
      {...props}
    >
      {children}
    </MotionComponent>
  );
};

demo.tsx
"use client";

import { motion, useSpring } from "motion/react";
import React, { useState, MouseEvent, useRef, useEffect } from "react";

interface ImageItem {
  img: string;
  label: string;
  title: string;
  description: string;
}
const list: ImageItem[] = [
  {
    img: "https://cdn.21st.dev/assets/mirror/43/430b818cf67010b14e9099901405158a7746f46e82dd27c6f32aa784d26b0ca9.jpg",
    label: "Urban Landscapes",
    title: "City Photography",
    description:
      "A visual exploration of modern architectural wonders, iconic city skylines, and the rhythm of urban life captured through bold compositions.",
  },
  {
    img: "https://cdn.21st.dev/assets/mirror/3a/3a31c2a3c5eb8d0185bb6a525fa10ddaea70c06d213f4ae2db0d11d8ec04f3cd.jpg",
    label: "Nature Portraits",
    title: "Outdoor Photography",
    description:
      "Intimate moments of wildlife in their natural habitat, showcasing raw beauty, movement, and the quiet balance of the outdoors.",
  },
  {
    img: "https://cdn.21st.dev/assets/mirror/c1/c16e0f7913a1e5f34b189c87fed11be77246066e890b520f4d7fd50343fac4f3.jpg",
    label: "Abstract Art",
    title: "Creative Photography",
    description:
      "Contemporary artistic expressions that blend color, texture, and form to create visually striking and thought-provoking compositions.",
  },
  {
    img: "https://cdn.21st.dev/assets/mirror/bb/bbb3bebd187c292fcd32712662858406051d54b44820314809a2b1ebac850b73.jpg",
    label: "Fashion Editorial",
    title: "Style Photography",
    description:
      "High-fashion editorials highlighting runway trends, expressive styling, and editorial storytelling through bold visuals.",
  },
  {
    img: "https://cdn.21st.dev/assets/mirror/af/af6872a4581b630fd941fd1847a292347ff75ffbc9c46f92aaa5f83781aad464.jpg",
    label: "Minimal Spaces",
    title: "Interior Photography",
    description:
      "Calm and intentional interior scenes that focus on light, texture, and spatial balance, capturing the beauty of simplicity.",
  },
  {
    img: "https://cdn.21st.dev/assets/mirror/32/328d064d9a3882d1d6aa58ee4c3852d56d84bae5171dbbc5b46dffa1d54c0ed9.jpg",
    label: "Portrait Studies",
    title: "Human Expression",
    description:
      "Expressive portraits that highlight emotion, personality, and subtle storytelling through light, framing, and composition.",
  },
  {
    img: "https://cdn.21st.dev/assets/mirror/c5/c566b36a283c272dedc88c6d1ee644aeed120b560cba655434187a7e1f0ecc22.jpg",
    label: "Motion & Speed",
    title: "Action Photography",
    description:
      "Dynamic captures of movement and energy, freezing fast-paced moments while preserving a strong sense of momentum.",
  },
  {
    img: "https://cdn.21st.dev/assets/mirror/ef/efd81c6ac2e8d341bdfbbbff1e64b4d25bb595ed65d861cf52dc61c9a736799c.jpg",
    label: "Aerial Perspectives",
    title: "Drone Photography",
    description:
      "Unique top-down viewpoints revealing patterns, scale, and geometry that are often hidden from ground level.",
  },
];

export default function Default() {
  const [img, setImg] = useState<{ src: string; alt: string; opacity: number }>(
    {
      src: list[1].img,
      alt: list[1].label,
      opacity: 1,
    },
  );

  const imageRef = useRef<HTMLImageElement>(null);
  const containerRef = useRef<HTMLDivElement>(null);

  const spring = { stiffness: 150, damping: 15, mass: 0.1 };
  const imagePos = { x: useSpring(600, spring), y: useSpring(360, spring) };

  // Seed a revealed position so the interaction reads at rest.
  useEffect(() => {
    imagePos.x.jump(600);
    imagePos.y.jump(360);
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, []);

  const handleMove = (e: MouseEvent<HTMLDivElement>) => {
    if (!imageRef.current || !containerRef.current) return;
    const containerRect = containerRef.current.getBoundingClientRect();
    const { clientX, clientY } = e;
    const relativeX = clientX - containerRect.left;
    const relativeY = clientY - containerRect.top;
    imagePos.x.set(relativeX - imageRef.current.offsetWidth / 2);
    imagePos.y.set(relativeY - imageRef.current.offsetHeight / 2);
  };

  const handleImageInteraction = (item: ImageItem, opacity: number) => {
    setImg({ src: item.img, alt: item.label, opacity });
  };

  return (
    <section className="bg-orange-200 font-manrope overflow-x-hidden">
      <div
        ref={containerRef}
        onMouseMove={handleMove}
        className="relative max-w-6xl mx-auto border-x border-orange-500"
      >
        <h1 className="lg:text-9xl sm:text-8xl px-5 text-7xl border-b border-orange-500 font-bold py-10 text-orange-500 font-spaceGrotesk">
          EXPERIENCE
        </h1>
        {list.map((item) => (
          <div
            key={item.label}
            onMouseEnter={() => handleImageInteraction(item, 1)}
            onMouseMove={() => handleImageInteraction(item, 1)}
            onMouseLeave={() => handleImageInteraction(item, 0)}
            className="w-full py-5 px-5 cursor-pointer relative text-center md:flex justify-between items-center text-neutral-900 border-b border-orange-500 last:border-none"
          >
            <div className="flex gap-2 items-center">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                className="w-14 h-14 text-orange-500"
                fill="none"
                stroke="currentColor"
                strokeWidth="2"
              >
                <path d="M9 17.3497C9 17.3497 15.9383 17.8924 16.9154 16.9154C17.8924 15.9383 17.3496 9 17.3496 9M16.5 16.5L6.5 6.5" />
              </svg>
              <div className="flex flex-col items-start">
                <h2 className="lg:text-4xl text-xl font-bold">{item.label}</h2>
                <span>{item?.title}</span>
              </div>
            </div>
            <p className="xl:max-w-xl md:max-w-96 ml-auto text-right md:pt-0 pt-5">
              {item.description}{" "}
            </p>
          </div>
        ))}

        <motion.img
          ref={imageRef}
          src={img.src}
          alt={img.alt}
          className="w-[300px] h-[220px] rounded-lg object-cover absolute top-0 left-0 transition-opacity duration-200 ease-in-out pointer-events-none shadow-xl"
          style={{ x: imagePos.x, y: imagePos.y, opacity: img.opacity }}
        />
      </div>
    </section>
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
