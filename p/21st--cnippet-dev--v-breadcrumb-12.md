<!-- Checkout Steps Breadcrumb · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-breadcrumb-12
     license: no-license · category: steps
     A checkout progress breadcrumb with numbered step circles, completed-step checkmarks, and back/continue navigation buttons. -->

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
components/ui/v-breadcrumb-12.tsx
"use client";

import { CheckIcon } from "lucide-react";
import { Fragment, useState } from "react";
import {
  Breadcrumb,
  BreadcrumbItem,
  BreadcrumbList,
  BreadcrumbPage,
  BreadcrumbSeparator,
} from "@/registry/default/ui/breadcrumb";
import { Button } from "@/registry/default/ui/button";

const steps = [
  { id: 1, label: "Cart" },
  { id: 2, label: "Shipping" },
  { id: 3, label: "Payment" },
  { id: 4, label: "Review" },
];

export function Pattern() {
  const [current, setCurrent] = useState(2);

  return (
    <div className="flex flex-col items-center gap-6">
      <Breadcrumb>
        <BreadcrumbList className="gap-1 sm:gap-1">
          {steps.map((step, i) => {
            const done = step.id < current;
            const active = step.id === current;
            return (
              <Fragment key={step.id}>
                <BreadcrumbItem>
                  {active ? (
                    <BreadcrumbPage className="flex items-center gap-1.5">
                      <span className="flex size-5 items-center justify-center rounded-full bg-primary font-bold text-[10px] text-primary-foreground">
                        {step.id}
                      </span>
                      <span className="font-semibold text-sm">
                        {step.label}
                      </span>
                    </BreadcrumbPage>
                  ) : (
                    <button
                      className={`flex items-center gap-1.5 text-sm ${done ? "text-foreground" : "text-muted-foreground/50"}`}
                      disabled={!done}
                      onClick={() => done && setCurrent(step.id)}
                      type="button"
                    >
                      <span
                        className={`flex size-5 items-center justify-center rounded-full font-bold text-[10px] ${
                          done
                            ? "bg-emerald-500 text-white"
                            : "border text-muted-foreground/50"
                        }`}
                      >
                        {done ? <CheckIcon className="size-3" /> : step.id}
                      </span>
                      {step.label}
                    </button>
                  )}
                </BreadcrumbItem>
                {i < steps.length - 1 && <BreadcrumbSeparator />}
              </Fragment>
            );
          })}
        </BreadcrumbList>
      </Breadcrumb>
      <div className="flex gap-2">
        <Button
          disabled={current === 1}
          onClick={() => setCurrent((c) => Math.max(1, c - 1))}
          size="sm"
          variant="outline"
        >
          Back
        </Button>
        <Button
          disabled={current === steps.length}
          onClick={() => setCurrent((c) => Math.min(steps.length, c + 1))}
          size="sm"
        >
          {current === steps.length - 1 ? "Place Order" : "Continue"}
        </Button>
      </div>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-breadcrumb-12";

export default function Default() {
  return (
    <div className="flex w-full items-center justify-center bg-background px-6 py-10">
      <Pattern />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add breadcrumb button
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
