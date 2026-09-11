<!-- Floating Navbar · @arihantcodes_1f7b8c4d · https://21st.dev/@arihantcodes_1f7b8c4d/components/floating-navbar
     license: no-license · category: dropdown
     A pill-shaped floating navigation bar with rounded icon buttons and a dropdown action menu for quick app navigation. -->

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
components/ui/floatingnavbar.tsx
import * as React from "react";
import { Button } from "@/components/ui/button";
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";
import { Home, Menu, MessageSquare, Plus, Settings, Users } from "lucide-react";

export default function Floatingnavbar() {
  return (
    // add fixed  to the nav class name to make the navbar stick to the bottom of the screen
    <div className=" bottom-6 left-0 right-0 flex justify-center">
      <nav className="flex items-center justify-center space-x-4 rounded-full border bg-background p-2 shadow-lg">
        <Button variant="ghost" size="icon" className="rounded-full">
          <Home className="h-5 w-5" />
          <span className="sr-only">Home</span>
        </Button>
        <Button variant="ghost" size="icon" className="rounded-full">
          <Users className="h-5 w-5" />
          <span className="sr-only">Users</span>
        </Button>
        <DropdownMenu>
          <DropdownMenuTrigger asChild>
            <Button
              size="icon"
              className="rounded-full bg-primary text-primary-foreground"
            >
              <Plus className="h-5 w-5" />
              <span className="sr-only">Add</span>
            </Button>
          </DropdownMenuTrigger>
          <DropdownMenuContent align="center">
            <DropdownMenuItem>
              <Users className="mr-2 h-4 w-4" />
              New Team
            </DropdownMenuItem>
            <DropdownMenuItem>
              <MessageSquare className="mr-2 h-4 w-4" />
              New Chat
            </DropdownMenuItem>
            <DropdownMenuItem>
              <Settings className="mr-2 h-4 w-4" />
              Settings
            </DropdownMenuItem>
          </DropdownMenuContent>
        </DropdownMenu>
        <Button variant="ghost" size="icon" className="rounded-full">
          <MessageSquare className="h-5 w-5" />
          <span className="sr-only">Messages</span>
        </Button>
        <Button variant="ghost" size="icon" className="rounded-full">
          <Menu className="h-5 w-5" />
          <span className="sr-only">Menu</span>
        </Button>
      </nav>
    </div>
  );
}

demo.tsx
import FloatingNavbar from "@/components/ui/floating-navbar";

export default function Default() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center bg-background p-8">
      <FloatingNavbar />
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
