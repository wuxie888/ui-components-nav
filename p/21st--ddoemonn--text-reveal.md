<!-- Text Reveal · @ddoemonn · https://21st.dev/@ddoemonn/components/text-reveal
     license: MIT · category: text
     Animated text that reveals word by word or character by character with a staggered blur-and-slide effect, optionally triggered when it scrolls into view. -->

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
components/ui/text-reveal.tsx
"use client";

import { Fragment, useMemo, useRef } from "react";
import { motion, useInView, useReducedMotion } from "motion/react";

const EASE = [0.23, 1, 0.32, 1] as const;
const DURATION = 0.6;

const HIDDEN = { opacity: 0, y: 10, filter: "blur(8px)" } as const;
const SHOWN = { opacity: 1, y: 0, filter: "blur(0px)" } as const;

export type TextRevealSplit = "word" | "character";

export type TextRevealUnit = {
  key: string;
  text: string;
  index: number;
};

export type TextRevealGroup = {
  key: string;
  units: TextRevealUnit[];
};

export type UseTextRevealOptions = {
  text: string;
  by?: TextRevealSplit;
  stagger?: number;
  maxDuration?: number;
  startOnView?: boolean;
  play?: boolean;
  once?: boolean;
  amount?: number;
};

export function useTextReveal<T extends HTMLElement = HTMLSpanElement>({
  text,
  by = "word",
  stagger = 0.055,
  maxDuration = 1.6,
  startOnView = true,
  play = true,
  once = true,
  amount = 0.35,
}: UseTextRevealOptions) {
  const ref = useRef<T>(null);
  const inView = useInView(ref, { once, amount });
  const reduced = useReducedMotion();

  const { groups, step, count } = useMemo(() => {
    const words = text.trim().length ? text.trim().split(/\s+/) : [];

    let index = 0;
    const built: TextRevealGroup[] = words.map((word, w) => {
      if (by === "character") {
        return {
          key: `w${w}`,
          units: Array.from(word).map((char, c) => ({
            key: `w${w}c${c}`,
            text: char,
            index: index++,
          })),
        };
      }
      return {
        key: `w${w}`,
        units: [{ key: `w${w}`, text: word, index: index++ }],
      };
    });

    const total = index;
    const span = Math.max(0, maxDuration - DURATION);

    return {
      groups: built,
      count: total,
      step: total > 1 ? Math.min(stagger, span / (total - 1)) : 0,
    };
  }, [text, by, stagger, maxDuration]);

  const started = play && (!startOnView || inView);

  return {
    ref,
    groups,
    step,
    count,
    started,
    reduced: Boolean(reduced),
    duration: count > 1 ? (count - 1) * step + DURATION : DURATION,
  };
}

export type TextRevealProps = UseTextRevealOptions & {
  className?: string;
};

export function TextReveal({
  text,
  by = "word",
  stagger = 0.055,
  maxDuration = 1.6,
  startOnView = true,
  play = true,
  once = true,
  amount = 0.35,
  className = "",
}: TextRevealProps) {
  const { ref, groups, step, started, reduced } = useTextReveal<HTMLSpanElement>(
    { text, by, stagger, maxDuration, startOnView, play, once, amount },
  );

  return (
    <span ref={ref} className={`text-stone-700 dark:text-stone-200 ${className}`}>
      <span className="sr-only">{text}</span>

      <span aria-hidden="true">
        {groups.map((group, g) => (
          <Fragment key={group.key}>
            {g > 0 ? " " : null}
            <span className="inline-block whitespace-nowrap align-baseline">
              {group.units.map((unit) => (
                <motion.span
                  key={unit.key}
                  className="inline-block align-baseline"
                  initial={reduced ? false : HIDDEN}
                  animate={started ? SHOWN : HIDDEN}
                  transition={
                    reduced
                      ? { duration: 0 }
                      : {
                          duration: DURATION,
                          ease: EASE,
                          delay: started ? unit.index * step : 0,
                        }
                  }
                >
                  {unit.text}
                </motion.span>
              ))}
            </span>
          </Fragment>
        ))}
      </span>
    </span>
  );
}

demo.tsx
"use client";

import * as React from "react";
import { TextReveal } from "@/components/ui/text-reveal";

const COPY =
  "Nobody is given a week to write these, so they ship at eighty percent and stay there for the life of the product.";

export function TextRevealDemo() {
  const [take, setTake] = React.useState(0);

  return (
    <div className="mx-auto w-full max-w-[420px]">
      <TextReveal
        key={take}
        text={COPY}
        startOnView={false}
        className="block text-[13.5px] leading-relaxed text-ink-2"
      />

      <button
        type="button"
        onClick={() => setTake((t) => t + 1)}
        className="mat-cap press mt-4 h-8 rounded-[6px] px-3 text-[12.5px] font-medium text-ink"
      >
        Reveal
      </button>
    </div>
  );
}

export default TextRevealDemo;
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
