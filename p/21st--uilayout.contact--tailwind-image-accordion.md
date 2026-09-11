<!-- Tailwind Image Accordion · @uilayout.contact · https://21st.dev/@uilayout.contact/components/tailwind-image-accordion
     license: MIT · category: team
     A horizontal image accordion where hovering or focusing a panel expands it and reveals a title and subtitle, built with pure Tailwind CSS group utilities. -->

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
components/ui/tailwind-image-accordion.tsx
'use client';
import Image from 'next/image';
import React from 'react';

const items = [
  {
    id: '1',
    url: '/adrian.png',
    title: 'Adrian Paul',
    description: 'COO & Co-Founder',
    tags: ['Floral', 'Highlands', 'Wildflowers', 'Colorful', 'Resilience'],
  },

  {
    id: '2',
    url: '/flualy.jpg',
    title: 'Flualy Cual',
    description: 'Founder & CEO',
    tags: ['Twilight', 'Peaks', 'Silhouette', 'Evening Sky', 'Peaceful'],
  },
  {
    id: '3',
    url: '/naymur.png',
    title: 'Naymur Rahman',
    description: 'CTO & Co-Founder',
    tags: ['Rocky', 'Ridges', 'Contrast', 'Adventure', 'Clouds'],
  },
];
function TailwindImageAccordion() {
  return (
    <>
      <div className='group flex max-md:flex-col justify-center gap-2 w-[80%] mx-auto mb-10 mt-3'>
        {items.map((item, i: number) => {
          return (
            <article
              key={item?.url ?? item?.title}
              className='group/article relative w-full rounded-xl overflow-hidden md:not-[&:hover]:group-hover:w-[20%] md:[&:not(:focus-within):not(:hover)]:group-focus-within:w-[20%] transition-all duration-300 ease-[cubic-bezier(.5,.85,.25,1.15)] before:absolute before:inset-x-0 before:bottom-0 before:h-1/3 before:bg-linear-to-t before:from-black/50 before:transition-opacity md:before:opacity-0 md:hover:before:opacity-100 focus-within:before:opacity-100 after:opacity-0 md:not-[&:hover]:group-hover:after:opacity-100 md:[&:not(:focus-within):not(:hover)]:group-focus-within:after:opacity-100 after:absolute after:inset-0 after:bg-white/30 after:backdrop-blur-sm after:rounded-lg after:transition-all focus-within:ring-3 focus-within:ring-indigo-300'
            >
              <a
                className='absolute inset-0 text-white z-10  p-3 flex flex-col justify-end'
                href='#'
              >
                <h1 className=' text-xl font-medium   md:whitespace-nowrap md:truncate md:opacity-0 group-hover/article:opacity-100 group-focus-within/article:opacity-100 md:translate-y-2 group-hover/article:translate-y-0 group-focus-within/article:translate-y-0 transition duration-200 ease-[cubic-bezier(.5,.85,.25,1.8)] group-hover/article:delay-300 group-focus-within/article:delay-300'>
                  {item?.title}
                </h1>
                <span className=' text-3xl font-medium  md:whitespace-nowrap md:truncate md:opacity-0 group-hover/article:opacity-100 group-focus-within/article:opacity-100 md:translate-y-2 group-hover/article:translate-y-0 group-focus-within/article:translate-y-0 transition duration-200 ease-[cubic-bezier(.5,.85,.25,1.8)] group-hover/article:delay-500 group-focus-within/article:delay-500'>
                  {item?.description}
                </span>
              </a>
              <Image
                className='object-cover h-72 md:h-[420px]  w-full'
                src={item?.url}
                width='960'
                height='480'
                alt='Image 01'
              />
            </article>
          );
        })}
      </div>
    </>
  );
}

export default TailwindImageAccordion;

demo.tsx
import TailwindImageAccordion from "@/components/ui/tailwind-image-accordion";

export default function Default() {
  return (
    <div className="w-full py-10">
      <TailwindImageAccordion />
    </div>
  );
}
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
