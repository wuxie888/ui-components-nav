<!-- Text Shimmer Animation · @stackingsu · https://21st.dev/@stackingsu/components/text-shimmer-animation-on-hover
     license: no-license · category: pricing-section
     An animated text effect that sweeps a multi-color gradient shimmer across the characters for emphasis or highlights. -->

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
components/uui/text-shimmer-animation-on-hover/example.tsx
'use client';

import { Button } from '@/components/ui/button';
import { TextShimmer } from './text-shimmer';

const features = ['Unlimited API requests', 'Priority inference queue', '128K context window', 'Dedicated support'];

export function PricingCard() {
  return (
    <div className="h-svh flex items-center justify-center px-4">
      <article className="bg-card w-full mx-auto md:max-w-sm p-6">
        <header>
          <TextShimmer className="font-medium">Ultra Tier</TextShimmer>
        </header>

        <div className="mt-6">
          <h1 className="text-lg font-medium text-muted-foreground">Usage-based</h1>
          <div className="mt-2 flex items-baseline gap-1.5">
            <span className="text-xl font-medium">$0.01</span>
            <span className="text-sm text-muted-foreground">/ 1K tokens</span>
          </div>
          <p className="mt-2 text-sm text-muted-foreground">Pay only for what you use. No monthly minimum.</p>
        </div>

        <ul className="mt-6 space-y-3">
          {features.map(feature => (
            <li key={feature} className="flex items-center gap-2 text-sm">
              <span className="size-2 bg-primary" />
              <span>{feature}</span>
            </li>
          ))}
        </ul>
        <footer className="mt-6">
          <Button variant="secondary" className="bg-muted w-full h-11 text-sm">
            <TextShimmer className="delay-1500">Get Started Now</TextShimmer>
          </Button>
        </footer>
      </article>
    </div>
  );
}

components/uui/text-shimmer-animation-on-hover/text-shimmer.tsx
import { cn } from '@/lib/utils';
import * as React from 'react';

export const TextShimmer = ({ children, className }: { children: React.ReactNode; className?: string }) => {
  return (
    <span
      className={cn(
        'bg-[linear-gradient(90deg,var(--foreground)_0%,var(--foreground)_var(--highlight-x),#f98dbe_calc(var(--highlight-x)+8%),#edb541_calc(var(--highlight-x)+28%),#54c546_calc(var(--highlight-x)+48%),var(--foreground)_calc(var(--highlight-x)+56%),var(--foreground)_100%)] animate-shimmer [-webkit-text-fill-color:transparent] bg-clip-text inline-block',
        className,
      )}
    >
      {children}
    </span>
  );
};

demo.tsx
'use client';

import { Button } from '@/components/ui/button';
import { TextShimmer } from '@/components/ui/text-shimmer-animation-on-hover';

const features = ['Unlimited API requests', 'Priority inference queue', '128K context window', 'Dedicated support'];

export default function PricingCard() {
  return (
    <div className="h-svh flex items-center justify-center px-4">
      <article className="bg-card w-full mx-auto md:max-w-sm p-6">
        <header>
          <TextShimmer className="font-medium">Ultra Tier</TextShimmer>
        </header>

        <div className="mt-6">
          <h1 className="text-lg font-medium text-muted-foreground">Usage-based</h1>
          <div className="mt-2 flex items-baseline gap-1.5">
            <span className="text-xl font-medium">$0.01</span>
            <span className="text-sm text-muted-foreground">/ 1K tokens</span>
          </div>
          <p className="mt-2 text-sm text-muted-foreground">Pay only for what you use. No monthly minimum.</p>
        </div>

        <ul className="mt-6 space-y-3">
          {features.map(feature => (
            <li key={feature} className="flex items-center gap-2 text-sm">
              <span className="size-2 bg-primary" />
              <span>{feature}</span>
            </li>
          ))}
        </ul>
        <footer className="mt-6">
          <Button variant="secondary" className="bg-muted w-full h-11 text-sm">
            <TextShimmer className="delay-1500">Get Started Now</TextShimmer>
          </Button>
        </footer>
      </article>
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
