<!-- Sparkles Title · @uilayout.contact · https://21st.dev/@uilayout.contact/components/sparkles-title4
     license: no-license · category: hero
     A hero title section with an animated particle sparkles effect fading out below a grid-lined card and description text. -->

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
components/ui/sparkles-title4.txt
import React from 'react';
import { Sparkles } from '@/components/sparkles';

function index() {
  return (
    <>
      <div className='min-h-screen w-screen overflow-hidden bg-black'>
        <div className='mx-auto pt-32 pb-4 w-screen max-w-2xl relative z-10'>
          <div className='relative h-full w-full bg-white'>
            <div className='absolute bottom-0 left-0 right-0 top-0 bg-[linear-gradient(to_right,#4f4f4f2e_1px,transparent_1px),linear-gradient(to_bottom,#4f4f4f2e_1px,transparent_1px)] bg-size-[14px_24px] mask-[radial-gradient(ellipse_80%_50%_at_50%_0%,#000_70%,transparent_110%)]'></div>
          </div>
          <div className=' w-16 h-16 mx-auto bg-white rounded-lg before:absolute relative before:w-full before:h-full before:bg-white/40 before:rounded-lg before:-top-2 before:-left-2'></div>
          <article className='text-white w-3/4 mx-auto block text-center pt-4 '>
            It is a modern and minimalist UI component library designed to
            simplify the process of building responsive and visually appealing
            web interfaces.
          </article>
        </div>
        <div className='relative h-64 w-screen overflow-hidden '>
          <div className='absolute inset-x-20 top-0 bg-linear-to-r from-transparent via-neutral-500 to-transparent h-[2px] w-2/4 mx-auto blur-xs' />
          <div className='absolute inset-x-20 top-0 bg-linear-to-r from-transparent via-neutral-400 to-transparent h-px w-2/4 mx-auto' />
          <div className='absolute inset-x-20 top-0 bg-linear-to-r from-transparent via-neutral-50 to-transparent h-px w-1/4 mx-auto' />

          <Sparkles
            density={1200}
            mousemove={true}
            className='absolute inset-x-0 -mt-24 top-0 h-full w-full mask-[radial-gradient(50%_50%,white,transparent_55%)]'
          />
        </div>
      </div>
    </>
  );
}

export default index;

components/ui/sparkles.txt
'use client';

import { useEffect, useId, useState } from 'react';
import Particles, { initParticlesEngine } from '@tsparticles/react';
import { loadSlim } from '@tsparticles/slim';

interface SparklesProps {
  className?: string;
  size?: number;
  minSize?: number | null;
  density?: number;
  speed?: number;
  minSpeed?: number | null;
  opacity?: number;
  direction?: string;
  opacitySpeed?: number;
  minOpacity?: number | null;
  color?: string;
  mousemove?: boolean;
  hover?: boolean;
  background?: string;
  options?: Record<string, any>; // Adjust type as needed based on `options` structure
}

export function Sparkles({
  className,
  size = 1.2,
  minSize = null,
  density = 800,
  speed = 1.5,
  minSpeed = null,
  opacity = 1,
  direction = '',
  opacitySpeed = 3,
  minOpacity = null,
  color = '#ffffff',
  mousemove = false,
  hover = false,
  background = 'transparent',
  options = {},
}: SparklesProps) {
  const [isReady, setIsReady] = useState(false);

  useEffect(() => {
    initParticlesEngine(async (engine) => {
      await loadSlim(engine);
    }).then(() => {
      setIsReady(true);
    });
  }, []);

  const id = useId();
  const defaultOptions = {
    background: {
      color: {
        value: background,
      },
    },
    fullScreen: {
      enable: false,
      zIndex: 1,
    },
    fpsLimit: 300,

    interactivity: {
      events: {
        onClick: {
          enable: true,
          mode: 'push',
        },
        onHover: {
          enable: hover,
          mode: 'grab',
          parallax: {
            enable: mousemove,
            force: 60,
            smooth: 10,
          },
        },
        resize: true as any,
      },
      modes: {
        push: {
          quantity: 4,
        },
        repulse: {
          distance: 200,
          duration: 0.4,
        },
      },
    },
    particles: {
      color: {
        value: color,
      },
      move: {
        enable: true,
        direction,
        speed: {
          min: minSpeed || speed / 130,
          max: speed,
        },
        straight: true,
      },
      collisions: {
        absorb: {
          speed: 2,
        },
        bounce: {
          horizontal: {
            value: 1,
          },
          vertical: {
            value: 1,
          },
        },
        enable: false,
        maxSpeed: 50,
        mode: 'bounce',
        overlap: {
          enable: true,
          retries: 0,
        },
      },
      number: {
        value: density,
      },
      opacity: {
        value: {
          min: minOpacity || opacity / 10,
          max: opacity,
        },
        animation: {
          enable: true,
          sync: false,
          speed: opacitySpeed,
        },
      },
      size: {
        value: {
          min: minSize || size / 1.5,
          max: size,
        },
      },
    },
    detectRetina: true,
  };
  return (
    isReady && (
      <Particles id={id} 
      // @ts-nocheck
      options={defaultOptions} className={className} />
    )
  );
}

demo.tsx
import SparklesTitle4 from "@/components/ui/sparkles-title4";

export default function Default() {
  return <SparklesTitle4 />;
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
