<!-- AI Response · @educalvolpz · https://21st.dev/@educalvolpz/components/ai-response
     license: MIT · category: text
     Streaming answer body that reveals text as it arrives and anchors inline citation markers to their sources. Respects prefers-reduced-motion. -->

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
components/ui/index.tsx
"use client";

import { cn } from "@/lib/utils";
import { motion, useReducedMotion } from "motion/react";
import { Fragment, useEffect, useRef } from "react";

const EASE_OUT = [0.23, 1, 0.32, 1] as const;
/**
 * Hoisted and frozen. Motion restarts an animation whenever the transition it is
 * given changes, and this component re-renders on every token — so the object
 * has to be the same reference every time.
 */
const WORD_TRANSITION = { duration: 0.22, ease: EASE_OUT } as const;
const WORD_BLUR_PX = 4;
/** `[1]` style markers become citation pills. */
const CITATION_MARKER = /^\[(\d+)\]$/;
/**
 * Split on whitespace *and* on markers, so a marker is its own token even when
 * punctuation is glued to it — `compute [1],` has to yield `[1]` and `,`
 * separately or the pill never matches.
 */
const TOKEN_SPLIT = /(\s+|\[\d+\])/;
/** Anything without a letter or digit is punctuation and is not animated. */
const HAS_WORD_CHARACTER = /[\p{L}\p{N}]/u;
const WHITESPACE_ONLY = /^\s+$/;

export type AIResponseCitation = {
  id: string;
  /** The number shown in the pill, matching the `[n]` marker in the text. */
  index: number;
  title: string;
  /**
   * Where the source lives, when it lives anywhere.
   *
   * Optional on purpose: most retrieval is over internal documents that have no
   * public URL, and a required field there just pushes people into inventing
   * `example.com` links that go nowhere. Without a url the pill renders as plain
   * text instead of a dead link.
   */
  url?: string;
};

export type AIResponseProps = {
  /** Sources referenced by `[n]` markers in the text. */
  citations?: AIResponseCitation[];
  className?: string;
  /** Shows a caret after the last word. */
  isStreaming?: boolean;
  /** The response so far. Re-render it as it grows. */
  text: string;
};

type Token = {
  citation?: AIResponseCitation;
  value: string;
};

const tokenize = (text: string, citations: AIResponseCitation[]): Token[] =>
  text
    .split(TOKEN_SPLIT)
    .filter((value) => value !== "")
    .map((value) => {
      const match = value.match(CITATION_MARKER);
      if (!match) {
        return { value };
      }
      const index = Number(match[1]);
      const citation = citations.find((entry) => entry.index === index);
      return citation ? { citation, value } : { value };
    });

/**
 * Streaming assistant text.
 *
 * Words animate in as they *arrive*, not on a fixed timer — the component
 * remembers how many tokens it had last render and only animates the new ones.
 * A timer-driven typewriter drifts out of step with the real stream and starts
 * lying about how fast the model is answering.
 */
const AIResponse = ({
  citations = [],
  className,
  isStreaming = false,
  text,
}: AIResponseProps) => {
  const shouldReduceMotion = useReducedMotion();
  const tokens = tokenize(text, citations);

  // How many tokens had already been painted before this render. Everything at
  // or after this index is new and gets the entrance.
  const paintedRef = useRef(0);
  const firstNewIndex = paintedRef.current;

  useEffect(() => {
    paintedRef.current = tokens.length;
  }, [tokens.length]);

  return (
    <p
      className={cn(
        "text-pretty text-foreground text-sm leading-relaxed",
        className
      )}
    >
      {tokens.map((token, index) => {
        const isNew = index >= firstNewIndex;
        // Keyed by position only. Keying by text too would remount the last word
        // every time a token extends it, so it would re-animate on every frame of
        // the stream.
        const key = index;

        // Whitespace and bare punctuation stay as text. Wrapping a comma in its
        // own inline-block would let the line break between a word and its
        // punctuation.
        if (
          WHITESPACE_ONLY.test(token.value) ||
          !(token.citation || HAS_WORD_CHARACTER.test(token.value))
        ) {
          return <Fragment key={key}>{token.value}</Fragment>;
        }

        if (token.citation) {
          return (
            <AIResponseCitationPill
              citation={token.citation}
              isNew={isNew}
              key={key}
              shouldReduceMotion={Boolean(shouldReduceMotion)}
            />
          );
        }

        return (
          <motion.span
            animate={{ filter: "blur(0px)", opacity: 1, y: 0 }}
            className="inline-block"
            initial={
              isNew && !shouldReduceMotion
                ? { filter: `blur(${WORD_BLUR_PX}px)`, opacity: 0, y: 2 }
                : false
            }
            key={key}
            // No stagger delay, deliberately. The transition object has to stay
            // identical across renders: a delay derived from the render-time
            // index goes negative as the text grows, and the entrance then never
            // resolves — words stay blurred forever. Token arrival is the stagger.
            transition={shouldReduceMotion ? { duration: 0 } : WORD_TRANSITION}
          >
            {token.value}
          </motion.span>
        );
      })}
      {isStreaming ? (
        <AIResponseCaret shouldReduceMotion={shouldReduceMotion} />
      ) : null}
    </p>
  );
};

