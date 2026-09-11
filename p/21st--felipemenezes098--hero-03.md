<!-- Hero 03 · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/hero-03
     license: no-license · category: hero
     A centered hero section with a serif headline, dual call-to-action buttons, and a portrait image that fades into the page below the copy. -->

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
components/ui/hero-03.tsx
'use client'

import * as React from 'react'
import { motion, useReducedMotion, type Variants } from 'motion/react'
import Balancer from 'react-wrap-balancer'

import { cn } from '@/lib/utils'

import { Cta, type CtaProps } from '../../shared/cta'

export interface Hero03Props {
  title: string
  description: string
  portraitImage: string
  portraitAlt?: string
  animation?: 'none' | 'subtle'
  primaryCTA: CtaProps
  secondaryCTA?: CtaProps
  variant?: 'standard' | 'compact'
}

const variantStyles = {
  standard: {
    section: 'py-20 sm:py-28',
    title: 'text-3xl sm:text-4xl md:text-5xl',
    description: 'mx-auto max-w-lg text-sm sm:text-base leading-relaxed',
    header: 'gap-5',
    content: 'gap-14 sm:gap-20',
    portrait: 'max-w-3xl',
  },
  compact: {
    section: 'py-14 sm:py-20',
    title: 'text-2xl sm:text-3xl md:text-4xl',
    description: 'mx-auto max-w-md text-sm leading-relaxed',
    header: 'gap-4',
    content: 'gap-10 sm:gap-14',
    portrait: 'max-w-2xl',
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
    transition: { duration: 0.5, ease: [0.22, 1, 0.36, 1] },
  },
}

const mediaItem: Variants = {
  hidden: { opacity: 0, y: 24, filter: 'blur(8px)' },
  visible: {
    opacity: 1,
    y: 0,
    filter: 'blur(0px)',
    transition: { duration: 0.6, ease: [0.22, 1, 0.36, 1] },
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

export function Hero03({
  title,
  description,
  portraitImage,
  portraitAlt = '',
  animation = 'none',
  primaryCTA,
  secondaryCTA,
  variant = 'standard',
}: Readonly<Hero03Props>) {
  const reduce = useReducedMotion()
  const animate = animation === 'subtle' && !reduce
  const vs = variantStyles[variant]

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
    <p className={cn('text-muted-foreground', vs.description)}>
      <Balancer>{description}</Balancer>
    </p>
  )

  const ctasElement = (primaryCTA?.ctaEnabled || secondaryCTA?.ctaEnabled) && (
    <div className="flex flex-wrap items-center justify-center gap-x-5 gap-y-3">
      {primaryCTA?.ctaEnabled && <Cta cta={primaryCTA} />}
      {secondaryCTA?.ctaEnabled && (
        <Cta
          cta={{ ...secondaryCTA, variant: secondaryCTA.variant ?? 'link' }}
        />
      )}
    </div>
  )

  const mediaElement = portraitImage && (
    <div className={cn('relative mx-auto w-full', vs.portrait)}>
      <div
        className={cn(
          'relative z-10 mx-auto w-full overflow-hidden',
          'mask-x-from-75% mask-x-to-100%',
          'mask-t-from-55% mask-t-to-100%',
          'mask-b-from-55% mask-b-to-100%',
          'mask-radial-[80%_70%] mask-radial-from-70% mask-radial-to-100% mask-radial-at-center',
          'dark:opacity-85 dark:mix-blend-darken',
        )}
      >
        <div
          aria-hidden
          className="bg-background/25 dark:bg-background/40 pointer-events-none absolute inset-0 mix-blend-overlay"
        />
        <img
          src={portraitImage}
          alt={portraitAlt}
          decoding="async"
          className="relative aspect-[5/4] w-full object-cover object-[center_15%] dark:mix-blend-lighten dark:brightness-[0.92] dark:contrast-[1.05] dark:saturate-[0.9]"
        />
      </div>
    </div>
  )

  return (
    <section className="bg-background relative isolate w-full overflow-hidden">
      <motion.div
        className={cn(
          'relative z-10 mx-auto flex max-w-6xl flex-col px-6',
          vs.section,
          vs.content,
        )}
        variants={animate ? container : undefined}
        initial={animate ? 'hidden' : false}
        whileInView={animate ? 'visible' : undefined}
        viewport={{ once: true, margin: '-80px' }}
      >
        <Reveal
          active={animate}
          className={cn(
            'mx-auto flex w-full max-w-2xl flex-col items-center text-center',
            vs.header,
          )}
        >
          {titleElement}
          {descriptionElement}
          {ctasElement}
        </Reveal>

        <Reveal active={animate} variants={mediaItem} className="w-full">
          {mediaElement}
        </Reveal>
      </motion.div>
    </section>
  )
}

components/ui/hero-03-example.tsx
import { Hero03, type Hero03Props } from './hero-03'

export const values = {
  title: 'Ideas worth sharing with the world.',
  description:
    'Turn rough notes into polished stories. Write, refine, and publish from one calm workspace built for focus.',
  portraitImage:
    'https://images.unsplash.com/photo-1746467364902-ab40952e33fe?q=80&w=1131&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
  portraitAlt: 'Alt',
  animation: 'subtle',
  primaryCTA: {
    ctaEnabled: true,
    text: 'Get started',
    link: '',
    variant: 'default',
    size: 'default',
  },
  secondaryCTA: {
    ctaEnabled: true,
    text: 'Learn more',
    link: '',
    variant: 'link',
  },
} satisfies Hero03Props

export function Hero03Example() {
  return <Hero03 {...values} />
}

demo.tsx
import { Hero03 } from '@/components/ui/hero-03'

export default function Hero03Demo() {
  return (
    <Hero03
      title="Ideas worth sharing with the world."
      description="Turn rough notes into polished stories. Write, refine, and publish from one calm workspace built for focus."
      portraitImage="https://cdn.21st.dev/assets/mirror/83/839c9582581e0c20b8fbd2ad66e9c87c31206f709d28534d411674d10c913757.jpg"
      portraitAlt="Alt"
      animation="subtle"
      primaryCTA={{ ctaEnabled: true, text: 'Get started', link: '', variant: 'default', size: 'default' }}
      secondaryCTA={{ ctaEnabled: true, text: 'Learn more', link: '', variant: 'link' }}
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
