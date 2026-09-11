<!-- Pros & Cons Block · @contentbit · https://21st.dev/@contentbit/components/pros-cons
     license: MIT · category: comparison
     A two-column content block that lists the advantages and disadvantages of a single option side by side. -->

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
components/ui/pros-cons.tsx
import type { BlockRenderContext } from '@contentbit/react'

import { prosConsBlock, splitProsCons } from '@contentbit/blocks'
import { defineBlockComponent } from '@contentbit/react'
import { Check, X } from 'lucide-react'

function Column({
  heading,
  items,
  tone,
  ctx,
}: {
  heading: string
  items: string[]
  tone: 'pro' | 'con'
  ctx: BlockRenderContext
}) {
  const Icon = tone === 'pro' ? Check : X
  const head =
    tone === 'pro'
      ? 'bg-emerald-500/[0.07] text-emerald-700 dark:text-emerald-400'
      : 'bg-rose-500/[0.07] text-rose-700 dark:text-rose-400'
  const mark =
    tone === 'pro' ? 'text-emerald-600 dark:text-emerald-500' : 'text-rose-600 dark:text-rose-500'
  return (
    <div className="bg-card overflow-hidden rounded-lg border">
      <div className={`flex items-center gap-2 border-b px-4 py-2.5 ${head}`}>
        <Icon className="size-3.5" aria-hidden strokeWidth={3} />
        <span className="text-xs font-semibold tracking-wider uppercase">{heading}</span>
      </div>
      <ul className="space-y-2 p-4 text-sm">
        {items.map((text, i) => (
          <li key={i} className="flex gap-2.5">
            <Icon className={`mt-1 size-3.5 shrink-0 ${mark}`} aria-hidden />
            {/* Item text is Markdown — render it, with paragraph margins zeroed. */}
            <div className="min-w-0 leading-relaxed [&>p]:m-0">{ctx.renderMarkdown(text)}</div>
          </li>
        ))}
      </ul>
    </div>
  )
}

export const ProsConsBlock = defineBlockComponent(prosConsBlock, ({ node, ctx }) => {
  const { pros, cons } = splitProsCons(node.data)
  return (
    <div data-cb-styled className="my-6 grid gap-3 sm:grid-cols-2">
      <Column heading="Pros" items={pros} tone="pro" ctx={ctx} />
      <Column heading="Cons" items={cons} tone="con" ctx={ctx} />
    </div>
  )
})

demo.tsx
"use client";

import ProsConsBlock from "@/components/ui/pros-cons";
import { genericBlocks } from "@contentbit/blocks";
import {
  assertValidDocument,
  compileDocument,
  createBlockRegistry,
} from "@contentbit/core";
import { ContentBlocks } from "@contentbit/react";

const markdown = `:::pros-cons
+ Cheap to run on serverless infrastructure
+ Near-instant setup with zero configuration
+ Scales automatically with traffic spikes
- No offline mode for local development
- Cold starts add noticeable latency
:::`;

const registry = createBlockRegistry().use(genericBlocks());
const document = assertValidDocument(
  compileDocument(markdown, registry),
  "pros-cons-demo.md",
);

export default function ProsConsDemo() {
  return (
    <div className="mx-auto w-full max-w-2xl px-4 py-8">
      <ContentBlocks
        document={document}
        components={{ "pros-cons": ProsConsBlock }}
        renderMarkdown={(md) => <p>{md}</p>}
      />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @contentbit/blocks @contentbit/core @contentbit/react lucide-react
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
