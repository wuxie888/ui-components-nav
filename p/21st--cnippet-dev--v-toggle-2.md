<!-- Outline Toggle Buttons · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-toggle-2
     license: no-license · category: toggle
     Toggle buttons with an outline variant for toggling text formatting options like bold and italic. -->

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
components/ui/v-toggle-2.tsx
import { BoldIcon, ItalicIcon } from "lucide-react";
import { Toggle } from "@/registry/default/ui/toggle";

export function Pattern() {
  return (
    <div className="flex flex-wrap items-center justify-center gap-2">
      <Toggle aria-label="Toggle italic" variant="outline">
        <ItalicIcon />
        Italic
      </Toggle>
      <Toggle aria-label="Toggle bold" variant="outline">
        <BoldIcon />
        Bold
      </Toggle>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-toggle-2";

export default function Default() {
  return <Pattern />;
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add toggle toggle?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068
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
