<!-- Account Menu · @ruixen.ui · https://21st.dev/@ruixen.ui/components/account-menu
     license: unspecified · category: dropdown
     The AccountMenu component is a modern, user-friendly dropdown menu designed for managing user account actions and preferences in a web application. It provides a clean interface with icons and labels, allowing users to easily navigate to their dashboard, profile settings, team spaces, and notifications. The menu supports nested submenus, radio groups, and checkbox items, enabling users to select themes, notification preferences, and other personalized settings. With proper alignment of icons and text, and checkmarks positioned on the right, the AccountMenu ensures a visually appealing, intuitive, and responsive experience, making it ideal for dashboards and account management panels. -->

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
components/ui/account-menu.tsx
"use client";

import * as React from "react";
import { Button } from "@/components/ui/button";
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuPortal,
  DropdownMenuRadioGroup,
  DropdownMenuRadioItem,
  DropdownMenuCheckboxItem,
  DropdownMenuSeparator,
  DropdownMenuSub,
  DropdownMenuSubContent,
  DropdownMenuSubTrigger,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";
import {
  User,
  Users,
  Settings,
  LayoutDashboard,
  LogOut,
  ChevronDown,
  Palette,
  Bell,
  Moon,
  Sun,
} from "lucide-react";

export default function AccountMenu() {
  return (
    <DropdownMenu>
      <DropdownMenuTrigger asChild>
        <Button
          variant="outline"
          className="flex items-center gap-2 rounded-xl border-border bg-background px-4 py-2 font-medium text-foreground hover:bg-muted"
        >
          <User className="h-5 w-5 text-foreground" />
          <span>Srinath G</span>
          <ChevronDown className="ml-1 h-4 w-4 text-muted-foreground" />
        </Button>
      </DropdownMenuTrigger>

      <DropdownMenuContent className="w-64 rounded-xl border-border bg-popover text-popover-foreground shadow-md p-2">
        {/* Account Section */}
        <DropdownMenuLabel className="text-sm text-muted-foreground">
          Account
        </DropdownMenuLabel>

        <DropdownMenuItem className="flex items-center gap-2 rounded-lg px-2 py-2 hover:bg-muted focus:bg-muted">
          <LayoutDashboard className="h-4 w-4" />
          <span className="flex-1">Dashboard</span>
        </DropdownMenuItem>

        <DropdownMenuItem className="flex items-center gap-2 rounded-lg px-2 py-2 hover:bg-muted focus:bg-muted">
          <Users className="h-4 w-4" />
          <span className="flex-1">Team Space</span>
        </DropdownMenuItem>

        <DropdownMenuItem className="flex items-center gap-2 rounded-lg px-2 py-2 hover:bg-muted focus:bg-muted">
          <Settings className="h-4 w-4" />
          <span className="flex-1">Settings</span>
        </DropdownMenuItem>

        <DropdownMenuSeparator className="my-1" />

        {/* Preferences Section */}
        <DropdownMenuLabel className="text-sm text-muted-foreground">
          Preferences
        </DropdownMenuLabel>

        {/* Theme Submenu */}
        <DropdownMenuSub>
          <DropdownMenuSubTrigger className="flex items-center gap-2 rounded-lg px-2 py-2 hover:bg-muted focus:bg-muted">
            <Palette className="h-4 w-4" />
            <span className="flex-1">Theme</span>
          </DropdownMenuSubTrigger>

          <DropdownMenuPortal>
            <DropdownMenuSubContent className="w-44 rounded-lg border-border bg-popover text-popover-foreground shadow-sm p-1">
              <DropdownMenuRadioGroup value="light">
                <DropdownMenuRadioItem
                  value="light"
                  className="flex items-center gap-2 rounded px-2 py-1 hover:bg-muted focus:bg-muted"
                >
                  <Sun className="h-4 w-4" />
                  <span className="flex-1">Light</span>
                </DropdownMenuRadioItem>

                <DropdownMenuRadioItem
                  value="dark"
                  className="flex items-center gap-2 rounded px-2 py-1 hover:bg-muted focus:bg-muted"
                >
                  <Moon className="h-4 w-4" />
                  <span className="flex-1">Dark</span>
                </DropdownMenuRadioItem>

                <DropdownMenuRadioItem
                  value="system"
                  className="flex items-center gap-2 rounded px-2 py-1 hover:bg-muted focus:bg-muted"
                >
                  <Bell className="h-4 w-4" />
                  <span className="flex-1">System Default</span>
                </DropdownMenuRadioItem>
              </DropdownMenuRadioGroup>
            </DropdownMenuSubContent>
          </DropdownMenuPortal>
        </DropdownMenuSub>

        {/* Notifications Submenu */}
        <DropdownMenuSub>
          <DropdownMenuSubTrigger className="flex items-center gap-2 rounded-lg px-2 py-2 hover:bg-muted focus:bg-muted">
            <Bell className="h-4 w-4" />
            <span className="flex-1">Notifications</span>
          </DropdownMenuSubTrigger>

          <DropdownMenuPortal>
            <DropdownMenuSubContent className="w-44 rounded-lg border-border bg-popover text-popover-foreground shadow-sm p-1">
              <DropdownMenuCheckboxItem className="flex items-center gap-2 rounded px-2 py-1 hover:bg-muted focus:bg-muted">
                <Bell className="h-4 w-4" />
                <span className="flex-1">Email Alerts</span>
              </DropdownMenuCheckboxItem>

              <DropdownMenuCheckboxItem className="flex items-center gap-2 rounded px-2 py-1 hover:bg-muted focus:bg-muted">
                <Bell className="h-4 w-4" />
                <span className="flex-1">Push Notifications</span>
              </DropdownMenuCheckboxItem>

              <DropdownMenuCheckboxItem className="flex items-center gap-2 rounded px-2 py-1 hover:bg-muted focus:bg-muted">
                <Bell className="h-4 w-4" />
                <span className="flex-1">SMS Alerts</span>
              </DropdownMenuCheckboxItem>
            </DropdownMenuSubContent>
          </DropdownMenuPortal>
        </DropdownMenuSub>

        <DropdownMenuSeparator className="my-1" />

        {/* Actions Section */}
        <DropdownMenuLabel className="text-sm text-muted-foreground">
          Actions
        </DropdownMenuLabel>

        <DropdownMenuItem className="flex items-center gap-2 rounded px-2 py-2 text-destructive hover:bg-destructive/10 focus:bg-destructive/10">
          <LogOut className="h-4 w-4" />
          <span className="flex-1">Logout</span>
        </DropdownMenuItem>
      </DropdownMenuContent>
    </DropdownMenu>
  );
}

demo.tsx
"use client";

import AccountMenu from "@/components/ui/account-menu";

export default function AccountMenuDemo() {
  return (
    <div className="min-h-screen flex flex-col items-center justify-center">
      <div className="mb-6">
        <AccountMenu />
      </div>

      <div className="max-w-md text-center text-slate-500 text-sm">
        <p>
          This menu demonstrates a professional dropdown design with clear
          icon-text spacing, nested submenus, and hover effects. Ideal for
          dashboards, team apps, and admin panels.
        </p>
      </div>
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
