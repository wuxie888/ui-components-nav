<!-- Help Dropdown Menu · @bundui · https://21st.dev/@bundui/components/dropdown-menu10
     license: MIT · category: navigation-menu
     An icon-triggered dropdown menu with a labeled help section linking to documentation, support, and contact options. -->

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
components/ui/index.tsx
import {
  BookIcon,
  InfoIcon,
  LifeBuoyIcon,
  MessageCircleMoreIcon,
} from "lucide-react";

import { Button } from "@/components/ui/button";
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";

export default function Component() {
  return (
    <DropdownMenu>
      <DropdownMenuTrigger asChild>
        <Button
          aria-label="Open edit menu"
          className="rounded-full shadow-none"
          size="icon"
          variant="ghost"
        >
          <InfoIcon aria-hidden="true" size={16} />
        </Button>
      </DropdownMenuTrigger>
      <DropdownMenuContent className="pb-2">
        <DropdownMenuLabel>Need help?</DropdownMenuLabel>
        <DropdownMenuItem
          asChild
          className="cursor-pointer py-1 focus:bg-transparent focus:underline"
        >
          <a href="#">
            <BookIcon aria-hidden="true" className="opacity-60" size={16} />
            Documentation
          </a>
        </DropdownMenuItem>
        <DropdownMenuItem
          asChild
          className="cursor-pointer py-1 focus:bg-transparent focus:underline"
        >
          <a href="#">
            <LifeBuoyIcon aria-hidden="true" className="opacity-60" size={16} />
            Support
          </a>
        </DropdownMenuItem>
        <DropdownMenuItem
          asChild
          className="cursor-pointer py-1 focus:bg-transparent focus:underline"
        >
          <a href="#">
            <MessageCircleMoreIcon
              aria-hidden="true"
              className="opacity-60"
              size={16}
            />
            Contact us
          </a>
        </DropdownMenuItem>
      </DropdownMenuContent>
    </DropdownMenu>
  );
}

demo.tsx
import {
  BookIcon,
  InfoIcon,
  LifeBuoyIcon,
  MessageCircleMoreIcon,
} from "lucide-react";

import { Button } from "@/components/ui/button";
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";

export default function Demo() {
  return (
    <div className="flex min-h-52 flex-col items-center justify-start pt-4">
      <DropdownMenu defaultOpen modal={false}>
        <DropdownMenuTrigger asChild>
          <Button
            aria-label="Open edit menu"
            className="rounded-full shadow-none"
            size="icon"
            variant="ghost"
          >
            <InfoIcon aria-hidden="true" size={16} />
          </Button>
        </DropdownMenuTrigger>
        <DropdownMenuContent align="center" className="pb-2" sideOffset={6}>
          <DropdownMenuLabel>Need help?</DropdownMenuLabel>
          <DropdownMenuItem
            asChild
            className="cursor-pointer py-1 focus:bg-transparent focus:underline"
          >
            <a href="#">
              <BookIcon aria-hidden="true" className="opacity-60" size={16} />
              Documentation
            </a>
          </DropdownMenuItem>
          <DropdownMenuItem
            asChild
            className="cursor-pointer py-1 focus:bg-transparent focus:underline"
          >
            <a href="#">
              <LifeBuoyIcon
                aria-hidden="true"
                className="opacity-60"
                size={16}
              />
              Support
            </a>
          </DropdownMenuItem>
          <DropdownMenuItem
            asChild
            className="cursor-pointer py-1 focus:bg-transparent focus:underline"
          >
            <a href="#">
              <MessageCircleMoreIcon
                aria-hidden="true"
                className="opacity-60"
                size={16}
              />
              Contact us
            </a>
          </DropdownMenuItem>
        </DropdownMenuContent>
      </DropdownMenu>
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
npx shadcn@latest add button dropdown-menu
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
