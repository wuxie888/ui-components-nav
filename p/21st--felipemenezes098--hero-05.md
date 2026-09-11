<!-- Editorial Hero · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/hero-05
     license: MIT · category: hero
     An editorial hero section with a left tagline, right-aligned headline and copy, optional CTAs, and a full-width image below. -->

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
components/ui/hero-05.tsx
'use client'

import * as React from 'react'
import { motion, useReducedMotion, type Variants } from 'motion/react'
import Balancer from 'react-wrap-balancer'

import { cn } from '@/lib/utils'

import { Cta, type CtaProps } from '../../shared/cta'

export interface Hero05Props {
  tagline: string
  title: string
  description: string
  landscapeImage: string
  landscapeAlt?: string
  animation?: 'none' | 'subtle'
  primaryCTA?: CtaProps
  secondaryCTA?: CtaProps
  variant?: 'standard' | 'compact'
}

const variantStyles = {
  standard: {
    copy: 'pt-20 pb-10 sm:pt-28 sm:pb-12 lg:pt-32',
    tagline: 'text-sm sm:text-base',
    title: 'text-3xl sm:text-4xl md:text-5xl',
    description: 'text-sm sm:text-base',
    header: 'gap-6 sm:gap-8',
    grid: 'gap-10',
  },
  compact: {
    copy: 'pt-14 pb-8 sm:pt-20 sm:pb-10 lg:pt-24',
    tagline: 'text-sm',
    title: 'text-2xl sm:text-3xl md:text-4xl',
    description: 'text-sm',
    header: 'gap-4 sm:gap-5',
    grid: 'gap-8',
  },
} as const

const container: Variants = {
  hidden: {},
  visible: { transition: { staggerChildren: 0.1, delayChildren: 0.05 } },
}

const item: Variants = {
  hidden: { opacity: 0, y: 12, filter: 'blur(6px)' },
  visible: {
    opacity: 1,
    y: 0,
    filter: 'blur(0px)',
    transition: { duration: 0.28, ease: [0.22, 1, 0.36, 1] },
  },
}

const mediaItem: Variants = {
  hidden: { opacity: 0, y: 20, filter: 'blur(8px)' },
  visible: {
    opacity: 1,
    y: 0,
    filter: 'blur(0px)',
    transition: { duration: 0.32, ease: [0.22, 1, 0.36, 1] },
  },
}

function Reveal({
  active,
  variants,
  className,
  children,
}: Readonly<{
  active: boolean
  variants?: Variants
  className?: string
  children: React.ReactNode
}>) {
  if (!active) return <div className={className}>{children}</div>

  return (
    <motion.div variants={variants ?? item} className={className}>
      {children}
    </motion.div>
  )
}

