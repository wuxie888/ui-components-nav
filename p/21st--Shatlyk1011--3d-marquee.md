<!-- 3D Marquee · @Shatlyk1011 · https://21st.dev/@Shatlyk1011/components/3d-marquee
     license: MIT · category: gallery
     A 3D perspective marquee that displays a grid of images with continuous scrolling animation. -->

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
components/emerald-ui-components/3d-marquee.tsx
'use client'

/**
 * @author: @shatlyk1011
 * @description: A 3D marquee component that rotates images in a 3D space.
 * @version: 1.0.0
 * @date: 2026-02-12
 * @license: MIT
 * @website: https://emerald-ui.com
 */
import { motion } from 'motion/react'
import { cn } from '@/lib/utils'

interface ThreeDMarqueeProps {
  images?: string[]
  className?: string
}

const defaultImages = [
  'https://images.emerald-ui.com/bellakitchenware-com-.webp',
  'https://images.emerald-ui.com/graza.webp',
  'https://images.emerald-ui.com/dolce-gabbana.webp',
  'https://images.emerald-ui.com/screenshot-2026-03-09-at-18.07.53.webp',
  'https://images.emerald-ui.com/screenshot-2026-03-09-at-18.00.33.webp',
  'https://images.emerald-ui.com/www-neonrated-com-.webp',
  'https://images.emerald-ui.com/freshman-tv-.webp',
  'https://images.emerald-ui.com/www-newyorker-com-.webp',
  'https://images.emerald-ui.com/www-zipline-com-.webp',
  'https://images.emerald-ui.com/www-yellowbirdfoods-com-.webp',
  'https://images.emerald-ui.com/cassettemusic.webp',
  'https://images.emerald-ui.com/giga-ai-.webp',
]

const ThreeDMarquee = ({
  images = defaultImages,
  className,
}: ThreeDMarqueeProps) => {
  const chunkSize = Math.ceil(images.length / 3)
  const chunks = Array.from({ length: 3 }, (_, colIndex) => {
    const start = colIndex * chunkSize
    return images.slice(start, start + chunkSize)
  })

  return (
    <div
      className={cn(
        'mx-auto block h-140 w-full overflow-hidden rounded-md max-xl:h-120 max-sm:h-100',
        className
      )}
    >
      <div className='flex size-full items-center justify-center'>
        <div className='aspect-square size-180 shrink-0 scale-135 max-xl:size-full max-xl:scale-110 max-sm:scale-130'>
          <div
            style={{ transform: 'rotateX(45deg) rotateY(0deg) rotateZ(45deg)' }}
            className='relative top-0 right-[-55%] grid size-full origin-top-left grid-cols-3 gap-5 transform-3d max-xl:-top-30 max-xl:right-[-45%] max-sm:top-0 max-sm:gap-2'
          >
            {chunks.map((subarray, colIndex) => (
              <motion.figure
                animate={{ y: colIndex % 2 === 0 ? 60 : -60 }}
                transition={{
                  duration: colIndex % 2 === 0 ? 10 : 15,
                  repeat: Infinity,
                  repeatType: 'reverse',
                }}
                key={colIndex + 'marquee'}
                className='flex flex-col items-start gap-6 max-sm:gap-3'
              >
                {subarray.map((src, imageIndex) => (
                  <div className='relative' key={imageIndex + src}>
                    <img
                      className='aspect-4/3 h-full w-full rounded-lg bg-neutral-100 object-cover select-none dark:bg-neutral-900'
                      key={imageIndex}
                      src={src}
                      draggable={false}
                      alt={`Image ${imageIndex + 1}`}
                    />
                  </div>
                ))}
              </motion.figure>
            ))}
          </div>
        </div>
      </div>
    </div>
  )
}

export default ThreeDMarquee

demo.tsx
import ThreeDMarquee from "../components/ui/3d-marquee";
const imgs = Array.from({length:24}, (_,i) => `https://picsum.photos/seed/${i+1}/400/300`);
export default function Demo() {
  return (
    <div className="min-h-screen bg-background flex items-center justify-center">
      <ThreeDMarquee images={imgs} />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install clsx framer-motion motion tailwind-merge
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
