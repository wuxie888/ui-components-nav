<!-- Autocomplete Field · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-field-7
     license: MIT · category: form
     A form field with an autocomplete search input that filters a list of options as you type and shows an empty state when nothing matches. -->

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
components/ui/v-field-7.tsx
"use client";

import {
  Autocomplete,
  AutocompleteEmpty,
  AutocompleteInput,
  AutocompleteItem,
  AutocompleteList,
  AutocompletePopup,
} from "@/registry/default/ui/autocomplete";
import {
  Field,
  FieldDescription,
  FieldLabel,
} from "@/registry/default/ui/field";

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
  return (
    <Field>
      <FieldLabel>Fruits</FieldLabel>
      <Autocomplete items={items}>
        <AutocompleteInput
          aria-label="Search items"
          placeholder="Search items…"
        />
        <AutocompletePopup>
          <AutocompleteEmpty>No items found.</AutocompleteEmpty>
          <AutocompleteList>
            {(item) => (
              <AutocompleteItem key={item.value} value={item}>
                {item.label}
              </AutocompleteItem>
            )}
          </AutocompleteList>
        </AutocompletePopup>
      </Autocomplete>
      <FieldDescription>Select a item.</FieldDescription>
    </Field>
  );
}

demo.tsx
import Particle from "@/components/ui/v-field-7";

export default function Default() {
  return (
    <div className="flex min-h-[360px] w-full items-start justify-center p-8">
      <div className="w-full max-w-xs">
        <Particle />
      </div>
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add field input scroll-area
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
