<!-- Horizontal Scroll · @uilayout.contact · https://21st.dev/@uilayout.contact/components/horizontal-scroll
     license: no-license · category: comparison
     A full-page scroll-driven section that translates a row of image panels horizontally as the user scrolls vertically. -->

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
components/ui/horizontal-scroll.txt
// @ts-nocheck
'use client';
import Image from 'next/image';

import { useEffect, useRef } from 'react';
import { animate, scroll, spring } from 'motion';
import { ReactLenis } from 'lenis/react';

export default function HorizontalScroll(): JSX.Element {
  const ulRef = useRef<HTMLUListElement>(null);

  useEffect(() => {
    const items = document.querySelectorAll('li');

    if (ulRef.current) {
      const controls = animate(
        ulRef.current,
        {
          transform: ['none', `translateX(-${items.length - 1}00vw)`],
        } as any,
        { easing: spring() }
      );
      const section = document.querySelector('section');
      if (section) {
        scroll(controls, { target: section });
      }
    }

    const segmentLength = 1 / items.length;
    items.forEach((item, i) => {
      const header = item.querySelector('h2');

      if (header) {
        const section = document.querySelector('section');
        if (section) {
          scroll(animate([header] as any, { x: [800, -800] } as any), {
            target: section,
            offset: [
              [i * segmentLength, 1],
              [(i + 1) * segmentLength, 0],
            ],
          });
        }
      }
    });
  }, []);

  return (
    <ReactLenis root>
      <main>
        <article>
          <header className='text-white relative  w-full bg-slate-950  grid place-content-center  h-[80vh]'>
            <div className='absolute bottom-0 left-0 right-0 top-0 bg-[linear-gradient(to_right,#4f4f4f2e_1px,transparent_1px),linear-gradient(to_bottom,#4f4f4f2e_1px,transparent_1px)] bg-size-[14px_24px] mask-[radial-gradient(ellipse_60%_50%_at_50%_0%,#000_70%,transparent_100%)]'></div>

            <h1 className='text-6xl font-bold text-center tracking-tight'>
              I know You Love to Scroll <br />
              So Scroll
            </h1>
          </header>
          <section className='h-[500vh] relative'>
            <ul ref={ulRef} className='flex sticky top-0'>
              <li className='h-screen w-screen bg-red-400 flex flex-col justify-center overflow-hidden  items-center'>
                <h2 className='text-[20vw] font-semibold relative bottom-5 inline-block text-black'>
                  PASSION
                </h2>
                <Image
                  src='https://images.unsplash.com/photo-1552664730-d307ca884978?w=600&h=600&fit=crop'
                  className='2xl:w-[550px] w-[380px] absolute bottom-0'
                  width={500}
                  height={500}
                  alt='image'
                />
              </li>
              <li className='h-screen w-screen bg-blue-400 flex flex-col justify-center overflow-hidden  items-center'>
                <h2 className='text-[20vw] font-semibold relative bottom-5 inline-block text-black'>
                  WORK
                </h2>
                <Image
                  src='https://images.unsplash.com/photo-1522071820081-009f0129c71c?w=600&h=600&fit=crop'
                  className='2xl:w-[550px] w-[380px] absolute bottom-0'
                  width={500}
                  height={500}
                  alt='image'
                />
              </li>
              <li className='h-screen w-screen bg-orange-400 flex flex-col justify-center overflow-hidden  items-center'>
                <h2 className='text-[20vw] font-semibold relative bottom-5 inline-block text-black'>
                  MOTIVATION
                </h2>
                <Image
                  src='https://images.unsplash.com/photo-1519389950473-47ba0277781c?w=600&h=600&fit=crop'
                  className='2xl:w-[550px] w-[380px] absolute bottom-0'
                  width={500}
                  height={500}
                  alt='image'
                />
              </li>
              <li className='h-screen w-screen bg-yellow-400 flex flex-col justify-center overflow-hidden  items-center'>
                <h2 className='text-[20vw] font-semibold relative bottom-5 inline-block text-black'>
                  INSPIRATION
                </h2>
                <Image
                  src='https://images.unsplash.com/photo-1531482615713-2afd69097998?w=600&h=600&fit=crop'
                  className='2xl:w-[550px] w-[380px] absolute bottom-0'
                  width={500}
                  height={500}
                  alt='image'
                />
              </li>
              <li className='h-screen w-screen bg-green-400 flex flex-col justify-center overflow-hidden  items-center'>
                <h2 className='text-[20vw] font-semibold relative bottom-5 inline-block text-black'>
                  BELIVE
                </h2>
                <Image
                  src='https://images.unsplash.com/photo-1521737711867-e3b97375f902?w=600&h=600&fit=crop'
                  className='2xl:w-[550px] w-[380px] absolute bottom-0'
                  width={500}
                  height={500}
                  alt='image'
                />
              </li>
            </ul>
          </section>
          <footer className='bg-red-600 font-medium text-white grid place-content-center h-[80vh]'>
            <p>
              Inspired By{' '}
              <a target='_blank' href='https://twitter.com/mattgperry'>
                Matt Perry
              </a>
            </p>
          </footer>
        </article>
      </main>
    </ReactLenis>
  );
}

demo.tsx
import HorizontalScroll from '@/components/ui/horizontal-scroll';

export default function HorizontalScrollDemo() {
  return <HorizontalScroll />;
}
```

Install NPM dependencies:
```bash
npm install lenis motion
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
