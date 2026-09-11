<!-- Payment Information Fieldset · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-fieldset-6
     license: MIT · category: form
     A payment information fieldset grouping cardholder name, card number, expiry date, and CVC inputs with a legend, labels, and inline validation errors. -->

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
components/ui/v-fieldset-6.tsx
import {
  Field,
  FieldDescription,
  FieldError,
  FieldLabel,
} from "@/registry/default/ui/field";
import { Fieldset, FieldsetLegend } from "@/registry/default/ui/fieldset";
import { Input } from "@/registry/default/ui/input";

export default function Particle() {
  return (
    <Fieldset className="flex w-full flex-col gap-5">
      <FieldsetLegend>Payment Information</FieldsetLegend>
      <Field>
        <FieldLabel>
          Cardholder name <span className="text-destructive-foreground">*</span>
        </FieldLabel>
        <Input
          autoComplete="cc-name"
          placeholder="Jane Smith"
          required
          type="text"
        />
        <FieldError>Cardholder name is required.</FieldError>
      </Field>
      <Field>
        <FieldLabel>
          Card number <span className="text-destructive-foreground">*</span>
        </FieldLabel>
        <Input
          autoComplete="cc-number"
          inputMode="numeric"
          placeholder="1234 5678 9012 3456"
          required
          type="text"
        />
        <FieldError>Please enter a valid card number.</FieldError>
      </Field>
      <div className="grid grid-cols-2 gap-4">
        <Field>
          <FieldLabel>
            Expiry date <span className="text-destructive-foreground">*</span>
          </FieldLabel>
          <Input
            autoComplete="cc-exp"
            placeholder="MM / YY"
            required
            type="text"
          />
          <FieldError>Invalid expiry date.</FieldError>
        </Field>
        <Field>
          <FieldLabel>
            CVC <span className="text-destructive-foreground">*</span>
          </FieldLabel>
          <Input
            autoComplete="cc-csc"
            inputMode="numeric"
            placeholder="123"
            required
            type="text"
          />
          <FieldDescription>3 or 4 digits on your card.</FieldDescription>
          <FieldError>Invalid CVC.</FieldError>
        </Field>
      </div>
    </Fieldset>
  );
}

demo.tsx
import Component from "@/components/ui/v-fieldset-6";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <div className="w-full max-w-sm">
        <Component />
      </div>
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add fieldset input
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
