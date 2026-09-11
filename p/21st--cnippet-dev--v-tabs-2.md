<!-- Underline Tabs · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-tabs-2
     license: MIT · category: navigation-menu
     A tabs component with an animated underline indicator for switching between content panels. -->

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
components/ui/v-tabs-2.tsx
import { Tabs, TabsList, TabsPanel, TabsTab } from "@/registry/default/ui/tabs";

export default function Particle() {
  return (
    <Tabs defaultValue="tab-1">
      <div className="border-b">
        <TabsList variant="underline">
          <TabsTab value="tab-1">Tab 1</TabsTab>
          <TabsTab value="tab-2">Tab 2</TabsTab>
          <TabsTab value="tab-3">Tab 3</TabsTab>
        </TabsList>
      </div>
      <TabsPanel value="tab-1">
        <p className="p-4 text-center text-muted-foreground text-xs">
          Tab 1 content
        </p>
      </TabsPanel>
      <TabsPanel value="tab-2">
        <p className="p-4 text-center text-muted-foreground text-xs">
          Tab 2 content
        </p>
      </TabsPanel>
      <TabsPanel value="tab-3">
        <p className="p-4 text-center text-muted-foreground text-xs">
          Tab 3 content
        </p>
      </TabsPanel>
    </Tabs>
  );
}

demo.tsx
import Particle from "@/components/ui/v-tabs-2";

export default function Default() {
  return (
    <div className="w-full max-w-md">
      <Particle />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add tabs
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