export function Hero05({
  tagline,
  title,
  description,
  landscapeImage,
  landscapeAlt = '',
  animation = 'none',
  primaryCTA,
  secondaryCTA,
  variant = 'standard',
}: Readonly<Hero05Props>) {
  const reduce = useReducedMotion()
  const animate = animation === 'subtle' && !reduce
  const vs = variantStyles[variant]

  const taglineElement = tagline && (
    <p
      className={cn(
        'text-muted-foreground max-w-xs leading-relaxed tracking-tight',
        vs.tagline,
      )}
    >
      <Balancer>{tagline}</Balancer>
    </p>
  )

  const titleElement = title && (
    <h1
      className={cn(
        'text-foreground font-serif font-normal tracking-tight text-balance',
        vs.title,
      )}
    >
      <Balancer>{title}</Balancer>
    </h1>
  )

  const descriptionElement = description && (
    <p
      className={cn(
        'text-muted-foreground max-w-xl leading-relaxed',
        vs.description,
      )}
    >
      <Balancer>{description}</Balancer>
    </p>
  )

  const ctasElement = (primaryCTA?.ctaEnabled || secondaryCTA?.ctaEnabled) && (
    <div className="flex flex-wrap items-center gap-x-4 gap-y-3">
      {primaryCTA?.ctaEnabled && <Cta cta={primaryCTA} />}
      {secondaryCTA?.ctaEnabled && (
        <Cta
          cta={{ ...secondaryCTA, variant: secondaryCTA.variant ?? 'link' }}
        />
      )}
    </div>
  )

  const mediaElement = landscapeImage && (
    <div className="relative w-full overflow-hidden">
      <div
        className={cn(
          'relative overflow-hidden rounded-b-sm',
          'mask-t-from-80% mask-t-to-95%',
        )}
      >
        <div
          aria-hidden
          className="bg-background/15 dark:bg-background/30 pointer-events-none absolute inset-0 z-10 mix-blend-overlay"
        />
        <img
          src={landscapeImage}
          alt={landscapeAlt}
          decoding="async"
          className="aspect-[2/1] w-full object-cover object-center outline outline-black/10 sm:aspect-[9/4] dark:outline-white/10 dark:brightness-[0.97] dark:saturate-[0.92]"
        />
      </div>
    </div>
  )

  return (
    <section className="bg-background relative isolate w-full overflow-hidden">
      <motion.div
        className={cn(
          'relative z-10 mx-auto grid max-w-7xl grid-cols-1 px-6 lg:grid-cols-12',
          vs.copy,
          vs.grid,
        )}
        variants={animate ? container : undefined}
        initial={animate ? 'hidden' : false}
        whileInView={animate ? 'visible' : undefined}
        viewport={{ once: true, margin: '-80px' }}
      >
        <Reveal
          active={animate}
          className="flex lg:col-span-4 lg:col-start-1 lg:items-end lg:self-stretch"
        >
          {taglineElement}
        </Reveal>

        <Reveal
          active={animate}
          className={cn(
            'flex flex-col items-start lg:col-span-6 lg:col-start-7',
            vs.header,
          )}
        >
          {titleElement}
          {descriptionElement}
          {ctasElement}
        </Reveal>
      </motion.div>

      <Reveal active={animate} variants={mediaItem} className="w-full">
        {mediaElement}
      </Reveal>
    </section>
  )
}

components/ui/hero-05-example.tsx
import { Hero05, type Hero05Props } from './hero-05'

export const values = {
  tagline: 'Brand, product, and story for teams building something new',
  title:
    'A creative studio for founders who want their work to feel considered.',
  description:
    'We help early stage companies turn rough ideas into clear identities, thoughtful digital products, and messaging people remember. Strategy, design, and execution in one place.',
  landscapeImage:
    'https://images.unsplash.com/photo-1578301978018-3005759f48f7?q=80&w=1144&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
  landscapeAlt: 'Alt',
  animation: 'subtle',
  primaryCTA: {
    ctaEnabled: false,
    text: 'Start a project',
    link: '',
    variant: 'default',
  },
  secondaryCTA: {
    ctaEnabled: false,
    text: 'View work',
    link: '',
    variant: 'link',
  },
} satisfies Hero05Props

export function Hero05Example() {
  return <Hero05 {...values} />
}

demo.tsx
import { Hero05, type Hero05Props } from '@/components/ui/hero-05'

const values = {
  tagline: 'Brand, product, and story for teams building something new',
  title:
    'A creative studio for founders who want their work to feel considered.',
  description:
    'We help early stage companies turn rough ideas into clear identities, thoughtful digital products, and messaging people remember. Strategy, design, and execution in one place.',
  landscapeImage:
    'https://cdn.21st.dev/assets/mirror/18/18129f9729d7e9fd5393c61d23efb785da4320478b3a878a3e6dd084d86a229d.jpg',
  landscapeAlt: 'Editorial hero landscape',
  animation: 'subtle',
  primaryCTA: {
    ctaEnabled: false,
    text: 'Start a project',
    link: '',
    variant: 'default',
  },
  secondaryCTA: {
    ctaEnabled: false,
    text: 'View work',
    link: '',
    variant: 'link',
  },
} satisfies Hero05Props

export default function Hero05Example() {
  return <Hero05 {...values} />
}
```

Install NPM dependencies:
```bash
npm install motion react-wrap-balancer
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button cta
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
