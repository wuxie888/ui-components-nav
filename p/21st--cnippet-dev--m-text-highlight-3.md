<!-- Text Highlight Direction Picker · @cnippet-dev · https://21st.dev/@cnippet-dev/components/m-text-highlight-3
     license: MIT · category: text
     Animated text highlight with buttons that sweep the highlight behind the text left, right, top-down, or bottom-up on demand. -->

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
components/ui/m-text-highlight-3.tsx
"use client";

import { useRef, useState } from "react";
import {
  TextHighlight,
  type TextHighlightRef,
} from "@/registry/default/motion/text-highlight";

const directions = ["ltr", "rtl", "ttb", "btt"] as const;

export default function TextHighlightDirections() {
  const ref = useRef<TextHighlightRef>(null);
  const [dir, setDir] = useState<(typeof directions)[number]>("ltr");

  return (
    <div className="flex min-h-50 flex-col items-center justify-center gap-6 px-6">
      <p className="font-bold text-2xl text-foreground">
        <TextHighlight
          direction={dir}
          highlightColor="hsl(155, 70%, 75%)"
          ref={ref}
          transition={{ bounce: 0, duration: 0.7, type: "spring" }}
          triggerType="ref"
        >
          Highlight direction
        </TextHighlight>
      </p>
      <div className="flex flex-wrap justify-center gap-2">
        {directions.map((d) => (
          <button
            className={`rounded-md px-3 py-1.5 font-medium text-xs transition-colors ${
              d === dir
                ? "bg-foreground text-background"
                : "bg-accent text-muted-foreground hover:text-foreground"
            }`}
            key={d}
            onClick={() => {
              setDir(d);
              ref.current?.reset();
              setTimeout(() => ref.current?.animate(d), 50);
            }}
            type="button"
          >
            {d.toUpperCase()}
          </button>
        ))}
      </div>
    </div>
  );
}

demo.tsx
import TextHighlightDirections from "@/components/ui/m-text-highlight-3";

export default function Default() {
  return <TextHighlightDirections />;
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add text-highlight
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
