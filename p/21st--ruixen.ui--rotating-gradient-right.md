<!-- Rotating Gradient Right · @ruixen.ui · https://21st.dev/@ruixen.ui/components/rotating-gradient-right
     license: unspecified · category: card
     This component creates a smooth, continuously rotating conic gradient glow behind any content.
The gradient’s vibrant colors transition seamlessly as it spins, producing a dynamic spotlight effect.
Its blurred, oversized shape adds depth and a soft ambient light, making foreground elements stand out. -->

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
components/ui/rotating-gradient-right.tsx
"use client";

import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { ArrowRight } from "lucide-react";

export default function RotatingGradientRight() {
  return (
    <section className="min-h-screen w-full bg-white dark:bg-black text-black dark:text-white px-8 py-16">
      <div className="mx-auto grid max-w-6xl items-center gap-12 md:grid-cols-2">
        {/* LEFT: Text */}
        <div className="relative mx-auto flex h-[40rem] w-full max-w-[60rem] items-center justify-center overflow-hidden rounded-3xl">
          {/* Rotating conic gradient glow */}
          <div className="absolute -inset-10 flex items-center justify-center">
            <div
              className="
                h-[120%] w-[120%] rounded-[36px] blur-3xl opacity-80
                bg-[conic-gradient(from_0deg,theme(colors.emerald.400),theme(colors.cyan.400),theme(colors.blue.500),theme(colors.violet.600),theme(colors.red.500),theme(colors.emerald.400))]
                animate-[spin_8s_linear_infinite]
              "
            />
          </div>

          {/* Black card inside the glow */}
          <Card className="w-[340px] z-10 rounded-2xl border border-white/10 bg-black/85 shadow-2xl backdrop-blur-xl">
            <CardContent className="p-5">
              <div className="mb-3 flex items-center justify-between">
                <span className="text-sm font-medium">Ruixen UI</span>
                <span className="text-xs text-zinc-400">99 / 99</span>
              </div>

              {/* Progress bar */}
              <div className="mb-3 h-1.5 w-full overflow-hidden rounded-full bg-white/10">
                <div
                  className="h-full w-[92%] rounded-full
                                bg-[linear-gradient(90deg,theme(colors.cyan.400),theme(colors.sky.400),theme(colors.emerald.400))]"
                />
              </div>

              <p className="text-xs text-zinc-400">
                Building components… please keep the project open until the
                process is complete.
              </p>

              <Button
                variant="secondary"
                className="mt-4 w-full rounded-lg bg-zinc-800 text-zinc-100 hover:bg-zinc-700"
              >
                Cancel
              </Button>
            </CardContent>
          </Card>
        </div>

        {/* RIGHT: Rotating gradient with black card */}
        <div className="space-y-4">
          <h2 className="text-lg sm:text-xl lg:text-3xl font-normal text-gray-900 dark:text-white leading-relaxed">
            Ruixen UI{" "}
            <span className="text-gray-500 dark:text-gray-400 text-sm sm:text-base lg:text-3xl">
              Build beautiful, modern interfaces with our comprehensive
              component library. No setup, no configuration needed.
            </span>
          </h2>
          <Button variant="link" className="px-0 text-black dark:text-white">
            Try Ruixen UI <ArrowRight />
          </Button>
        </div>
      </div>
    </section>
  );
}

demo.tsx
import RotatingGradientRight from "@/components/ui/rotating-gradient-right";

export default function DemoOne() {
  return <RotatingGradientRight />;
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button card
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
