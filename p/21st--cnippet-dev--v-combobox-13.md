<!-- Multi-Select Combobox Form Field · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-combobox-13
     license: no-license · category: form
     A form field with a multi-select combobox that renders selected values as removable chips, filters options as you type, and validates on submit. -->

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
components/ui/v-combobox-13.tsx
"use client";

import type { FormEvent } from "react";
import { useState } from "react";
import { Button } from "@/registry/default/ui/button";
import {
  Combobox,
  ComboboxChip,
  ComboboxChips,
  ComboboxChipsInput,
  ComboboxEmpty,
  ComboboxItem,
  ComboboxList,
  ComboboxPopup,
  ComboboxValue,
} from "@/registry/default/ui/combobox";
import { Field, FieldError, FieldLabel } from "@/registry/default/ui/field";
import { Form } from "@/registry/default/ui/form";

const items = [
  { label: "Apple", value: "apple" },
  { label: "Banana", value: "banana" },
  { label: "Orange", value: "orange" },
  { label: "Grape", value: "grape" },
  { label: "Strawberry", value: "strawberry" },
  { label: "Mango", value: "mango" },
  { label: "Pineapple", value: "pineapple" },
  { label: "Kiwi", value: "kiwi" },
  { label: "Peach", value: "peach" },
  { label: "Pear", value: "pear" },
];

export default function Particle() {
  const [loading, setLoading] = useState(false);
  const onSubmit = async (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const formData = new FormData(e.currentTarget);
    const selectedItems = formData.getAll("items");
    const itemValues = selectedItems.map(
      (selectedItem) =>
        items.find((item) => item.label === selectedItem)?.value ||
        selectedItem,
    );
    setLoading(true);
    await new Promise((r) => setTimeout(r, 800));
    setLoading(false);
    alert(`Favorite items: ${itemValues.join(", ") || ""}`);
  };

  return (
    <Form className="flex w-full max-w-64 flex-col gap-4" onSubmit={onSubmit}>
      <Field name="items">
        <FieldLabel>Favorite items</FieldLabel>
        <Combobox items={items} multiple required>
          <ComboboxChips>
            <ComboboxValue>
              {(value: { value: string; label: string }[]) => (
                <>
                  {value?.map((item) => (
                    <ComboboxChip aria-label={item.label} key={item.value}>
                      {item.label}
                    </ComboboxChip>
                  ))}
                  <ComboboxChipsInput
                    placeholder={value.length > 0 ? undefined : "Select items…"}
                  />
                </>
              )}
            </ComboboxValue>
          </ComboboxChips>
          <ComboboxPopup>
            <ComboboxEmpty>No items found.</ComboboxEmpty>
            <ComboboxList>
              {(item) => (
                <ComboboxItem key={item.value} value={item}>
                  {item.label}
                </ComboboxItem>
              )}
            </ComboboxList>
          </ComboboxPopup>
        </Combobox>
        <FieldError>Please select at least one item.</FieldError>
      </Field>
      <Button loading={loading} type="submit">
        Submit
      </Button>
    </Form>
  );
}

demo.tsx
import Combobox13 from "@/components/ui/v-combobox-13";

export default function Default() {
  return (
    <div className="flex min-h-72 w-full items-center justify-center p-6">
      <Combobox13 />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add combobox
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
