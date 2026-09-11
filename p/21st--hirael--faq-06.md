<!-- FAQ Accordion Reveal · @hirael · https://21st.dev/@hirael/components/faq-06
     license: MIT · category: faq
     A centered FAQ section with a pill badge, a word-by-word animated headline, and an accordion of bordered question cards that stagger into view and tint when opened. -->

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
components/ui/faq-06.tsx
// FAQ 6 from Hirael <https://hirael.com/blocks/faqs/faq-06>
// MIT · Mohammad Shehadeh · https://github.com/MohammadShehadeh/hirael

'use client';

import * as React from 'react';
import { type HTMLMotionProps, motion, useReducedMotion } from 'motion/react';

import { cn } from '@/lib/utils';
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from '@/registry/hirael/bases/radix/ui/accordion';
import { Badge } from '@/registry/hirael/bases/radix/ui/badge';

const HEADLINE = 'Answers before you ask';

const FAQS: readonly { id: string; q: string; a: string }[] = [
  {
    id: 'item-1',
    q: 'What do I actually install?',
    a: 'Plain TSX files. The CLI copies each component into your repo, so there is no package to update and nothing hidden behind a version pin.',
  },
  {
    id: 'item-2',
    q: 'Does it work with my existing shadcn/ui setup?',
    a: 'Yes. Every item reads the same CSS variables and uses the same primitives, so it lands next to what you already have and picks up your theme.',
  },
  {
    id: 'item-3',
    q: 'Can I change the code after installing?',
    a: 'That is the point. The source is yours from the first install. Edit it, rename it, delete the parts you do not need.',
  },
  {
    id: 'item-4',
    q: 'Is right-to-left supported?',
    a: 'Every component and block uses logical properties and flips directional icons, so an Arabic or Hebrew layout works without extra configuration.',
  },
  {
    id: 'item-5',
    q: 'How do I report a problem?',
    a: 'Open an issue on GitHub with the component name and a short reproduction. Most reports get a reply within a day.',
  },
];

const EASE = 'easeOut' as const;

const FaqBadge = ({ className, ...props }: React.ComponentProps<typeof Badge>) => {
  const reduce = useReducedMotion();
  return (
    <motion.div
      initial={reduce ? false : { opacity: 0, scale: 0.9 }}
      whileInView={{ opacity: 1, scale: 1 }}
      viewport={{ once: true }}
      transition={{ duration: 0.5, ease: EASE, delay: 0.2 }}
    >
      <Badge
        data-slot="faq-badge"
        variant="outline"
        className={cn(
          'rounded-full bg-card/70 px-4 py-1.5 font-mono text-[10px] uppercase tracking-[0.14em] text-muted-foreground backdrop-blur-sm',
          className,
        )}
        {...props}
      />
    </motion.div>
  );
};

interface FaqTitleProps extends Omit<React.ComponentProps<'h2'>, 'children'> {
  children: string;
}

const FaqTitle = ({ children, className, ...props }: FaqTitleProps) => {
  const reduce = useReducedMotion();
  const words = children.split(' ');
  const half = Math.floor(words.length / 2);

  return (
    <h2
      data-slot="faq-title"
      className={cn(
        'mx-auto max-w-3xl text-balance font-serif text-4xl font-medium leading-[1.04] tracking-tight sm:text-5xl',
        className,
      )}
      {...props}
    >
      {words.map((word, i) => (
        <motion.span
          key={`${word}-${i}`}
          className={cn('me-[0.25em] inline-block', i < half ? 'text-muted-foreground' : 'text-foreground')}
          initial={reduce ? false : { opacity: 0, y: 16 }}
          whileInView={{ opacity: 1, y: 0 }}
          viewport={{ once: true }}
          transition={{ duration: 0.5, ease: EASE, delay: 0.2 + i * 0.08 }}
        >
          {word}
        </motion.span>
      ))}
    </h2>
  );
};

const FaqDescription = ({ className, ...props }: HTMLMotionProps<'p'>) => {
  const reduce = useReducedMotion();
  return (
    <motion.p
      data-slot="faq-description"
      initial={reduce ? false : { opacity: 0, y: 20 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true }}
      transition={{ duration: 0.5, ease: EASE, delay: 0.4 }}
      className={cn('mx-auto max-w-2xl text-pretty text-base text-muted-foreground sm:text-lg', className)}
      {...props}
    />
  );
};

interface FaqCardProps extends React.ComponentProps<typeof AccordionItem> {
  /** Position in the list, used to stagger the reveal. */
  index?: number;
}

const FaqCard = ({ index = 0, className, ...props }: FaqCardProps) => {
  const reduce = useReducedMotion();
  return (
    <motion.div
      initial={reduce ? false : { opacity: 0, y: 20 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true }}
      transition={{ duration: 0.45, ease: EASE, delay: index * 0.1 }}
    >
      <AccordionItem
        data-slot="faq-card"
        className={cn(
          'rounded-lg border border-border bg-card px-4 transition-colors last:border-b data-[state=open]:bg-muted/40 md:px-6',
          className,
        )}
        {...props}
      />
    </motion.div>
  );
};

const Faq06 = () => {
  return (
    <section data-slot="faq" className="bg-background py-16 md:py-24">
      <div className="mx-auto w-full max-w-3xl px-6">
        <div className="mb-10 flex flex-col items-center gap-5 text-center">
          <FaqBadge>FAQ</FaqBadge>
          <FaqTitle>{HEADLINE}</FaqTitle>
          <FaqDescription>
            The questions that come up most when a team installs its first component. Still unsure? Open an issue and
            ask.
          </FaqDescription>
        </div>

        <Accordion type="multiple" data-slot="faq-list" className="flex flex-col gap-3">
          {FAQS.map((item, i) => (
            <FaqCard key={item.id} value={item.id} index={i}>
              <AccordionTrigger className="gap-6 py-4 text-start text-base font-medium text-foreground hover:no-underline md:text-lg">
                {item.q}
              </AccordionTrigger>
              <AccordionContent className="pb-5 text-base text-muted-foreground">{item.a}</AccordionContent>
            </FaqCard>
          ))}
        </Accordion>
      </div>
    </section>
  );
};

export { FaqBadge, FaqTitle, FaqDescription, FaqCard };

export default Faq06;

demo.tsx
import Faq06 from "@/components/ui/faq-06";

export default function Faq06Demo() {
  return <Faq06 />;
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add accordion accordion.json badge
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
