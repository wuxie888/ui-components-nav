<!-- Centered Serif Hero · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/hero-01
     license: agpl-3.0 · category: hero
     A centered serif hero section with a soft gradient wash background, pill call-to-action button, and a floating integration logo cloud. -->

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
components/ui/hero-01.tsx
'use client'

import * as React from 'react'
import { motion, useReducedMotion, type Variants } from 'motion/react'
import Balancer from 'react-wrap-balancer'

import { cn } from '@/lib/utils'

import { Cta, type CtaProps } from '../../shared/cta'
import { IntegrationCloud } from './integration-cloud'

export interface Hero01Props {
  title: string
  titleLine2?: string
  description: string
  washImage: string
  animation?: 'none' | 'subtle'
  primaryCTA: CtaProps
  integrationRows: string[][]
  variant?: 'standard' | 'compact'
}

const variantStyles = {
  standard: {
    section: 'py-20 sm:py-28',
    title: 'text-3xl sm:text-4xl md:text-5xl',
    description: 'max-w-md text-sm sm:text-base',
    content: 'gap-8',
  },
  compact: {
    section: 'py-14 sm:py-20',
    title: 'text-2xl sm:text-3xl md:text-4xl',
    description: 'max-w-sm text-sm',
    content: 'gap-6',
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

function Reveal({
  active,
  className,
  children,
}: Readonly<{
  active: boolean
  className?: string
  children: React.ReactNode
}>) {
  if (!active) return <div className={className}>{children}</div>

  return (
    <motion.div variants={item} className={className}>
      {children}
    </motion.div>
  )
}

export function Hero01({
  title,
  titleLine2,
  description,
  washImage,
  animation = 'none',
  primaryCTA,
  integrationRows,
  variant = 'standard',
}: Readonly<Hero01Props>) {
  const reduce = useReducedMotion()
  const animate = animation === 'subtle' && !reduce
  const vs = variantStyles[variant]

  const backgroundElement = washImage && (
    <div
      aria-hidden
      className="pointer-events-none absolute inset-x-0 top-0 z-0 mx-auto h-full w-full mask-t-from-60% mask-t-to-90% mask-b-from-75% mask-b-to-85% mask-radial-[70%_70%] mask-radial-from-60% mask-radial-to-90% mask-radial-at-top opacity-50 md:mask-radial-[70%_90%] dark:opacity-10"
    >
      <img
        src={washImage}
        alt=""
        className="absolute inset-0 size-full object-cover object-top"
      />
      <div className="bg-background/30 dark:bg-background/45 absolute inset-0" />
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

  const ctaElement = <Cta cta={primaryCTA} />

  const illustrationElement = (
    <IntegrationCloud rows={integrationRows} />
  )

  return (
    <section className="bg-background relative isolate w-full overflow-hidden">
      {backgroundElement}

      <motion.div
        className={cn(
          'relative z-10 mx-auto flex max-w-3xl flex-col items-center px-6 text-center',
          vs.section,
          vs.content,
        )}
        variants={animate ? container : undefined}
        initial={animate ? 'hidden' : false}
        whileInView={animate ? 'visible' : undefined}
        viewport={{ once: true, margin: '-80px' }}
      >
        <Reveal active={animate} className="flex flex-col items-center gap-5">
          {titleElement}
          {descriptionElement}
        </Reveal>

        <Reveal active={animate}>{ctaElement}</Reveal>

        <Reveal active={animate} className="w-full">
          {illustrationElement}
        </Reveal>
      </motion.div>
    </section>
  )
}

components/ui/hero-01-example.tsx
import { Hero01, type Hero01Props } from './hero-01'

export const values = {
  title: 'Build what matters.',
  titleLine2: 'Connect what works.',
  description:
    'A single layer for payments, auth, and messaging in your product.',
  washImage:
    'https://images.unsplash.com/photo-1578301978018-3005759f48f7?q=80&w=1144&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
  animation: 'subtle',
  primaryCTA: {
    ctaEnabled: true,
    text: 'Get started',
    link: '',
    variant: 'default',
    size: 'default',
  },
  integrationRows: [
    ['Notion', 'GitHub', 'Stripe', 'Figma'],
    ['Supabase', 'Resend', 'Raycast'],
  ],
} satisfies Hero01Props

export function Hero01Example() {
  return <Hero01 {...values} />
}

components/ui/integration-cloud.tsx
export interface IntegrationCloudProps {
  rows: readonly (readonly string[])[]
}

export function IntegrationCloud({ rows }: Readonly<IntegrationCloudProps>) {
  if (rows.length === 0) return null

  return (
    <div
      aria-hidden
      className="flex w-full flex-col items-center gap-3 mask-x-from-90% mask-x-to-100% pt-4 pb-1"
    >
      {rows.map((row, rowIndex) => (
        <div
          key={rowIndex}
          className="flex items-center justify-center gap-3 sm:gap-4"
        >
          {row.map((name, nameIndex) => (
            <div
              key={nameIndex}
              className="border-border/50 bg-card/90 text-foreground/70 shrink-0 rounded-full border px-3.5 py-1.5 text-xs font-medium whitespace-nowrap shadow-sm"
            >
              {name}
            </div>
          ))}
        </div>
      ))}
    </div>
  )
}

demo.tsx
import { Hero01 } from '@/components/ui/hero-01'

export default function Hero01Example() {
  return (
    <Hero01
      title="Build what matters."
      titleLine2="Connect what works."
      description="A single layer for payments, auth, and messaging in your product."
      washImage="https://cdn.21st.dev/assets/mirror/18/18129f9729d7e9fd5393c61d23efb785da4320478b3a878a3e6dd084d86a229d.jpg"
      animation="subtle"
      primaryCTA={{
        ctaEnabled: true,
        text: 'Get started',
        link: '',
        variant: 'default',
        size: 'default',
      }}
      integrationRows={[
        ['Notion', 'GitHub', 'Stripe', 'Figma'],
        ['Supabase', 'Resend', 'Raycast'],
      ]}
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
