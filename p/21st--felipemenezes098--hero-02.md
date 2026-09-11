<!-- Hero with Dashboard Mockup · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/hero-02
     license: agpl-3.0 · category: hero
     A left-aligned serif hero section with a media panel showing an image backdrop and a floating dashboard mockup, plus an animated call-to-action. -->

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
components/ui/hero-02.tsx
'use client'

import * as React from 'react'
import { motion, useReducedMotion, type Variants } from 'motion/react'
import Balancer from 'react-wrap-balancer'

import { cn } from '@/lib/utils'

import { Cta, type CtaProps } from '../../shared/cta'
import { DashboardDemo } from './dashboard-demo'

export interface Hero02Props {
  title: string
  titleLine2?: string
  description: string
  washImage: string
  animation?: 'none' | 'subtle'
  primaryCTA: CtaProps
  variant?: 'standard' | 'compact'
}

const variantStyles = {
  standard: {
    section: 'py-20 sm:py-28',
    title: 'text-3xl sm:text-4xl md:text-5xl',
    description: 'max-w-md text-sm sm:text-base',
    header: 'gap-5',
    content: 'gap-14 sm:gap-20',
  },
  compact: {
    section: 'py-14 sm:py-20',
    title: 'text-2xl sm:text-3xl md:text-4xl',
    description: 'max-w-sm text-sm',
    header: 'gap-4',
    content: 'gap-10 sm:gap-14',
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

export function Hero02({
  title,
  titleLine2,
  description,
  washImage,
  animation = 'none',
  primaryCTA,
  variant = 'standard',
}: Readonly<Hero02Props>) {
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

  const mediaElement = (
    <div className="relative w-full overflow-hidden rounded-md outline outline-black/10 dark:outline-white/10">
      {washImage && (
        <img
          src={washImage}
          alt=""
          aria-hidden
          className="absolute inset-0 size-full object-cover"
        />
      )}
      <div className="from-background/30 via-background/10 to-background/40 absolute inset-0 bg-gradient-to-b" />
      <div className="relative flex items-center justify-center px-6 py-12 sm:px-12 sm:py-16">
        <DashboardDemo />
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
          className={cn('flex max-w-2xl flex-col items-start', vs.header)}
        >
          {titleElement}
          {descriptionElement}
          {ctaElement}
        </Reveal>

        <Reveal active={animate} variants={mediaItem} className="w-full">
          {mediaElement}
        </Reveal>
      </motion.div>
    </section>
  )
}

components/ui/hero-02-example.tsx
import { Hero02, type Hero02Props } from './hero-02'

export const values = {
  title: 'Every metric that matters,',
  titleLine2: 'in one clear view.',
  description:
    'Track revenue, users, and activity in real time, with no setup and no spreadsheets.',
  washImage:
    'https://images.unsplash.com/photo-1578301978018-3005759f48f7?q=80&w=1144&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
  animation: 'subtle',
  primaryCTA: {
    ctaEnabled: true,
    text: 'Start free',
    link: '',
    variant: 'default',
    size: 'default',
  },
} satisfies Hero02Props

export function Hero02Example() {
  return <Hero02 {...values} />
}

components/ui/dashboard-demo.tsx
import {
  Activity,
  BarChart3,
  Bell,
  LayoutGrid,
  Search,
  Settings,
  Users,
} from 'lucide-react'

import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar'
import { Badge } from '@/components/ui/badge'
import { Button } from '@/components/ui/button'
import { Card } from '@/components/ui/card'
import { Separator } from '@/components/ui/separator'

const nav = [
  { icon: LayoutGrid, label: 'Overview', active: true },
  { icon: BarChart3, label: 'Reports', active: false },
  { icon: Users, label: 'Customers', active: false },
  { icon: Activity, label: 'Activity', active: false },
  { icon: Settings, label: 'Settings', active: false },
]

const stats = [
  { label: 'Revenue', value: '$48.2k', trend: '+12.4%', up: true },
  { label: 'Active users', value: '2,318', trend: '+4.1%', up: true },
  { label: 'Conversion', value: '3.6%', trend: '+0.8%', up: true },
  { label: 'Churn', value: '1.4%', trend: '-2.0%', up: false },
]

const bars = [42, 58, 36, 64, 48, 72, 55, 80, 61, 88, 70, 94]

const activity = [
  {
    src: 'https://images.unsplash.com/photo-1500648767791-00dcc994a43e?w=64&q=80',
    name: 'James Doe',
    action: 'upgraded to Pro',
    time: '2m',
  },
  {
    src: 'https://images.unsplash.com/photo-1494790108377-be9c29b29330?w=64&q=80',
    name: 'Sara Lin',
    action: 'invited 3 members',
    time: '18m',
  },
  {
    src: 'https://images.unsplash.com/photo-1633332755192-727a05c4013d?w=64&q=80',
    name: 'Marcus Vale',
    action: 'connected Stripe',
    time: '1h',
  },
]

export function DashboardDemo() {
  return (
    <div className="relative w-full max-w-4xl">
      <Card className="bg-card w-full gap-0 overflow-hidden p-0 shadow-2xl outline outline-black/10 dark:outline-white/10">
        {/* Window chrome */}
        <div className="flex items-center gap-2 border-b px-3 py-2.5">
          <div className="flex shrink-0 gap-1.5">
            <span className="size-2.5 rounded-full bg-red-400/70" />
            <span className="size-2.5 rounded-full bg-amber-400/70" />
            <span className="size-2.5 rounded-full bg-emerald-400/70" />
          </div>
          <div className="text-muted-foreground bg-muted mx-auto flex h-6 w-full max-w-56 min-w-0 items-center gap-1.5 rounded-md px-2 text-[10px]">
            <Search className="size-3 shrink-0" />
            <span className="truncate">app.acme.com/overview</span>
          </div>
          <Avatar className="size-6 shrink-0">
            <AvatarImage
              src="https://images.unsplash.com/photo-1500648767791-00dcc994a43e?w=64&q=80"
              alt=""
            />
            <AvatarFallback className="text-[9px]">JD</AvatarFallback>
          </Avatar>
        </div>

        <div className="flex">
          {/* Sidebar */}
          <div className="hidden w-44 shrink-0 flex-col gap-1 border-r p-3 sm:flex">
            <div className="mb-2 flex items-center gap-2 px-1">
              <div className="bg-primary/85 size-6 rounded-lg" />
              <span className="text-sm font-semibold tracking-tight">Acme</span>
            </div>
            {nav.map(({ icon: Icon, label, active }) => (
              <div
                key={label}
                className={`flex items-center gap-2 rounded-lg px-2 py-1.5 text-xs ${
                  active
                    ? 'bg-muted text-foreground font-medium'
                    : 'text-muted-foreground'
                }`}
              >
                <Icon className="size-3.5 shrink-0" />
                <span className="truncate">{label}</span>
              </div>
            ))}
          </div>

          {/* Main */}
          <div className="flex min-w-0 flex-1 flex-col gap-3 p-3 sm:p-4">
            {/* Header */}
            <div className="flex items-center justify-between gap-2">
              <div className="flex min-w-0 flex-col gap-0.5">
                <span className="truncate text-sm font-semibold tracking-tight">
                  Overview
                </span>
                <span className="text-muted-foreground truncate text-[10px]">
                  Last 30 days · updated 2m ago
                </span>
              </div>
              <div className="flex shrink-0 items-center gap-1.5">
                <Button size="xs" variant="outline">
                  Export
                </Button>
                <Button size="xs">New report</Button>
              </div>
            </div>

            {/* KPI row */}
            <div className="grid grid-cols-2 gap-2 lg:grid-cols-4">
              {stats.map((s) => (
                <div key={s.label} className="min-w-0 rounded-lg border p-2.5">
                  <span className="text-muted-foreground block truncate text-[9px]">
                    {s.label}
                  </span>
                  <div className="mt-1 flex flex-wrap items-baseline justify-between gap-x-1">
                    <span className="text-sm font-semibold tabular-nums">
                      {s.value}
                    </span>
                    <span
                      className={`text-[9px] font-medium tabular-nums ${
                        s.up
                          ? 'text-emerald-600 dark:text-emerald-400'
                          : 'text-red-500 dark:text-red-400'
                      }`}
                    >
                      {s.trend}
                    </span>
                  </div>
                </div>
              ))}
            </div>

            {/* Chart */}
            <div className="rounded-lg border p-3">
              <div className="flex items-center justify-between gap-2">
                <div className="flex min-w-0 flex-col gap-0.5">
                  <span className="truncate text-xs font-medium">
                    Monthly revenue
                  </span>
                  <span className="text-muted-foreground truncate text-[9px]">
                    $48,240 total
                  </span>
                </div>
                <span className="text-muted-foreground flex shrink-0 items-center gap-1 text-[9px]">
                  <span className="bg-primary/80 size-1.5 rounded-full" />
                  This year
                </span>
              </div>

              <div className="mt-3 flex h-20 items-end gap-1.5">
                {bars.map((h, i) => (
                  <div
                    key={i}
                    className="bg-primary/70 min-h-0.5 flex-1 rounded-sm"
                    style={{ height: `${h}%` }}
                  />
                ))}
              </div>
            </div>

            {/* Recent activity */}
            <div className="rounded-lg border p-3">
              <div className="flex items-center justify-between">
                <span className="text-xs font-medium">Recent activity</span>
                <Badge variant="secondary" className="text-[9px]">
                  Live
                </Badge>
              </div>
              <div className="mt-2 flex flex-col">
                {activity.map((a, i) => (
                  <div key={a.name}>
                    {i > 0 && <Separator className="my-2" />}
                    <div className="flex items-center gap-2">
                      <Avatar className="size-6 shrink-0">
                        <AvatarImage src={a.src} alt="" />
                        <AvatarFallback className="text-[9px]">
                          {a.name[0]}
                        </AvatarFallback>
                      </Avatar>
                      <span className="min-w-0 flex-1 truncate text-[11px]">
                        <span className="font-medium">{a.name}</span>
                        <span className="text-muted-foreground">
                          {' '}
                          {a.action}
                        </span>
                      </span>
                      <span className="text-muted-foreground shrink-0 text-[9px] tabular-nums">
                        {a.time}
                      </span>
                    </div>
                  </div>
                ))}
              </div>
            </div>
          </div>
        </div>
      </Card>

      {/* Floating notification */}
      <div className="absolute right-1 bottom-1 max-w-[calc(100%-0.5rem)] sm:-right-4 sm:-bottom-4">
        <Card className="flex-row items-center gap-2.5 p-2.5 shadow-lg">
          <div className="bg-primary/15 text-primary relative flex size-8 shrink-0 items-center justify-center rounded-lg">
            <Bell className="size-4" />
            <span className="border-card absolute -top-0.5 -right-0.5 size-2 rounded-full border-2 bg-emerald-500" />
          </div>
          <div className="flex min-w-0 flex-col">
            <span className="truncate text-xs leading-tight font-medium">
              Payment received
            </span>
            <span className="text-muted-foreground truncate text-[10px] leading-tight">
              $1,290 from Acme Inc.
            </span>
          </div>
          <Badge variant="secondary" className="ml-1 shrink-0 text-[10px]">
            +$1.2k
          </Badge>
        </Card>
      </div>
    </div>
  )
}

demo.tsx
import { Hero02, type Hero02Props } from '@/components/ui/hero-02'

const values = {
  title: 'Every metric that matters,',
  titleLine2: 'in one clear view.',
  description:
    'Track revenue, users, and activity in real time, with no setup and no spreadsheets.',
  washImage:
    'https://cdn.21st.dev/assets/mirror/18/18129f9729d7e9fd5393c61d23efb785da4320478b3a878a3e6dd084d86a229d.jpg',
  animation: 'subtle',
  primaryCTA: {
    ctaEnabled: true,
    text: 'Start free',
    link: '',
    variant: 'default',
    size: 'default',
  },
} satisfies Hero02Props

export default function Hero02Demo() {
  return <Hero02 {...values} />
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion react-wrap-balancer
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar badge button card cta separator
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
