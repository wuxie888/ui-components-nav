<!-- Documentation Link Card · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-card-15
     license: no-license · category: icon
     A compact card with a labeled icon header and a body containing descriptive text plus a link, ideal for documentation or resource callouts. -->

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
components/ui/v-card-15.tsx
import { BookOpenIcon, LinkIcon } from "lucide-react";
import { Card, CardContent } from "@/registry/default/ui/card";

const item = {
  description:
    "Find guides, API references, and examples to integrate with our platform.",
  icon: <BookOpenIcon aria-hidden="true" />,
  label: "Documentation",
  link: "View docs",
};

export function Pattern() {
  return (
    <Card className="w-full max-w-xs p-0">
      <CardContent className="p-0">
        <div className="border-b px-4 py-3">
          <div className="flex items-center gap-2 text-muted-foreground [&_svg]:size-4">
            {item.icon}
            <span className="font-medium text-foreground text-sm">
              {item.label}
            </span>
          </div>
        </div>
        <div className="space-y-3 p-4">
          <p className="text-muted-foreground text-sm leading-relaxed">
            {item.description}
          </p>
          <a
            className="inline-flex items-center gap-1 font-medium text-primary text-xs hover:underline"
            href="#"
          >
            <LinkIcon aria-hidden="true" className="size-2.5 shrink-0" />
            {item.link}
          </a>
        </div>
      </CardContent>
    </Card>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-card-15";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-6">
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
