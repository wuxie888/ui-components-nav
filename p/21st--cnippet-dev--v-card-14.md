<!-- Icon Link Card · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-card-14
     license: no-license · category: cta
     A compact card with an icon badge, title, description and a call-to-action link, useful for summaries, feature highlights or navigation shortcuts. -->

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
components/ui/v-card-14.tsx
import { ChevronRightIcon, ShoppingBagIcon } from "lucide-react";
import { Card, CardContent } from "@/registry/default/ui/card";

const item = {
  description:
    "Track and review all recent purchases, updates, and status changes in one place.",
  icon: <ShoppingBagIcon aria-hidden="true" />,
  link: "View Orders",
  title: "Recent Orders Overview",
};

export function Pattern() {
  return (
    <Card className="w-full max-w-xs">
      <CardContent className="flex flex-col gap-3">
        <div className="flex size-11 items-center justify-center rounded-md bg-primary [&_svg]:size-5 [&_svg]:text-primary-foreground">
          {item.icon}
        </div>
        <a
          className="block font-medium text-foreground text-sm leading-tight hover:text-primary"
          href="#"
        >
          {item.title}
        </a>
        <p className="text-muted-foreground text-xs leading-relaxed">
          {item.description}
        </p>
        <a
          className="inline-flex items-center gap-1 font-medium text-primary text-xs hover:underline"
          href="#"
        >
          {item.link}
          <ChevronRightIcon aria-hidden="true" className="size-2.5 shrink-0" />
        </a>
      </CardContent>
    </Card>
  );
}

demo.tsx
import VCard14 from "@/components/ui/v-card-14";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <VCard14 />
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
npx shadcn@latest add card
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
