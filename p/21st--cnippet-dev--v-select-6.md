<!-- Currency Select · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-select-6
     license: MIT · category: form
     A currency picker select dropdown that lists currencies with their symbol, name, and ISO code, built on Base UI. -->

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
components/ui/v-select-6.tsx
import {
  Select,
  SelectItem,
  SelectPopup,
  SelectTrigger,
  SelectValue,
} from "@/registry/default/ui/select";

type Currency = {
  code: string;
  label: string;
  symbol: string;
  value: string;
};

const currencies: Currency[] = [
  { code: "USD", label: "US Dollar", symbol: "$", value: "usd" },
  { code: "EUR", label: "Euro", symbol: "€", value: "eur" },
  { code: "GBP", label: "British Pound", symbol: "£", value: "gbp" },
  { code: "JPY", label: "Japanese Yen", symbol: "¥", value: "jpy" },
  { code: "CAD", label: "Canadian Dollar", symbol: "$", value: "cad" },
  { code: "AUD", label: "Australian Dollar", symbol: "$", value: "aud" },
  { code: "INR", label: "Indian Rupee", symbol: "₹", value: "inr" },
  { code: "CHF", label: "Swiss Franc", symbol: "₣", value: "chf" },
];

const placeholder = {
  code: "",
  label: "Select currency",
  symbol: "",
  value: null,
};
const allItems = [placeholder, ...currencies];

export default function Component() {
  return (
    <Select defaultValue={currencies[0]} items={allItems}>
      <SelectTrigger className="w-60">
        <SelectValue />
      </SelectTrigger>
      <SelectPopup>
        {currencies.map((item) => (
          <SelectItem key={item.value} value={item}>
            <span className="flex items-center gap-2">
              <span className="w-6 text-center font-mono text-muted-foreground text-sm">
                {item.symbol}
              </span>
              <span>{item.label}</span>
              <span className="ms-auto text-muted-foreground text-xs">
                {item.code}
              </span>
            </span>
          </SelectItem>
        ))}
      </SelectPopup>
    </Select>
  );
}

demo.tsx
import Component from "@/components/ui/v-select-6";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-8">
      <Component />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add select
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
