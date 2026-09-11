<!-- Add Block Dropdown Menu · @bundui · https://21st.dev/@bundui/components/dropdown-menu11
     license: MIT · category: dropdown
     A dropdown menu that lets users add a content block, listing options like text, quote, divider and headings each with an icon and description. -->

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
  Heading1Icon,
  Heading2Icon,
  MinusIcon,
  PlusIcon,
  TextQuoteIcon,
  TypeIcon,
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
          <PlusIcon aria-hidden="true" size={16} />
        </Button>
      </DropdownMenuTrigger>
      <DropdownMenuContent className="pb-2">
        <DropdownMenuLabel>Add block</DropdownMenuLabel>
        <DropdownMenuItem>
          <div
            aria-hidden="true"
            className="flex size-8 items-center justify-center rounded-md border bg-background"
          >
            <TypeIcon className="opacity-60" size={16} />
          </div>
          <div>
            <div className="font-medium text-sm">Text</div>
            <div className="text-muted-foreground text-xs">
              Start writing with plain text
            </div>
          </div>
        </DropdownMenuItem>
        <DropdownMenuItem>
          <div
            aria-hidden="true"
            className="flex size-8 items-center justify-center rounded-md border bg-background"
          >
            <TextQuoteIcon className="opacity-60" size={16} />
          </div>
          <div>
            <div className="font-medium text-sm">Quote</div>
            <div className="text-muted-foreground text-xs">Capture a quote</div>
          </div>
        </DropdownMenuItem>
        <DropdownMenuItem>
          <div
            aria-hidden="true"
            className="flex size-8 items-center justify-center rounded-md border bg-background"
          >
            <MinusIcon className="opacity-60" size={16} />
          </div>
          <div>
            <div className="font-medium text-sm">Divider</div>
            <div className="text-muted-foreground text-xs">
              Visually divide blocks
            </div>
          </div>
        </DropdownMenuItem>
        <DropdownMenuItem>
          <div
            aria-hidden="true"
            className="flex size-8 items-center justify-center rounded-md border bg-background"
          >
            <Heading1Icon className="opacity-60" size={16} />
          </div>
          <div>
            <div className="font-medium text-sm">Heading 1</div>
            <div className="text-muted-foreground text-xs">
              Big section heading
            </div>
          </div>
        </DropdownMenuItem>
        <DropdownMenuItem>
          <div
            aria-hidden="true"
            className="flex size-8 items-center justify-center rounded-md border bg-background"
          >
            <Heading2Icon className="opacity-60" size={16} />
          </div>
          <div>
            <div className="font-medium text-sm">Heading 2</div>
            <div className="text-muted-foreground text-xs">
              Medium section subheading
            </div>
          </div>
        </DropdownMenuItem>
      </DropdownMenuContent>
    </DropdownMenu>
  );
}

demo.tsx
import {
  Heading1Icon,
  Heading2Icon,
  MinusIcon,
  PlusIcon,
  TextQuoteIcon,
  TypeIcon,
} from "lucide-react";

import { Button } from "@/components/ui/button";
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";

export default function DemoDropdownMenu() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center">
      <DropdownMenu defaultOpen>
        <DropdownMenuTrigger asChild>
          <Button
            aria-label="Open edit menu"
            className="rounded-full shadow-none"
            size="icon"
            variant="ghost"
          >
            <PlusIcon aria-hidden="true" size={16} />
          </Button>
        </DropdownMenuTrigger>
        <DropdownMenuContent className="pb-2" align="start" sideOffset={8}>
          <DropdownMenuLabel>Add block</DropdownMenuLabel>
          <DropdownMenuItem>
            <div
              aria-hidden="true"
              className="flex size-8 items-center justify-center rounded-md border bg-background"
            >
              <TypeIcon className="opacity-60" size={16} />
            </div>
            <div>
              <div className="font-medium text-sm">Text</div>
              <div className="text-muted-foreground text-xs">
                Start writing with plain text
              </div>
            </div>
          </DropdownMenuItem>
          <DropdownMenuItem>
            <div
              aria-hidden="true"
              className="flex size-8 items-center justify-center rounded-md border bg-background"
            >
              <TextQuoteIcon className="opacity-60" size={16} />
            </div>
            <div>
              <div className="font-medium text-sm">Quote</div>
              <div className="text-muted-foreground text-xs">
                Capture a quote
              </div>
            </div>
          </DropdownMenuItem>
          <DropdownMenuItem>
            <div
              aria-hidden="true"
              className="flex size-8 items-center justify-center rounded-md border bg-background"
            >
              <MinusIcon className="opacity-60" size={16} />
            </div>
            <div>
              <div className="font-medium text-sm">Divider</div>
              <div className="text-muted-foreground text-xs">
                Visually divide blocks
              </div>
            </div>
          </DropdownMenuItem>
          <DropdownMenuItem>
            <div
              aria-hidden="true"
              className="flex size-8 items-center justify-center rounded-md border bg-background"
            >
              <Heading1Icon className="opacity-60" size={16} />
            </div>
            <div>
              <div className="font-medium text-sm">Heading 1</div>
              <div className="text-muted-foreground text-xs">
                Big section heading
              </div>
            </div>
          </DropdownMenuItem>
          <DropdownMenuItem>
            <div
              aria-hidden="true"
              className="flex size-8 items-center justify-center rounded-md border bg-background"
            >
              <Heading2Icon className="opacity-60" size={16} />
            </div>
            <div>
              <div className="font-medium text-sm">Heading 2</div>
              <div className="text-muted-foreground text-xs">
                Medium section subheading
              </div>
            </div>
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
