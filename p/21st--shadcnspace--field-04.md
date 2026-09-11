<!-- Shipping Address Fieldset · @shadcnspace · https://21st.dev/@shadcnspace/components/field-04
     license: MIT · category: form
     A shipping address form fieldset with first/last name, street, city, state, postal code, country select and a save-address checkbox, built on the shadcn Field primitive. -->

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
components/shadcn-space/field/field-04.tsx
import { Checkbox } from "@/components/ui/checkbox";
import {
  Field,
  FieldDescription,
  FieldGroup,
  FieldLabel,
  FieldLegend,
  FieldSet,
} from "@/components/ui/field";
import { Input } from "@/components/ui/input";
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select";

const ShippingAddressDemo = () => {
  return (
    <FieldSet className="w-full max-w-lg">
      <FieldLegend>Shipping address</FieldLegend>
      <FieldDescription>
        We&apos;ll use this address for delivery and order updates.
      </FieldDescription>
      <FieldGroup>
        <div className="grid grid-cols-2 gap-4">
          <Field>
            <FieldLabel htmlFor="shipping-first-name">First name</FieldLabel>
            <Input id="shipping-first-name" placeholder="Jane" />
          </Field>
          <Field>
            <FieldLabel htmlFor="shipping-last-name">Last name</FieldLabel>
            <Input id="shipping-last-name" placeholder="Doe" />
          </Field>
        </div>
        <Field>
          <FieldLabel htmlFor="shipping-address">Street address</FieldLabel>
          <Input id="shipping-address" placeholder="123 Main Street" />
        </Field>
        <div className="grid grid-cols-2 gap-4">
          <Field>
            <FieldLabel htmlFor="shipping-city">City</FieldLabel>
            <Input id="shipping-city" placeholder="San Francisco" />
          </Field>
          <Field>
            <FieldLabel htmlFor="shipping-state">State</FieldLabel>
            <Input id="shipping-state" placeholder="California" />
          </Field>
        </div>
        <div className="grid grid-cols-2 gap-4">
          <Field>
            <FieldLabel htmlFor="shipping-zip">Postal code</FieldLabel>
            <Input id="shipping-zip" placeholder="94103" />
          </Field>
          <Field>
            <FieldLabel htmlFor="shipping-country">Country</FieldLabel>
            <Select defaultValue="United States">
              <SelectTrigger id="shipping-country" className="w-full">
                <SelectValue placeholder="Select a country" />
              </SelectTrigger>
              <SelectContent>
                <SelectItem value="United States">United States</SelectItem>
                <SelectItem value="Canada">Canada</SelectItem>
                <SelectItem value="United Kingdom">United Kingdom</SelectItem>
                <SelectItem value="India">India</SelectItem>
              </SelectContent>
            </Select>
          </Field>
        </div>
        <Field orientation="horizontal">
          <Checkbox id="shipping-save" defaultChecked />
          <FieldLabel htmlFor="shipping-save" className="font-normal">
            Save this address for future orders
          </FieldLabel>
        </Field>
      </FieldGroup>
    </FieldSet>
  );
};

export default ShippingAddressDemo;

demo.tsx
import ShippingAddressField from "@/components/ui/field-04";

export default function Field04Demo() {
  return (
    <div className="flex w-full items-center justify-center p-8">
      <ShippingAddressField />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add checkbox field input select
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
