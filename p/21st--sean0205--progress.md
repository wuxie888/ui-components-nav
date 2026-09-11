<!-- Progress · @sean0205 · https://21st.dev/@sean0205/components/progress
     license: MIT · category: progress
     Displays an indicator showing the completion progress of a task, typically displayed as a progress bar. -->

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
components/ui/c-progress-1.tsx
import {
  Progress,
  ProgressLabel,
  ProgressValue,
} from "@/components/ui/progress"

export function Pattern() {
  return (
    <div className="mx-auto w-full max-w-xs">
      <Progress value={56}>
        <ProgressLabel>Upload progress</ProgressLabel>
        <ProgressValue />
      </Progress>
    </div>
  )
}

demo.tsx
'use client';

import { useEffect, useState } from 'react';
import { ProgressCircle } from '@/components/ui/progress';

export default function Component() {
  const [cpuUsage, setCpuUsage] = useState(0);

  useEffect(() => {
    // CPU usage simulation
    const cpuTimer = setInterval(() => {
      setCpuUsage((prev) => {
        const target = 30 + Math.sin(Date.now() / 3000) * 20 + Math.random() * 15;
        return prev + (target - prev) * 0.1;
      });
    }, 100);

    return () => {
      clearInterval(cpuTimer);
    };
  }, []);

  return (
    <div className="flex items-center justify-center">
      <div className="flex flex-col items-center gap-3">
        <ProgressCircle
          value={cpuUsage}
          size={80}
          strokeWidth={6}
          className="text-fuchsia-500"
          indicatorClassName="text-fuchsia-500"
        >
          <div className="text-center">
            <div className="text-base font-bold">{Math.round(cpuUsage)}%</div>
            <div className="text-xs text-muted-foreground">CPU</div>
          </div>
        </ProgressCircle>
        <span className="text-xs text-muted-foreground">Processor Usage</span>
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install radix-ui
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add progress
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
