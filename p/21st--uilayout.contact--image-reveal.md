<!-- Image Reveal · @uilayout.contact · https://21st.dev/@uilayout.contact/components/image-reveal
     license: unspecified · category: image
     Here is image reveal component -->

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
components/ui/image-reveal.tsx
'use client';

import { motion, useSpring } from 'motion/react';
import React, { type MouseEvent, useRef, useState } from 'react';

interface ImageItem {
  img: string;
  label: string;
  tag: string;
}

function ImageReveal2() {
  const [img, setImg] = useState<{ src: string; alt: string; opacity: number }>({
    src: '',
    alt: '',
    opacity: 0,
  });

  const imageRef = useRef<HTMLImageElement>(null);
  const containerRef = useRef<HTMLDivElement>(null);

  const list: ImageItem[] = [
    {
      img: 'https://images.unsplash.com/photo-1682806816936-c3ac11f65112?q=80&w=1274&auto=format&fit=crop',
      label: 'Stay Confident',
      tag: 'Style',
    },
    {
      img: 'https://images.unsplash.com/photo-1681063762354-d542c03bbfc5?q=80&w=1274&auto=format&fit=crop',
      label: 'Create Your Space',
      tag: 'Design',
    },
    {
      img: 'https://images.unsplash.com/photo-1679640034489-a6db1f096b70?q=80&w=1274&auto=format&fit=crop',
      label: 'Be Yourself',
      tag: 'Identity',
    },
    {
      img: 'https://images.unsplash.com/photo-1679482451632-b2e126da7142?q=80&w=1274&auto=format&fit=crop',
      label: 'Live Inspired',
      tag: 'Lifestyle',
    },
  ];

  const spring = {
    stiffness: 150,
    damping: 15,
    mass: 0.1,
  };

  const imagePos = {
    x: useSpring(0, spring),
    y: useSpring(0, spring),
  };

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
    <section ref={containerRef} onMouseMove={handleMove} className='relative w-4/5 mx-auto p-4'>
      {list.map((item) => (
        <div
          key={item.label}
          onMouseEnter={() => handleImageInteraction(item, 1)}
          onMouseMove={() => handleImageInteraction(item, 1)}
          onMouseLeave={() => handleImageInteraction(item, 0)}
          className='w-full py-5 cursor-pointer text-center flex justify-between items-center text-primary border-b dark:border-neutral-800 border-neutral-50 last:border-none'
        >
          <p className='text-5xl'>{item.label}</p>
          <span>
            {item.tag} <span className='w-3 h-3 bg-primary inline-block'></span>
          </span>
        </div>
      ))}

      <motion.img
        ref={imageRef}
        src={img.src}
        alt={img.alt}
        className='w-[300px] h-[220px] rounded-lg object-cover absolute top-0 left-0 transition-opacity duration-200 ease-in-out pointer-events-none'
        style={{
          x: imagePos.x,
          y: imagePos.y,
          opacity: img.opacity,
        }}
      />
    </section>
  );
}

export default ImageReveal2;

demo.tsx
// demo.tsx
import React from 'react';
import Component from '@/components/ui/image-reveal';

const ComponentDemo = () => {
  return (
    <div className='flex flex-col items-center justify-center gap-12 p-8 bg-black min-h-screen text-white w-full'>

      <div className='flex justify-center w-full'>
        <div className='flex flex-col items-center gap-4 border p-6 rounded-xl border-white bg-black shadow-lg'>
          <h2 className='text-2xl font-semibold mb-2'></h2>
          <Component />
        </div>
      </div>
    </div>
  );
};

export { ComponentDemo as DemoOne };
```

Install NPM dependencies:
```bash
npm install lucide-react
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
