<!-- Image Mouse Trail · @uilayout.contact · https://21st.dev/@uilayout.contact/components/image-mousetrail-without-component
     license: MIT · category: hero
     An interactive hero section that reveals a trail of images following the cursor as the mouse moves across the container. -->

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
components/ui/without-component-mousetrail.tsx
//@ts-nocheck
'use client';
import { items } from '@/components/website/constant';
import React, { createRef, useRef } from 'react';

export default function ImageMouseTrail3() {
  const containerRef = useRef(null);
  const refs = useRef(items.map(() => createRef<HTMLImageElement>()));

  let globalIndex = 0;
  let last = { x: 0, y: 0 };

  const activate = (image, x, y) => {
    const containerRect = containerRef.current?.getBoundingClientRect();
    const relativeX = x - containerRect.left;
    const relativeY = y - containerRect.top;
    image.style.left = `${relativeX}px`;
    image.style.top = `${relativeY}px`;

    image.style.zIndex = (globalIndex % items.length) + 1;

    image.dataset.status = 'active';
    setTimeout(() => {
      image.dataset.status = 'inactive';
    }, 1000);
    last = { x, y };
  };

  const distanceFromLast = (x, y) => {
    return Math.hypot(x - last.x, y - last.y);
  };
  const deactivate = (image) => {
    image.dataset.status = 'inactive';
  };
  const handleOnMove = (e) => {
    if (distanceFromLast(e.clientX, e.clientY) > window.innerWidth / 20) {
      const lead = refs.current[globalIndex % refs.current.length].current;

      const tail = refs.current[(globalIndex - 5) % refs.current.length]?.current;

      if (lead) activate(lead, e.clientX, e.clientY);
      if (tail) deactivate(tail);

      globalIndex++;
    }
  };

  return (
    <section
      onMouseMove={handleOnMove}
      onTouchMove={(e) => handleOnMove(e.touches[0])}
      ref={containerRef}
      className='grid place-content-center h-[600px] w-full bg-[#e0dfdf] relative overflow-hidden rounded-lg'
    >
      {items.map((item, index) => (
        <img
          key={item.src}
          className="object-cover z-10   w-40 h-48 scale-0 opacity:0 data-[status='active']:scale-100  data-[status='active']:opacity-100 transition-transform duration-500 data-[status='active']:ease-out-expo  absolute  -translate-y-[50%] -translate-x-[50%]"
          data-index={index}
          data-status='inactive'
          src={item.url}
          alt={`image-${index}`}
          ref={refs.current[index]}
        />
      ))}
      <article className='relative z-20 mix-blend-difference'>
        <h1 className='md:text-4xl text-2xl text-center font-semibold'>
          ✨ Experience Interactive Designs <br />
          with Dynamic Mouse Trails <br />
          built with Tailwind CSS
        </h1>
      </article>
    </section>
  );
}

demo.tsx
import ImageMouseTrail3 from '@/components/ui/image-mousetrail-without-component';

export default function ImageMouseTrailDemo() {
  return (
    <div className='w-full p-4'>
      <ImageMouseTrail3 />
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
