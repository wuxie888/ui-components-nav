<!-- Thinking Reasoning · @kvnkld · https://21st.dev/@kvnkld/components/thinking-reasoning
     license: MIT · category: ai-chat
     An animated, collapsible AI thinking block whose shimmering label expands to stream the agent's reasoning, then folds into a "Thought for Ns" summary. -->

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
components/ui/ThinkingReasoning.tsx
"use client";

import styles from "./ThinkingReasoning.module.css";
import { useEffect, useRef, useState } from "react";

const SENTENCES = [
  "Reading the request and the current selection, then locating the jwt.verify call inside the auth middleware.",
  "The verify call sets no algorithms allowlist, so a token signed with 'none' or a weak cipher could be accepted.",
  "Tracing where the signing secret is loaded from and confirming it is never logged or sent back to the client.",
  "Planning to pin the algorithm to HS256 and to validate the issuer and audience claims on every incoming request.",
  "Scanning the existing tests around the middleware so the fix stays covered and nothing downstream regresses.",
  "Drafting the patch with a focused regression test that rejects tampered, expired, and unsigned tokens.",
];

// Per-sentence reveal cadence (ms). Sums to ~5s of "thinking".
const DELAYS = [700, 900, 800, 850, 800, 900];
const THINK_MS = DELAYS.reduce((a, b) => a + b, 0);
const ELAPSED_S = Math.max(1, Math.round(THINK_MS / 1000));
const COLLAPSE_BEAT = 360;

// Geometry - keep in sync with the CSS below.
const SENT_H = 40; // 2 lines × 20px
const GAP = 4;
const MAX_H = 180; // viewport grows with content up to this, then scrolls
const FADE = 16; // top/bottom fade once the viewport is capped

export function ThinkingReasoning() {
  // "thinking" | "done"
  const [phase, setPhase] = useState("thinking");
  const [revealed, setRevealed] = useState(0);
  // While thinking the reasoning is always open; once done it folds into
  // the summary and the user can toggle it back open.
  const [open, setOpen] = useState(false);
  // Which soft fades to show while scrolling the unfolded reasoning.
  const [fade, setFade] = useState({ top: false, bottom: true });
  const viewportRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    if (window.matchMedia?.("(prefers-reduced-motion: reduce)").matches) {
      setRevealed(SENTENCES.length);
      setPhase("done");
      return;
    }
    const timers: ReturnType<typeof setTimeout>[] = [];
    const at = (ms: number, fn: () => void) => timers.push(setTimeout(fn, ms));
    let t = 0;
    DELAYS.forEach((d, i) => {
      t += d;
      at(t, () => setRevealed(i + 1));
    });
    at(THINK_MS + COLLAPSE_BEAT, () => setPhase("done"));
    return () => timers.forEach(clearTimeout);
  }, []);

  const done = phase === "done";
  const expanded = done ? open : true;
  const count = done ? SENTENCES.length : revealed;
  const contentH = count > 0 ? count * SENT_H + (count - 1) * GAP : 0;
  const capped = contentH > MAX_H;
  const viewH = capped ? MAX_H : contentH;
  const scrollable = done && open;
  const translate = scrollable ? 0 : capped ? MAX_H - FADE - contentH : 0;

  const showTop = scrollable ? fade.top : capped;
  const showBottom = scrollable ? fade.bottom : capped;
  const mask = capped
    ? `linear-gradient(to bottom, transparent 0, #000 ${showTop ? FADE : 0}px, #000 calc(100% - ${showBottom ? FADE : 0}px), transparent 100%)`
    : "none";

  const onScroll = () => {
    const el = viewportRef.current;
    if (!el) return;
    setFade({
      top: el.scrollTop > 1,
      bottom: el.scrollTop + el.clientHeight < el.scrollHeight - 1,
    });
  };

  const toggle = () => {
    const next = !open;
    if (next) {
      setFade({ top: false, bottom: true });
      if (viewportRef.current) viewportRef.current.scrollTop = 0;
    }
    setOpen(next);
  };

  return (
    <div className={styles.tr}>
      <button
        type="button"
        className={styles.trHeader + (done ? " " + styles.isClickable : "")}
        aria-expanded={expanded}
        aria-label="Toggle thought"
        onClick={done ? toggle : undefined}
      >
        {done ? (
          <span className={styles.trLabel}>
            <span className={styles.trVerb}>Thought</span> for {ELAPSED_S}s
          </span>
        ) : (
          <span className={styles.trLabel + " " + styles.trShimmer}>Thinking…</span>
        )}
        {done && (
          <svg className={styles.trChevron} viewBox="0 0 24 24" width="12" height="12" aria-hidden="true">
            <path d="m4.5 15.75 7.5-7.5 7.5 7.5" fill="none" stroke="currentColor" strokeWidth="1.8" strokeLinecap="round" strokeLinejoin="round" />
          </svg>
        )}
      </button>

      <div className={styles.trCollapsible + (expanded ? "" : " " + styles.isCollapsed)}>
        <div className={styles.trInner}>
          <div
            ref={viewportRef}
            className={styles.trViewport + (scrollable ? " " + styles.isScroll : "")}
            style={{ height: `${viewH}px`, WebkitMaskImage: mask, maskImage: mask }}
            onScroll={scrollable ? onScroll : undefined}
          >
            <div className={styles.trStream} style={{ transform: `translateY(${translate}px)` }}>
              {SENTENCES.slice(0, count).map((line, i) => (
                <p key={i} className={styles.trSentence}>{line}</p>
              ))}
            </div>
          </div>
        </div>
      </div>
    </div>
  );
}

