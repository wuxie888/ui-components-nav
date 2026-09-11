<!-- Scroll 01 · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/scroll-01
     license: MIT · category: features
     A scroll-driven section with sticky media on one side that swaps images as the reader scrolls through the accompanying text content. -->

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
components/ui/scroll-01.tsx
'use client'

import {
  motion,
  useScroll,
  useTransform,
  useMotionValueEvent,
} from 'motion/react'
import { useRef, useState, Dispatch, SetStateAction } from 'react'

type Scroll01Item = {
  title: string
  description: string
  media: string
}

export interface Scroll01Props {
  items: Scroll01Item[]
}

function ScrollItem({
  item,
  index,
  setActive,
}: {
  item: Scroll01Item
  index: number
  setActive: Dispatch<SetStateAction<number>>
}) {
  const ref = useRef<HTMLDivElement | null>(null)

  const { scrollYProgress } = useScroll({
    target: ref,
    offset: ['start 90%', 'end 15%'],
  })

  const y = useTransform(scrollYProgress, [0, 1], [20, -20])

  const opacityValues = index === 0 ? [1, 0.7, 1, 0] : [0, 0.7, 1, 0]
  const opacity = useTransform(scrollYProgress, [0, 0.3, 0.7, 1], opacityValues)

  const isActive = useTransform(scrollYProgress, (v) => v > 0.4 && v < 0.6)

  useMotionValueEvent(isActive, 'change', (v) => {
    if (v) {
      setActive((prev) => (prev === index ? prev : index))
    }
  })

  return (
    <motion.article
      ref={ref}
      style={{ opacity, y }}
      className="flex flex-col items-center"
    >
      <div className="text-center">
        <h3 className="mb-2 text-2xl font-semibold">{item.title}</h3>
        <p className="text-muted-foreground">{item.description}</p>
      </div>
    </motion.article>
  )
}

export function Scroll01({ items }: Readonly<Scroll01Props>) {
  const [activeIndex, setActiveIndex] = useState<number>(0)

  return (
    <>
      <div className="space-y-10 md:hidden">
        {items.map((item, index) => (
          <article
            key={`${item.title}-${index}`}
            className="flex flex-col items-start space-y-4"
          >
            <div className="space-y-2">
              <h3 className="text-2xl font-semibold">{item.title}</h3>
              <p className="text-muted-foreground">{item.description}</p>
            </div>
            <img
              src={item.media}
              alt={item.title}
              className="h-72 w-full rounded-2xl object-cover"
            />
          </article>
        ))}
      </div>

      <div className="hidden gap-10 md:grid md:grid-cols-2">
        <div className="sticky top-20 max-h-[70vh] overflow-hidden rounded-2xl">
          {items.map((item, index) => (
            <motion.img
              key={`${item.title}-${index}`}
              src={item.media}
              alt={item.title}
              className="absolute inset-0 aspect-4/3 h-full w-full object-cover"
              initial={{ opacity: index === 0 ? 1 : 0 }}
              animate={{
                opacity: activeIndex === index ? 1 : 0,
                willChange: 'opacity',
              }}
              transition={{
                duration: 0.15,
                ease: 'linear',
              }}
            />
          ))}
        </div>

        <div className="py-[35vh]">
          <div className="space-y-[30vh]">
            {items.map((item, index) => (
              <ScrollItem
                key={`${item.title}-${index}`}
                item={item}
                index={index}
                setActive={setActiveIndex}
              />
            ))}
          </div>
        </div>
      </div>
    </>
  )
}

components/ui/scroll-01-example.tsx
import {
  Scroll01,
  type Scroll01Props,
} from './scroll-01'

export const values = {
  items: [
    {
      title: 'Build faster',
      description: 'Create interfaces quickly using reusable blocks.',
      media:
        'https://images.unsplash.com/photo-1486092642310-0c4e84309adb?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    },
    {
      title: 'Customize easily',
      description: 'Adapt everything to your design system.',
      media:
        'https://images.unsplash.com/photo-1479707406242-e8929e87e734?q=80&w=1170&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    },
    {
      title: 'Stay consistent',
      description: 'Keep layouts balanced with a clear visual rhythm.',
      media:
        'https://images.unsplash.com/photo-1628880689946-f4a0533ac5fc?q=80&w=1170&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    },
    {
      title: 'Guide attention',
      description: 'Highlight the right content without adding noise.',
      media:
        'https://images.unsplash.com/photo-1571495653425-621ba3cb7ac1?q=80&w=1170&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    },
    {
      title: 'Scale calmly',
      description: 'Expand your pages with patterns that stay elegant.',
      media:
        'https://images.unsplash.com/photo-1561990306-7462bfe923b6?q=80&w=1170&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    },
  ],
} satisfies Scroll01Props

export function Scroll01Example() {
  return (
    <div>
      <div className="h-10" />
      <Scroll01 items={values.items} />
      <div className="h-30" />
    </div>
  )
}

demo.tsx
import { Scroll01 } from "@/components/ui/scroll-01";

const values = {
  items: [
    {
      title: "Build faster",
      description: "Create interfaces quickly using reusable blocks.",
      media:
        "https://cdn.21st.dev/assets/mirror/d3/d3cc531dd08ff828c572ac43c22057d34e7f4a48e86ee12a178f86073e6caf5a.jpg",
    },
    {
      title: "Customize easily",
      description: "Adapt everything to your design system.",
      media:
        "https://cdn.21st.dev/assets/mirror/f7/f75faa3b90f7f605bb660b0497013d0b44d57f97dc1bba97412521ee8f816efb.jpg",
    },
    {
      title: "Stay consistent",
      description: "Keep layouts balanced with a clear visual rhythm.",
      media:
        "https://cdn.21st.dev/assets/mirror/f2/f247ca8274d119e669ef1a78047884608b7818d425a4f76f3d66915f0ccac13c.jpg",
    },
    {
      title: "Guide attention",
      description: "Highlight the right content without adding noise.",
      media:
        "https://cdn.21st.dev/assets/mirror/6d/6d3292ee7e52514c2496d5e01f47a49901a5e9503271c027139a69489e6b2fdb.jpg",
    },
    {
      title: "Scale calmly",
      description: "Expand your pages with patterns that stay elegant.",
      media:
        "https://cdn.21st.dev/assets/mirror/2f/2f902ca3b242544d0a9e33a7824fc30cac31e9121fd2713c89e0aeeec0363a58.jpg",
    },
  ],
};

export default function Scroll01Demo() {
  return (
    <div className="mx-auto max-w-4xl px-4">
      <div className="h-10" />
      <Scroll01 items={values.items} />
      <div className="h-32" />
    </div>
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
