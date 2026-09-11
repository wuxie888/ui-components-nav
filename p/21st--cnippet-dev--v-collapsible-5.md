<!-- Collapsible Form Fields · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-collapsible-5
     license: no-license · category: pricing-section
     A pricing card with a collapsible section that reveals advanced input fields such as tax rate and discount. -->

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
components/ui/v-collapsible-5.tsx
"use client";

import { Settings2Icon } from "lucide-react";
import { useState } from "react";
import { Button } from "@/registry/default/ui/button";
import {
  Card,
  CardContent,
  CardHeader,
  CardTitle,
} from "@/registry/default/ui/card";
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from "@/registry/default/ui/collapsible";
import { Field, FieldLabel } from "@/registry/default/ui/field";
import {
  InputGroup,
  InputGroupAddon,
  InputGroupInput,
  InputGroupText,
} from "@/registry/default/ui/input-group";

export function Pattern() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div className="h-54 w-full max-w-xs">
      <Card>
        <CardHeader>
          <CardTitle>Unit Pricing</CardTitle>
        </CardHeader>
        <CardContent>
          <Collapsible
            className="flex flex-col gap-3"
            onOpenChange={setIsOpen}
            open={isOpen}
          >
            <div className="flex items-end gap-2">
              <Field className="flex-1">
                <FieldLabel className="sr-only">Base Price</FieldLabel>
                <InputGroup>
                  <InputGroupInput
                    defaultValue="19.00"
                    placeholder="0.00"
                    type="number"
                  />
                  <InputGroupAddon align="inline-end">
                    <InputGroupText>$</InputGroupText>
                  </InputGroupAddon>
                </InputGroup>
              </Field>
              <CollapsibleTrigger
                render={
                  <Button className="shrink-0" size="icon" variant="outline" />
                }
              >
                <Settings2Icon aria-hidden="true" className="size-3.5" />
              </CollapsibleTrigger>
            </div>

            <CollapsibleContent className="data-[state=closed]:animate-collapsible-up data-[state=open]:animate-collapsible-down">
              <Field>
                <FieldLabel>Tax Rate (%)</FieldLabel>
                <InputGroup>
                  <InputGroupInput
                    defaultValue="15"
                    placeholder="0"
                    type="number"
                  />
                  <InputGroupAddon align="inline-end">
                    <InputGroupText>%</InputGroupText>
                  </InputGroupAddon>
                </InputGroup>
              </Field>
              <Field>
                <FieldLabel>Discount (%)</FieldLabel>
                <InputGroup>
                  <InputGroupInput
                    defaultValue="0"
                    placeholder="0"
                    type="number"
                  />
                  <InputGroupAddon align="inline-end">
                    <InputGroupText>%</InputGroupText>
                  </InputGroupAddon>
                </InputGroup>
              </Field>
            </CollapsibleContent>
          </Collapsible>
        </CardContent>
      </Card>
    </div>
  );
}

demo.tsx
"use client";

import { Settings2Icon } from "lucide-react";
import { useState } from "react";
import { Button } from "@/components/ui/v-collapsible-5-utils/button";
import {
  Card,
  CardContent,
  CardHeader,
  CardTitle,
} from "@/components/ui/v-collapsible-5-utils/card";
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from "@/components/ui/v-collapsible-5-utils/collapsible";
import { Field, FieldLabel } from "@/components/ui/v-collapsible-5-utils/field";
import {
  InputGroup,
  InputGroupAddon,
  InputGroupInput,
  InputGroupText,
} from "@/components/ui/v-collapsible-5-utils/input-group";

function Pattern() {
  const [isOpen, setIsOpen] = useState(true);

  return (
    <div className="w-full max-w-xs">
      <Card>
        <CardHeader>
          <CardTitle>Unit Pricing</CardTitle>
        </CardHeader>
        <CardContent>
          <Collapsible
            className="flex flex-col gap-3"
            onOpenChange={setIsOpen}
            open={isOpen}
          >
            <div className="flex items-end gap-2">
              <Field className="flex-1">
                <FieldLabel className="sr-only">Base Price</FieldLabel>
                <InputGroup>
                  <InputGroupInput
                    defaultValue="19.00"
                    placeholder="0.00"
                    type="number"
                  />
                  <InputGroupAddon align="inline-end">
                    <InputGroupText>$</InputGroupText>
                  </InputGroupAddon>
                </InputGroup>
              </Field>
              <CollapsibleTrigger
                render={
                  <Button className="shrink-0" size="icon" variant="outline" />
                }
              >
                <Settings2Icon aria-hidden="true" className="size-3.5" />
              </CollapsibleTrigger>
            </div>

            <CollapsibleContent className="flex flex-col gap-3 data-[state=closed]:animate-collapsible-up data-[state=open]:animate-collapsible-down">
              <Field>
                <FieldLabel>Tax Rate (%)</FieldLabel>
                <InputGroup>
                  <InputGroupInput
                    defaultValue="15"
                    placeholder="0"
                    type="number"
                  />
                  <InputGroupAddon align="inline-end">
                    <InputGroupText>%</InputGroupText>
                  </InputGroupAddon>
                </InputGroup>
              </Field>
              <Field>
                <FieldLabel>Discount (%)</FieldLabel>
                <InputGroup>
                  <InputGroupInput
                    defaultValue="0"
                    placeholder="0"
                    type="number"
                  />
                  <InputGroupAddon align="inline-end">
                    <InputGroupText>%</InputGroupText>
                  </InputGroupAddon>
                </InputGroup>
              </Field>
            </CollapsibleContent>
          </Collapsible>
        </CardContent>
      </Card>
    </div>
  );
}

export default function Default() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center p-6">
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
npx shadcn@latest add button card collapsible input textarea
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
