<!-- Storage Usage Progress · @bundui · https://21st.dev/@bundui/components/progress7
     license: no-license · category: badge
     A progress bar showing storage usage with a color-coded status badge indicating usage level. -->

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

import { Progress } from "@/components/ui/progress";
import { Badge } from "@/components/ui/badge";

export default function ProgressComponent() {
  const getStatusColor = (value: number) => {
    if (value >= 80) return "bg-red-500";
    if (value >= 50) return "bg-yellow-500";
    return "bg-green-500";
  };

  const getStatusText = (value: number) => {
    if (value >= 80) return "High";
    if (value >= 50) return "Medium";
    return "Low";
  };

  const usage = 75;

  return (
    <div className="w-full max-w-sm space-y-2">
      <div className="flex items-center justify-between">
        <span className="text-sm font-medium">Storage Usage</span>
        <Badge variant={usage >= 80 ? "destructive" : usage >= 50 ? "secondary" : "default"}>
          {getStatusText(usage)}
        </Badge>
      </div>
      <Progress value={usage} className={getStatusColor(usage)} />
      <div className="flex items-center justify-between text-xs text-muted-foreground">
        <span>7.5 GB of 10 GB used</span>
        <span>{usage}%</span>
      </div>
    </div>
  );
}

demo.tsx
import ProgressComponent from "@/components/ui/progress7";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-6">
      <ProgressComponent />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge progress
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
