<!-- Button Group Input · @uiable · https://21st.dev/@uiable/components/button-group-input
     license: no-license · category: form
     A search-style input paired with an attached search button, grouped together as a single connected control. -->

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
components/uiable/button-group/button-group-input.tsx
// shadcn
import { Button } from "@/components/ui/button"
import { ButtonGroup } from "@/components/ui/button-group"
import { Input } from "@/components/ui/input"

// assets
import { SearchIcon } from "lucide-react"

//  ------------------------------ | BUTTON GROUP - INPUT | ------------------------------  //

export default function ButtonGroupInput() {
  return (
    <ButtonGroup className="items-stretch">
      <Input placeholder="Search..." className="z-10" />
      <Button variant="outline" aria-label="Search">
        <SearchIcon />
      </Button>
    </ButtonGroup>
  )
}

demo.tsx
import ButtonGroupInput from "@/components/ui/button-group-input";

export default function ButtonGroupInputDemo() {
  return (
    <div className="flex items-center justify-center min-h-[200px] w-full">
      <ButtonGroupInput />
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
npx shadcn@latest add button button-group input separator
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
