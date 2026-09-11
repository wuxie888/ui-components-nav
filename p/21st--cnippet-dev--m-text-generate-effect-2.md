<!-- AI Response Text Generate Card · @cnippet-dev · https://21st.dev/@cnippet-dev/components/m-text-generate-effect-2
     license: no-license · category: ai-chat
     An AI assistant chat card that animates its response with a word-by-word text-generate effect and regenerates on demand. -->

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
components/ui/m-text-generate-effect-2.tsx
//biome-ignore-all lint/style/noNonNullAssertion:<>

"use client";

import { useState } from "react";
import { TextGenerateEffect } from "@/registry/default/motion/text-generate-effect";

const responses = [
  "Here are 164 animated components built on Motion and Tailwind CSS, ready to copy and paste into your project.",
  "Each component ships with a source file, three real-world examples, and a CLI install command.",
];

export default function TextGenerateEffectAI() {
  const [index, setIndex] = useState(0);
  const [trigger, setTrigger] = useState(true);

  const regenerate = () => {
    setTrigger(false);
    setTimeout(() => {
      setIndex((i) => (i + 1) % responses.length);
      setTrigger(true);
    }, 50);
  };

  return (
    <div className="flex min-h-50 items-center justify-center px-6">
      <div className="w-full max-w-lg rounded-2xl border border-border bg-card p-6 shadow-sm">
        <div className="mb-3 flex items-center gap-2">
          <div className="h-6 w-6 rounded-full bg-linear-to-br from-violet-500 to-pink-500" />
          <span className="font-semibold text-muted-foreground text-xs">
            AI Assistant
          </span>
        </div>
        <TextGenerateEffect
          as="p"
          className="text-foreground text-sm leading-relaxed"
          filter
          key={index}
          staggerDuration={0.06}
          transition={{ duration: 0.4 }}
          trigger={trigger}
        >
          {responses[index]!}
        </TextGenerateEffect>
        <button
          className="mt-4 text-muted-foreground text-xs hover:text-foreground"
          onClick={regenerate}
          type="button"
        >
          ↺ Regenerate
        </button>
      </div>
    </div>
  );
}

demo.tsx
import TextGenerateEffectAI from "@/components/ui/m-text-generate-effect-2";

export default function Default() {
  return <TextGenerateEffectAI />;
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add text-generate-effect text-generate-effect?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068
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
