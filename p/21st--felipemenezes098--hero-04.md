<!-- Editorial Collage Hero · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/hero-04
     license: no-license · category: hero
     A two-column hero section with a serif headline, description, call-to-action buttons, and a layered collage of two overlapping images over a soft background wash. -->

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
components/ui/hero-04.tsx
'use client'

import * as React from 'react'
import { motion, useReducedMotion, type Variants } from 'motion/react'
import Balancer from 'react-wrap-balancer'

import { cn } from '@/lib/utils'

import { Cta, type CtaProps } from '../../shared/cta'
import { ArtCollage } from './art-collage'

export interface Hero04Props {
  title: string
  washImage?: string
  titleLine2?: string
  description: string
  primaryImage: string
  secondaryImage: string
  primaryAlt?: string
  secondaryAlt?: string
  animation?: 'none' | 'subtle'
  primaryCTA: CtaProps
  secondaryCTA?: CtaProps
  variant?: 'standard' | 'compact'
}

const variantStyles = {
  standard: {
    section: 'py-20 sm:py-28',
    title: 'text-3xl sm:text-4xl md:text-5xl',
    description: 'max-w-md text-sm sm:text-base',
    header: 'gap-5',
    grid: 'gap-12 lg:gap-16',
  },
  compact: {
    section: 'py-14 sm:py-20',
    title: 'text-2xl sm:text-3xl md:text-4xl',
    description: 'max-w-sm text-sm',
    header: 'gap-4',
    grid: 'gap-10 lg:gap-12',
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

export function Hero04({
  title,
  titleLine2,
  description,
  washImage,
  primaryImage,
  secondaryImage,
  primaryAlt = '',
  secondaryAlt = '',
  animation = 'none',
  primaryCTA,
  secondaryCTA,
  variant = 'standard',
}: Readonly<Hero04Props>) {
  const reduce = useReducedMotion()
  const animate = animation === 'subtle' && !reduce
  const vs = variantStyles[variant]

  const backgroundElement = washImage && (
    <div
      aria-hidden
      className="pointer-events-none absolute inset-0 z-0 aspect-2/3 mask-radial-[75%_100%] mask-radial-from-45% mask-radial-to-75% mask-radial-at-top opacity-75 blur-xl md:aspect-square lg:aspect-video dark:opacity-5"
    >
      <img
        src={washImage}
        alt=""
        className="h-full w-full object-cover object-top"
      />
    </div>
  )

  const titleElement = title && (
    <h1
      className={cn(
        'text-foreground font-serif font-normal tracking-tight text-balance',
        vs.title,
      )}
    >
      <Balancer>{title}</Balancer>
      {titleLine2 && (
        <>
          <br />
          <Balancer>{titleLine2}</Balancer>
        </>
      )}
    </h1>
  )

  const descriptionElement = description && (
    <p className={cn('text-muted-foreground', vs.description)}>
      <Balancer>{description}</Balancer>
    </p>
  )

  const ctasElement = (primaryCTA?.ctaEnabled || secondaryCTA?.ctaEnabled) && (
    <div className="mt-2 flex flex-wrap items-center gap-x-4 gap-y-3">
      {primaryCTA?.ctaEnabled && <Cta cta={primaryCTA} />}
      {secondaryCTA?.ctaEnabled && (
        <Cta
          cta={{ ...secondaryCTA, variant: secondaryCTA.variant ?? 'link' }}
        />
      )}
    </div>
  )

  const mediaElement = (
    <ArtCollage
      primaryImage={primaryImage}
      secondaryImage={secondaryImage}
      primaryAlt={primaryAlt}
      secondaryAlt={secondaryAlt}
    />
  )

  return (
    <section className="bg-background relative isolate w-full overflow-hidden">
      {backgroundElement}

      <motion.div
        className={cn(
          'relative z-10 mx-auto grid max-w-6xl grid-cols-1 items-center px-6 lg:grid-cols-2',
          vs.section,
          vs.grid,
        )}
        variants={animate ? container : undefined}
        initial={animate ? 'hidden' : false}
        whileInView={animate ? 'visible' : undefined}
        viewport={{ once: true, margin: '-80px' }}
      >
        <Reveal
          active={animate}
          className={cn('flex flex-col items-start', vs.header)}
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

components/ui/hero-04-example.tsx
import { Hero04, type Hero04Props } from './hero-04'

export const values = {
  title: 'A gallery for the work',
  titleLine2: 'you are proud of.',
  description:
    'Collect, arrange, and publish your art in a space that feels like a studio, not a spreadsheet.',
  washImage:
    'https://images.unsplash.com/photo-1685013640715-8701bbaa2207?q=80&w=2198&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
  primaryImage:
    'https://images.unsplash.com/photo-1746467364902-ab40952e33fe?q=80&w=1131&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
  secondaryImage:
    'https://images.unsplash.com/photo-1578301978018-3005759f48f7?q=80&w=1144&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
  primaryAlt: 'Featured artwork',
  secondaryAlt: 'Abstract artwork',
  animation: 'subtle',
  primaryCTA: {
    ctaEnabled: true,
    text: 'Start your gallery',
    link: '',
    variant: 'default',
    size: 'default',
  },
  secondaryCTA: {
    ctaEnabled: true,
    text: 'See examples',
    link: '',
    variant: 'link',
  },
} satisfies Hero04Props

export function Hero04Example() {
  return <Hero04 {...values} />
}

components/ui/art-collage.tsx
export function ArtCollage({
  primaryImage,
  secondaryImage,
  primaryAlt = '',
  secondaryAlt = '',
}: Readonly<{
  primaryImage: string
  secondaryImage: string
  primaryAlt?: string
  secondaryAlt?: string
}>) {
  return (
    <div className="relative mx-auto w-full max-w-md sm:max-w-lg">
      <div
        aria-hidden
        className="bg-primary/10 pointer-events-none absolute -inset-6 -z-10 rounded-full opacity-40 blur-3xl dark:opacity-25"
      />

      <div className="relative aspect-[4/5] w-[82%] overflow-hidden rounded-2xl shadow-xl outline outline-black/10 dark:outline-white/10">
        <img
          src={primaryImage}
          alt={primaryAlt}
          decoding="async"
          className="size-full object-cover"
        />
        <div className="from-background/20 absolute inset-0 bg-gradient-to-t to-transparent" />
      </div>

      <div className="absolute -right-2 bottom-6 aspect-square w-[42%] rotate-3 overflow-hidden rounded-xl shadow-lg outline outline-black/10 sm:-right-4 dark:outline-white/10">
        <img
          src={secondaryImage}
          alt={secondaryAlt}
          decoding="async"
          className="size-full object-cover"
        />
      </div>
    </div>
  )
}

demo.tsx
import { Hero04, type Hero04Props } from '@/components/ui/hero-04'

const values = {
  title: 'A gallery for the work',
  titleLine2: 'you are proud of.',
  description:
    'Collect, arrange, and publish your art in a space that feels like a studio, not a spreadsheet.',
  washImage:
    'https://cdn.21st.dev/assets/mirror/a1/a17b190eb8f6463b5d28be076a83f47ac86f2a759fb2295a763ad6b8616a610b.jpg',
  primaryImage:
    'https://cdn.21st.dev/assets/mirror/83/839c9582581e0c20b8fbd2ad66e9c87c31206f709d28534d411674d10c913757.jpg',
  secondaryImage:
    'https://cdn.21st.dev/assets/mirror/18/18129f9729d7e9fd5393c61d23efb785da4320478b3a878a3e6dd084d86a229d.jpg',
  primaryAlt: 'Featured artwork',
  secondaryAlt: 'Abstract artwork',
  animation: 'subtle',
  primaryCTA: {
    ctaEnabled: true,
    text: 'Start your gallery',
    link: '',
    variant: 'default',
    size: 'default',
  },
  secondaryCTA: {
    ctaEnabled: true,
    text: 'See examples',
    link: '',
    variant: 'link',
  },
} satisfies Hero04Props

export default function Hero04Demo() {
  return <Hero04 {...values} />
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