const AIResponseCaret = ({
  shouldReduceMotion,
}: {
  shouldReduceMotion: boolean | null;
}) => (
  // Inline rather than absolutely positioned, so it rides the last glyph for
  // free and never has to be told where the text ended.
  <motion.span
    animate={shouldReduceMotion ? { opacity: 1 } : { opacity: [1, 0.15, 1] }}
    aria-hidden="true"
    className="ml-0.5 inline-block h-[1em] w-[2px] translate-y-[0.15em] rounded-full bg-current align-baseline"
    transition={
      shouldReduceMotion
        ? { duration: 0 }
        : { duration: 1, ease: "linear", repeat: Number.POSITIVE_INFINITY }
    }
  />
);

const AIResponseCitationPill = ({
  citation,
  isNew,
  shouldReduceMotion,
}: {
  citation: AIResponseCitation;
  isNew: boolean;
  shouldReduceMotion: boolean;
}) => {
  const shared = {
    animate: { opacity: 1, scale: 1 },
    // No left margin: the marker is already preceded by a space in the text, so
    // adding one here doubles the gap.
    className:
      "mr-0.5 inline-flex h-4 min-w-4 items-center justify-center rounded-full border border-border bg-muted px-1 align-super font-medium text-[10px] text-muted-foreground no-underline",
    initial:
      isNew && !shouldReduceMotion
        ? { opacity: 0, scale: 0.6 }
        : (false as const),
    title: citation.title,
    transition: shouldReduceMotion
      ? { duration: 0 }
      : { bounce: 0.1, duration: 0.25, type: "spring" as const },
  };

  // An internal document has nowhere to go, so it is not dressed up as a link —
  // no hover affordance, no pointer, nothing to click and be disappointed by.
  if (!citation.url) {
    return <motion.span {...shared}>{citation.index}</motion.span>;
  }

  return (
    <motion.a
      {...shared}
      className={`${shared.className} transition-colors hover:border-foreground/30 hover:text-foreground`}
      href={citation.url}
      rel="noopener noreferrer"
      target="_blank"
    >
      {citation.index}
    </motion.a>
  );
};

export default AIResponse;

demo.tsx
"use client";

import Component from "@/components/ui/ai-response";

export default function DemoOne() {
  return (
    <div className="flex min-h-[440px] w-full items-center justify-center bg-background p-10">
      <div className="w-full max-w-lg rounded-2xl border bg-card p-6 shadow-sm">
        <Component
          citations={[
            { id: "1", index: 1, title: "Transitions — Motion", url: "https://motion.dev" },
            { id: "2", index: 2, title: "Animations guide — web.dev", url: "https://web.dev" },
          ]}
          text={`Use spring animations for interface motion rather than fixed-duration tweens [1]. A spring keeps its momentum when the target changes mid-flight, so an interrupted animation never snaps.

Animate only transform and opacity [2] — those are the two properties the browser can run on the compositor, off the main thread.`}
        />
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion motion
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
