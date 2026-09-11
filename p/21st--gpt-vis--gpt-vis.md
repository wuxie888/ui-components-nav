<!-- GPT-Vis · @gpt-vis · https://21st.dev/@gpt-vis/components/gpt-vis
     license: MIT · category: ai-chat
     A React chart component that renders AI/LLM-generated data visualizations from vis-syntax strings or config objects, supporting 26 chart types via @antv/gpt-vis. -->

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
components/ui/gpt-vis.tsx
'use client';

import { GPTVis as GPTVisCore, type VisualizationOptions } from '@antv/gpt-vis';
import { clsx, type ClassValue } from 'clsx';
import { useEffect, useRef, useState } from 'react';
import { twMerge } from 'tailwind-merge';

function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}

export interface GPTVisProps extends Omit<VisualizationOptions, 'container'> {
  content: string | Record<string, unknown>;
  className?: string;
  containerStyle?: React.CSSProperties;
}

export function GPTVis({
  content,
  width,
  height,
  theme,
  wrapper,
  locale,
  className,
  containerStyle,
}: GPTVisProps) {
  const containerRef = useRef<HTMLDivElement>(null);
  const [instance, setInstance] = useState<GPTVisCore | null>(null);

  useEffect(() => {
    if (!containerRef.current) return;
    const inst = new GPTVisCore({
      container: containerRef.current,
      width,
      height,
      theme,
      wrapper,
      locale,
    });
    setInstance(inst);
    return () => {
      inst.destroy();
      setInstance(null);
    };
  }, [width, height, theme, wrapper, locale]);

  const contentDeps = typeof content === 'string' ? content : JSON.stringify(content);

  useEffect(() => {
    instance?.render(content);
  }, [instance, contentDeps]);

  return (
    <div
      ref={containerRef}
      className={cn('w-full min-h-[300px]', className)}
      style={containerStyle}
    />
  );
}

demo.tsx
import { GPTVis } from "@/components/ui/gpt-vis";

export default function Default() {
  const visSyntax = `vis line
data
  - time 2020
    value 100
  - time 2021
    value 120
  - time 2022
    value 150
  - time 2023
    value 180
title Sales Trend`;

  return (
    <div className="inline-block bg-background p-4 text-foreground">
      <GPTVis
        content={visSyntax}
        theme="academy"
        width={600}
        height={400}
        className="w-auto min-h-0"
      />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @antv/gpt-vis clsx tailwind-merge
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
