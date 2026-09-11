<!-- Onboarding Stepper Progress · @shadcnspace · https://21st.dev/@shadcnspace/components/progress-02
     license: MIT · category: onboarding
     An onboarding progress tracker with a step counter, percentage completion bar, and back/next navigation buttons. -->

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
components/shadcn-space/progress/progress-02.tsx
"use client";

import { useState } from "react";
import { Progress } from "@/components/ui/progress";
import { Button } from "@/components/ui/button";

const TOTAL_STEPS = 4;

export default function OnboardingStepper() {
  const [current, setCurrent] = useState(1);
  const progressPct = ((current - 1) / (TOTAL_STEPS - 1)) * 100;

  return (
    <div className="w-full max-w-md rounded-xl border bg-background p-6 space-y-6">
      {/* progress */}
      <div className="space-y-2">
        <div className="flex items-center justify-between text-xs text-muted-foreground">
          <span>
            Step {current} of {TOTAL_STEPS}
          </span>
          <span>{Math.round(progressPct)}% complete</span>
        </div>
        <Progress
          value={progressPct}
          className="**:data-[slot='progress-track']:h-1.5!"
        />
      </div>

      {/* Navigation */}
      <div className="flex justify-between">
        <Button
          variant="outline"
          size="sm"
          className="cursor-pointer"
          onClick={() => setCurrent((c) => Math.max(1, c - 1))}
          disabled={current === 1}
        >
          Back
        </Button>
        <Button
          size="sm"
          className="cursor-pointer hover:bg-primary/80"
          onClick={() => setCurrent((c) => Math.min(TOTAL_STEPS, c + 1))}
          disabled={current === TOTAL_STEPS}
        >
          {current === TOTAL_STEPS - 1 ? "Finish" : "Next"}
        </Button>
      </div>
    </div>
  );
}

demo.tsx
import OnboardingStepper from "@/components/ui/progress-02";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <OnboardingStepper />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button progress
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