components/ui/ThinkingReasoning.module.css
.tr {
  display: flex;
  flex-direction: column;
  width: 360px;
  max-width: 100%;
  /* anchor the header: header (20) + viewport margin (6) + max viewport (180) */
  min-height: 206px;
  font-family: "Inter", system-ui, sans-serif;
  /* soft fade-in on (re)mount */
  animation: tr-block-in 320ms cubic-bezier(0.22, 1, 0.36, 1) both;
}
@keyframes tr-block-in {
  from { opacity: 0; }
  to { opacity: 1; }
}
.trHeader {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  align-self: flex-start;
  min-height: 20px;
  padding: 0;
  border: 0;
  background: transparent;
  cursor: default;
}
.trHeader.isClickable { cursor: pointer; }
.trLabel {
  font-size: 13px;
  line-height: 18px;
  font-weight: 500;
  /* softer than "Thought" so "for Ns" matches the dark-mode hierarchy */
  color: var(--tre-label, color-mix(in srgb, #a1a1a1 68%, transparent));
  letter-spacing: -0.005em;
}
.trVerb { color: var(--tre-verb, #a1a1a1); }
.trChevron {
  color: var(--tre-chevron, #a1a1a1);
  transition: transform 280ms cubic-bezier(0.22, 1, 0.36, 1);
  /* base path is an up caret; collapsed summary points down */
  transform: rotate(180deg);
}
.trHeader[aria-expanded="true"] .trChevron { transform: rotate(0deg); }
.trHeader.isClickable:hover .trChevron { color: var(--tre-hover, #a1a1a1); }
.trCollapsible {
  display: grid;
  grid-template-rows: 1fr;
  opacity: 1;
  transition: grid-template-rows 320ms cubic-bezier(0.22, 1, 0.36, 1),
    opacity 220ms ease;
}
.trCollapsible.isCollapsed {
  grid-template-rows: 0fr;
  opacity: 0;
  pointer-events: none;
}
.trInner { min-height: 0; overflow: hidden; }
/* viewport: grows with the content, then caps at MAX_H. While thinking
   the stream auto-scrolls behind a soft fade; once unfolded by the user
   it becomes natively scrollable and the fades follow the scroll position. */
.trViewport {
  margin-top: 6px;
  overflow: hidden;
  transition: height 360ms cubic-bezier(0.22, 1, 0.36, 1);
}
.trViewport.isScroll {
  overflow-y: auto;
  scrollbar-width: none;
}
.trViewport.isScroll::-webkit-scrollbar { display: none; }
.trStream {
  display: flex;
  flex-direction: column;
  gap: 4px;
  transition: transform 560ms cubic-bezier(0.22, 1, 0.36, 1);
  will-change: transform;
}
.trSentence {
  margin: 0;
  height: 40px;
  line-height: 20px;
  font-size: 13px;
  font-weight: 425;
  color: var(--tre-sentence, #a1a1a1);
  letter-spacing: -0.005em;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  animation: tr-sentence-in 420ms cubic-bezier(0.22, 1, 0.36, 1) both;
}
@keyframes tr-sentence-in {
  from { opacity: 0; }
  to { opacity: 1; }
}
/* label-shine: a soft brightness valley sweeps through the text */
.trShimmer {
  color: transparent;
  -webkit-text-fill-color: transparent;
  background: linear-gradient(
    90deg,
    #a1a1a1 0%, #a1a1a1 30%,
    rgba(161, 161, 161, 0.45) 45%, rgba(161, 161, 161, 0.45) 55%,
    #a1a1a1 70%, #a1a1a1 100%
  );
  background-size: 300% 100%;
  -webkit-background-clip: text;
  background-clip: text;
  animation: tr-shine 2.25s cubic-bezier(0.25, 0.1, 0.25, 1) infinite;
}
@keyframes tr-shine {
  0%, 18% { background-position: 100% 0; }
  82%, 100% { background-position: 0% 0; }
}
:global(:root),
:global([data-theme="light"]) {
  --tre-label: color-mix(in srgb, #a1a1a1 68%, transparent);
  --tre-verb: #a1a1a1;
  --tre-chevron: #a1a1a1;
  --tre-sentence: #a1a1a1;
  --tre-hover: #a1a1a1;
}
:global([data-theme="dark"]),
:global(.dark) {
  --tre-label: #737373;
  --tre-verb: #a3a3a3;
  --tre-chevron: #737373;
  --tre-sentence: #737373;
  --tre-hover: #a3a3a3;
}
@media (prefers-color-scheme: dark) {
  :global(:root:not([data-theme])) {
    --tre-label: #737373;
    --tre-verb: #a3a3a3;
    --tre-chevron: #737373;
    --tre-sentence: #737373;
    --tre-hover: #a3a3a3;
  }
}

demo.tsx
import ThinkingReasoning from "@/components/ui/thinking-reasoning";

export default function Default() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center p-8">
      <ThinkingReasoning />
    </div>
  );
}
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
