<!-- Editorial Image Hero · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/hero-07
     license: MIT · category: hero
     A hero section with a full-width landscape image above and a top-aligned tagline paired with a right-aligned serif headline, description, and call-to-action buttons below. -->

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
components/ui/hero-07.tsx
'use client'

import * as React from 'react'
import { motion, useReducedMotion, type Variants } from 'motion/react'
import Balancer from 'react-wrap-balancer'

import { cn } from '@/lib/utils'

import { Cta, type CtaProps } from '../../shared/cta'

export interface Hero07Props {
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
    copy: 'pb-20 pt-10 sm:pb-28 sm:pt-12 lg:pb-32',
    tagline: 'text-sm sm:text-base',
    title: 'text-3xl sm:text-4xl md:text-5xl',
    description: 'text-sm sm:text-base',
    header: 'gap-6 sm:gap-8',
    grid: 'gap-10',
  },
  compact: {
    copy: 'pb-14 pt-8 sm:pb-20 sm:pt-10 lg:pb-24',
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
  hidden: { opacity: 0, y: -20, filter: 'blur(8px)' },
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

export function Hero07({
  tagline,
  title,
  description,
  landscapeImage,
  landscapeAlt = '',
  animation = 'none',
  primaryCTA,
  secondaryCTA,
  variant = 'standard',
}: Readonly<Hero07Props>) {
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
          'relative overflow-hidden rounded-t-sm',
          'mask-b-from-80% mask-b-to-95%',
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
      <Reveal active={animate} variants={mediaItem} className="w-full">
        {mediaElement}
      </Reveal>

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
          className="flex lg:col-span-4 lg:col-start-1 lg:items-start lg:self-stretch"
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
    </section>
  )
}

components/ui/hero-07-example.tsx
import { Hero07, type Hero07Props } from './hero-07'

export const values = {
  tagline: 'Architecture, interiors, and spaces built to last',
  title: 'Design-led homes for people who care how a place feels.',
  description:
    'From first sketch to final detail, we shape residential projects that balance light, material, and daily life. Thoughtful planning, refined finishes, and a calm build process.',
  landscapeImage:
    'https://images.unsplash.com/photo-1578301978018-3005759f48f7?q=80&w=1144&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
  landscapeAlt: 'Alt',
  animation: 'subtle',
  primaryCTA: {
    ctaEnabled: false,
    text: 'View projects',
    link: '',
    variant: 'default',
  },
  secondaryCTA: {
    ctaEnabled: false,
    text: 'Book a consultation',
    link: '',
    variant: 'link',
  },
} satisfies Hero07Props

export function Hero07Example() {
  return <Hero07 {...values} />
}

demo.tsx
import { Hero07 } from '@/components/ui/hero-07'

export default function Hero07Demo() {
  return (
    <Hero07
      tagline="Architecture, interiors, and spaces built to last"
      title="Design-led homes for people who care how a place feels."
      description="From first sketch to final detail, we shape residential projects that balance light, material, and daily life. Thoughtful planning, refined finishes, and a calm build process."
      landscapeImage="https://cdn.21st.dev/assets/mirror/18/18129f9729d7e9fd5393c61d23efb785da4320478b3a878a3e6dd084d86a229d.jpg"
      landscapeAlt="Modern residential architecture"
      animation="subtle"
      primaryCTA={{ ctaEnabled: false, text: 'View projects', link: '', variant: 'default' }}
      secondaryCTA={{ ctaEnabled: false, text: 'Book a consultation', link: '', variant: 'link' }}
    />
  )
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
