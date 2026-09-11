<!-- Inline Citations · @kvnkld · https://21st.dev/@kvnkld/components/inline-citations
     license: no-license · category: tooltip
     Prose text with inline numbered citation chips that link to sources, reveal the reference name on hover, and list all references in a footer. -->

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
components/ui/InlineCitations.tsx
import styles from "./InlineCitations.module.css";

type CiteRef = { n: number; label: string; host: string; url: string };

const TEXT =
  "Transformers scale well with data and compute[1], though attention is quadratic in sequence length[2].";
const REFS: CiteRef[] = [
  { n: 1, label: "Attention Is All You Need", host: "arxiv.org", url: "https://arxiv.org/abs/1706.03762" },
  { n: 2, label: "Efficient Transformers: A Survey", host: "arxiv.org", url: "https://arxiv.org/abs/2009.06732" },
];

function CiteArrow() {
  return (
    <svg width="10" height="10" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="1.8" strokeLinecap="round" strokeLinejoin="round" aria-hidden>
      <path d="M4.5 10.5 12 3m0 0 7.5 7.5M12 3v18" />
    </svg>
  );
}

export function InlineCitations({
  text = TEXT,
  refs = REFS,
}: {
  text?: string;
  refs?: CiteRef[];
}) {
  // Markers like [1] in the text become small numbered chips that link to
  // the source and reveal the reference name in a tooltip on hover.
  const parts = text.split(/(\[\d+\])/g);
  return (
    <div className={styles.citeProse}>
      <p>
        {parts.map((part, i) => {
          const m = part.match(/^\[(\d+)\]$/);
          if (!m) return <span key={i}>{part}</span>;
          const r = refs.find((x) => x.n === Number(m[1]));
          return r ? (
            <span key={i} className={styles.citeTip}>
              <a className={styles.citeMark} href={r.url} target="_blank" rel="noreferrer">{r.n}</a>
              <span className={styles.citeTipBox} role="tooltip">{r.label}</span>
            </span>
          ) : (
            <span key={i} className={styles.citeMark}>{m[1]}</span>
          );
        })}
      </p>
      <div className={styles.citeFooter}>
        {refs.map((r) => (
          <a key={r.n} className={styles.citeRef} href={r.url} target="_blank" rel="noreferrer">
            <span className={styles.citeMark}>{r.n}</span>
            <span className={styles.citeRefLabel}>{r.label}</span>
            <span className={styles.citeSep}>·</span>
            <span className={styles.citeRefHost}>{r.host}</span>
            <span className={styles.citeArrow} aria-hidden><CiteArrow /></span>
          </a>
        ))}
      </div>
    </div>
  );
}

components/ui/InlineCitations.module.css
.citeProse { max-width: 100%; font-size: 14px; line-height: 19px; color: #1a1a1a; overflow-wrap: anywhere; }
.citeProse p { margin: 0; }
.citeMark { display: inline-flex; align-items: center; justify-content: center; width: 12px; height: 12px; flex: none; border-radius: 4px; background: #f4f5f7; color: #a1a1a1; font-size: 9px; font-weight: 600; line-height: 1; vertical-align: 5.5px; margin: 0 2px; }
a.citeMark { cursor: pointer; text-decoration: none; transition: color 0.15s, background 0.15s; }
a.citeMark:hover { color: #1a1a1a; background: #e6e8ec; }
.citeTip { position: relative; display: inline; }
.citeTipBox { position: absolute; left: 50%; bottom: calc(100% + 6px); transform: translateX(-50%) translateY(1px); font-size: 10px; line-height: 1; font-weight: 500; color: rgba(255, 255, 255, 0.9); background: rgba(29, 29, 29, 0.6); -webkit-backdrop-filter: blur(6px); backdrop-filter: blur(6px); padding: 4px 5px; border-radius: 6px; white-space: nowrap; pointer-events: none; opacity: 0; filter: blur(2px); transition: opacity 0.15s ease, transform 0.15s ease, filter 0.15s ease; z-index: 1000; }
.citeTip:hover .citeTipBox, .citeTip:focus-within .citeTipBox { opacity: 1; filter: blur(0); transform: translateX(-50%) translateY(0); }
.citeFooter { display: flex; flex-direction: column; gap: 6px; margin-top: 12px; padding-top: 10px; border-top: 1px solid #e6e8ec; }
.citeRef { display: flex; align-items: center; gap: 6px; font-size: 12px; line-height: 18px; color: #a1a1a1; min-width: 0; text-decoration: none; cursor: pointer; }
.citeRef .citeMark { margin: 0; }
.citeRefLabel { color: #1a1a1a; font-weight: 450; flex: 0 1 auto; min-width: 0; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
.citeSep { color: #a1a1a1; flex: none; }
.citeRefHost { color: #a1a1a1; flex: none; white-space: nowrap; transition: color 0.16s; }
.citeArrow { display: inline-flex; flex: none; margin-left: -2px; color: #a1a1a1; opacity: 0; transform: rotate(45deg) translate(0, 2px); transition: opacity 0.16s, transform 0.22s; pointer-events: none; }
.citeRef:hover .citeArrow { opacity: 1; transform: rotate(45deg) translate(0, 0); }
.citeRef:hover .citeRefHost { color: #1a1a1a; }
@media (prefers-color-scheme: dark) {
  .citeProse { color: #f5f5f5; }
  .citeMark { background: #424242; }
  a.citeMark:hover { color: #f5f5f5; background: #525252; }
  .citeFooter { border-top-color: #303030; }
  .citeSep { color: #737373; }
  .citeArrow { color: #737373; }
  .citeRefLabel { color: #f5f5f5; }
  .citeRef:hover .citeRefHost { color: #f5f5f5; }
  .citeTipBox { background: rgba(255, 255, 255, 0.12); }
}
:global([data-theme="dark"]) .citeTipBox,
:global(.dark) .citeTipBox { background: rgba(255, 255, 255, 0.12); }

demo.tsx
import { InlineCitations } from "@/components/ui/inline-citations";

export default function Default() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center p-8">
      <div className="w-full max-w-md">
        <InlineCitations />
      </div>
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
