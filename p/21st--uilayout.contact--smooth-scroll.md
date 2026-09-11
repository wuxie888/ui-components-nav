<!-- Smooth Scroll · @uilayout.contact · https://21st.dev/@uilayout.contact/components/smooth-scroll
     license: unspecified · category: scroll-area
     Here is Smooth Scroll card -->

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
components/ui/smooth-scroll.txt
'use client';
import { ReactLenis } from 'lenis/react';
export default function index(): JSX.Element {
  return (
    <ReactLenis root>
      <main>
        <article>
          <section className='text-white  h-screen  w-full bg-slate-950  grid place-content-center sticky top-0'>
            <div className='absolute bottom-0 left-0 right-0 top-0 bg-[linear-gradient(to_right,#4f4f4f2e_1px,transparent_1px),linear-gradient(to_bottom,#4f4f4f2e_1px,transparent_1px)] bg-size-[54px_54px] mask-[radial-gradient(ellipse_60%_50%_at_50%_0%,#000_70%,transparent_100%)]'></div>

            <h1 className='2xl:text-7xl text-6xl px-8 font-semibold text-center tracking-tight leading-[120%]'>
              I Know What Exactly you're <br /> Looking For! Scroll Please 👇
            </h1>
          </section>

          <section className='bg-neutral-300 text-black grid place-content-center h-screen sticky top-0 rounded-tr-2xl rounded-tl-2xl overflow-hidden'>
            <div className='absolute bottom-0 left-0 right-0 top-0 bg-[linear-gradient(to_right,#4f4f4f2e_1px,transparent_1px),linear-gradient(to_bottom,#4f4f4f2e_1px,transparent_1px)] bg-size-[54px_54px] mask-[radial-gradient(ellipse_60%_50%_at_50%_0%,#000_70%,transparent_100%)]'></div>
            <h1 className='2xl:text-7xl text-4xl px-8 font-semibold text-center tracking-tight leading-[120%]'>
              If you don't like this then I'm sorry, <br /> create your own and
              make it open source 💼
            </h1>
          </section>
          <section className='text-white  h-screen  w-full bg-slate-950  grid place-content-center sticky top-0'>
            <div className='absolute bottom-0 left-0 right-0 top-0 bg-[linear-gradient(to_right,#4f4f4f2e_1px,transparent_1px),linear-gradient(to_bottom,#4f4f4f2e_1px,transparent_1px)] bg-size-[54px_54px] mask-[radial-gradient(ellipse_60%_50%_at_50%_0%,#000_70%,transparent_100%)]'></div>
            <h1 className='2xl:text-7xl text-5xl px-8 font-semibold text-center tracking-tight leading-[120%]'>
              Thanks To Scroll.
              <br /> Now Scroll Up Again☝️🏿
            </h1>
          </section>
        </article>
      </main>
    </ReactLenis>
  );
}

demo.tsx
// demo.tsx
import React from 'react';
import Component from '@/components/ui/smooth-scroll';

function ComponentDemo() {
  return (
    <Component />
  );
}

export { ComponentDemo as DemoOne };
```

Install NPM dependencies:
```bash
npm install lenis
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
