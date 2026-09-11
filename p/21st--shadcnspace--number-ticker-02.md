<!-- Number Ticker Currency Counter · @shadcnspace · https://21st.dev/@shadcnspace/components/number-ticker-02
     license: no-license · category: pricing-section
     An animated currency counter that smoothly rolls between values for displaying prices, balances, or real-time financial figures. -->

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
components/shadcn-space/number-ticker/number-ticker-02.tsx
"use client";

import { useEffect, useState } from "react";
import NumberFlow, { type Value } from "@number-flow/react";

type NumberTickerProps = {
  value: Value;
  currency?: string;
  decimals?: number;
  className?: string;
};

/**
 * NumberTicker 02 - Money Flow
 * Premium currency display with smooth high-fidelity rolls.
 */
function NumberTicker({
  value,
  currency = "USD",
  decimals = 2,
  className,
}: NumberTickerProps) {
  return (
    <div className="inline-flex items-center gap-3">
      <NumberFlow
        value={value}
        format={{
          style: "currency",
          currency: currency,
          minimumFractionDigits: decimals,
          maximumFractionDigits: decimals,
        }}
        className={className}
      />
    </div>
  );
}

const NumberTickerDemo = () => {
  const [val, setVal] = useState(1284.5);

  useEffect(() => {
    const interval = setInterval(() => {
      setVal((prev) => prev + (Math.random() * 50 - 20));
    }, 2500);
    return () => clearInterval(interval);
  }, []);

  return (
    <div>
      <NumberTicker
        value={val}
        className="text-foreground font-medium lg:text-5xl sm:text-4xl text-3xl tracking-tight"
      />
    </div>
  );
};

export default NumberTickerDemo;

demo.tsx
"use client";

import * as React from "react";
import NumberTicker from "@/components/ui/number-ticker-02";

export default function NumberTickerDemo() {
  const [val, setVal] = React.useState(1284.5);

  React.useEffect(() => {
    const interval = setInterval(() => {
      setVal((prev) => prev + (Math.random() * 50 - 20));
    }, 2500);
    return () => clearInterval(interval);
  }, []);

  return (
    <div>
      <NumberTicker
        value={val}
        className="text-foreground font-medium lg:text-5xl sm:text-4xl text-3xl tracking-tight"
      />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @number-flow/react lucide-react
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
