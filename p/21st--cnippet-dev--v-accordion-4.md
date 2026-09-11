<!-- Controlled Accordion · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-accordion-4
     license: no-license · category: faq
     A controlled accordion built on Base UI whose open panels are driven by external React state, with a button to expand items programmatically and a live readout of which items are open. -->

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
components/ui/v-accordion-4.tsx
"use client";

import { useState } from "react";
import {
  Accordion,
  AccordionItem,
  AccordionPanel,
  AccordionTrigger,
} from "@/registry/default/ui/accordion";
import { Button } from "@/registry/default/ui/button";

export default function Particle() {
  const [value, setValue] = useState<string[]>([]);

  return (
    <div className="flex w-full flex-col gap-4">
      <Accordion className="w-full" onValueChange={setValue} value={value}>
        <AccordionItem value="item-1">
          <AccordionTrigger>What is Base UI?</AccordionTrigger>
          <AccordionPanel>
            Base UI is a library of high-quality unstyled React components for
            design systems and web apps.
          </AccordionPanel>
        </AccordionItem>
        <AccordionItem value="item-2">
          <AccordionTrigger>How do I get started?</AccordionTrigger>
          <AccordionPanel>
            Head to the "Quick start" guide in the docs. If you've used unstyled
            libraries before, you'll feel at home.
          </AccordionPanel>
        </AccordionItem>
        <AccordionItem value="item-3">
          <AccordionTrigger>Can I use it for my project?</AccordionTrigger>
          <AccordionPanel>
            Of course! Base UI is free and open source.
          </AccordionPanel>
        </AccordionItem>
      </Accordion>

      <div className="flex flex-col items-start gap-4">
        <Button
          onClick={() => setValue(["item-1", "item-2"])}
          variant="outline"
        >
          Open First Two
        </Button>
        <p className="text-muted-foreground text-sm">
          Open items: {value.length > 0 ? value.join(", ") : "None"}
        </p>
      </div>
    </div>
  );
}

demo.tsx
import Particle from "@/components/ui/v-accordion-4";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-8">
      <div className="w-full max-w-md">
        <Particle />
      </div>
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add accordion button
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
