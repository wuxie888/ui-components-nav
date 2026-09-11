<!-- Coming Soon Launch Teaser · @hirael · https://21st.dev/@hirael/components/coming-soon-02
     license: MIT · category: announcement
     A full-viewport "coming soon" landing section with a badge, animated word-by-word serif headline, sub-copy and a disabled CTA above a glowing sparkle horizon, for a product launch or under-construction page. -->

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
components/ui/coming-soon-02.tsx
// Coming Soon 2 from Hirael <https://hirael.com/blocks/not-found/coming-soon-02>
// MIT · Mohammad Shehadeh · https://github.com/MohammadShehadeh/hirael

'use client';

import * as React from 'react';
import { motion, useReducedMotion } from 'motion/react';

import { cn } from '@/lib/utils';
import { Badge } from '@/registry/hirael/bases/radix/ui/badge';
import { Sparkles } from '@/registry/hirael/bases/radix/components/sparkles';

const HEADLINE = 'Something new is on the way';

const Headline = () => {
  const reduce = useReducedMotion();
  const words = HEADLINE.split(' ');
  const half = Math.floor(words.length / 2);

  return (
    <h1
      data-slot="coming-soon-title"
      className="max-w-xl font-serif text-5xl font-medium leading-[1.04] tracking-tight sm:text-6xl md:text-7xl"
    >
      {words.map((word, i) => (
        <motion.span
          key={`${word}-${i}`}
          className={cn('inline-block', i < half ? 'text-muted-foreground' : 'text-foreground')}
          initial={reduce ? false : { opacity: 0, y: 16 }}
          whileInView={{ opacity: 1, y: 0 }}
          viewport={{ once: true }}
          transition={{ duration: 0.5, ease: 'easeOut', delay: 0.2 + i * 0.08 }}
        >
          {word}
          {i < words.length - 1 ? ' ' : null}
        </motion.span>
      ))}
    </h1>
  );
};

const ComingSoon02 = () => {
  const reduce = useReducedMotion();

  return (
    <section
      data-slot="coming-soon"
      className="relative isolate flex min-h-svh flex-col items-center justify-center overflow-hidden bg-background pt-24"
    >
      <motion.div
        data-slot="coming-soon-body"
        className="relative z-10 mx-auto flex w-full max-w-3xl flex-col items-center gap-5 px-6 text-center md:px-10"
        initial={reduce ? false : { opacity: 0, y: 24 }}
        whileInView={{ opacity: 1, y: 0 }}
        viewport={{ once: true }}
        transition={{ duration: 0.8, ease: 'easeOut' }}
      >
        <motion.div
          initial={reduce ? false : { opacity: 0, scale: 0.9 }}
          whileInView={{ opacity: 1, scale: 1 }}
          viewport={{ once: true }}
          transition={{ duration: 0.6, ease: 'easeOut', delay: 0.3 }}
        >
          <Badge
            variant="outline"
            data-slot="coming-soon-badge"
            className="rounded-full bg-card/70 px-4 py-1.5 font-mono text-[10px] uppercase tracking-[0.14em] text-muted-foreground backdrop-blur-sm"
          >
            Launching soon
          </Badge>
        </motion.div>

        <Headline />

        <motion.p
          data-slot="coming-soon-description"
          className="mt-2 max-w-2xl text-pretty text-base text-muted-foreground sm:text-lg"
          initial={reduce ? false : { opacity: 0 }}
          whileInView={{ opacity: 1 }}
          viewport={{ once: true }}
          transition={{ duration: 0.6, delay: 0.8 }}
        >
          Hirael Cloud brings managed Postgres and object storage to the same terminal-first console you use for the
          registry. We are finishing the last pieces now.
        </motion.p>

        <motion.div
          data-slot="coming-soon-actions"
          className="mt-4 flex flex-col items-center gap-4 sm:flex-row"
          initial={reduce ? false : { opacity: 0, y: 16 }}
          whileInView={{ opacity: 1, y: 0 }}
          viewport={{ once: true }}
          transition={{ duration: 0.6, delay: 1 }}
        >
          <Badge variant="outline" data-slot="coming-soon-cta" className="rounded-full px-5 py-2 text-sm font-medium">
            Coming soon
          </Badge>
        </motion.div>
      </motion.div>

      <div
        data-slot="coming-soon-horizon"
        aria-hidden
        className="relative -mt-32 h-96 w-full overflow-hidden [mask-image:radial-gradient(50%_50%,black,transparent)] after:absolute after:-start-1/2 after:top-1/2 after:aspect-[1/0.7] after:w-[200%] after:rounded-[100%] after:border-t after:border-border after:bg-card after:content-['']"
      >
        <div
          data-slot="coming-soon-horizon-glow"
          className="absolute inset-0 bg-[radial-gradient(circle_at_50%_100%,color-mix(in_oklch,var(--primary)_40%,transparent),transparent_70%)] opacity-40"
        />
        <Sparkles
          density={4}
          size={1.4}
          color="var(--primary)"
          className="[mask-image:radial-gradient(50%_50%,black,transparent_85%)]"
        />
      </div>
    </section>
  );
};

export default ComingSoon02;

demo.tsx
import ComingSoon02 from "@/components/ui/coming-soon-02";

export default function ComingSoon02Demo() {
  return <ComingSoon02 />;
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge sparkles.json
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
