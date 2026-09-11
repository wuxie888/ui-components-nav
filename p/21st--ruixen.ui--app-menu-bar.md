<!-- App Menu Bar · @ruixen.ui · https://21st.dev/@ruixen.ui/components/app-menu-bar
     license: unspecified · category: dashboard
     The AppMenuBar component is a versatile, fully-featured navigation menu designed for modern web applications. It combines a clean, horizontal menubar layout with intuitive nested menus, allowing users to easily access projects, teams, calendar events, notifications, and files. Each menu item is enhanced with clear Lucide icons, and nested submenus are implemented using MenubarSub, ensuring that hover interactions are smooth and the parent menu stays open when navigating into submenus. The component supports multiple levels of navigation without closing unexpectedly, maintains consistent spacing between icons and text, and leverages subtle hover effects for better visual feedback. Its modular structure makes it highly customizable, making it ideal for dashboards, admin panels, or collaborative tools where organized and accessible -->

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
components/ui/app-menu-bar.tsx
"use client";

import {
  Menubar,
  MenubarContent,
  MenubarItem,
  MenubarMenu,
  MenubarSeparator,
  MenubarTrigger,
  MenubarSub,
  MenubarSubTrigger,
  MenubarSubContent,
} from "@/components/ui/menubar";
import {
  Folder,
  FolderPlus,
  FileText,
  Users,
  UserCheck,
  Settings,
  Bell,
  Calendar,
  ChevronRight,
  CheckCircle,
  Upload,
} from "lucide-react";

export default function AppMenuBar() {
  return (
    <Menubar className="bg-background w-fit border-b border-slate-200 shadow-sm rounded-b-lg">
      {/* Projects Menu */}
      <MenubarMenu>
        <MenubarTrigger className="flex items-center gap-2">
          <Folder className="w-4 h-4" />
          Projects
        </MenubarTrigger>
        <MenubarContent className="w-56">
          <MenubarItem className="flex items-center gap-2">
            <FolderPlus className="w-4 h-4" />
            New Project
          </MenubarItem>
          <MenubarItem className="flex items-center gap-2">
            <FileText className="w-4 h-4" />
            All Projects
          </MenubarItem>
          <MenubarItem className="flex items-center gap-2">
            <CheckCircle className="w-4 h-4" />
            Completed Projects
          </MenubarItem>
        </MenubarContent>
      </MenubarMenu>

      {/* Teams Menu with Submenu */}
      <MenubarMenu>
        <MenubarTrigger className="flex items-center gap-2">
          <Users className="w-4 h-4" />
          Teams
        </MenubarTrigger>
        <MenubarContent className="w-56">
          <MenubarItem className="flex items-center gap-2">
            <UserCheck className="w-4 h-4" />
            All Members
          </MenubarItem>
          <MenubarItem className="flex items-center gap-2">
            <FolderPlus className="w-4 h-4" />
            Create Team
          </MenubarItem>

          <MenubarSeparator />

          {/* Proper Nested Submenu */}
          <MenubarSub>
            <MenubarSubTrigger className="flex items-center justify-between gap-2">
              Manage Teams
            </MenubarSubTrigger>
            <MenubarSubContent className="w-48">
              <MenubarItem className="flex items-center gap-2">
                <Settings className="w-4 h-4" />
                Team Settings
              </MenubarItem>
              <MenubarItem className="flex items-center gap-2">
                <Users className="w-4 h-4" />
                Team Members
              </MenubarItem>
            </MenubarSubContent>
          </MenubarSub>
        </MenubarContent>
      </MenubarMenu>

      {/* Calendar Menu */}
      <MenubarMenu>
        <MenubarTrigger className="flex items-center gap-2">
          <Calendar className="w-4 h-4" />
          Calendar
        </MenubarTrigger>
        <MenubarContent className="w-48">
          <MenubarItem className="flex items-center gap-2">
            <Calendar className="w-4 h-4" />
            View Calendar
          </MenubarItem>
          <MenubarItem className="flex items-center gap-2">
            <FileText className="w-4 h-4" />
            Schedule Task
          </MenubarItem>
        </MenubarContent>
      </MenubarMenu>

      {/* Notifications */}
      <MenubarMenu>
        <MenubarTrigger className="flex items-center gap-2">
          <Bell className="w-4 h-4" />
          Notifications
        </MenubarTrigger>
        <MenubarContent className="w-48">
          <MenubarItem className="flex items-center gap-2">
            <Bell className="w-4 h-4" />
            All Notifications
          </MenubarItem>
        </MenubarContent>
      </MenubarMenu>

      {/* Files Menu */}
      <MenubarMenu>
        <MenubarTrigger className="flex items-center gap-2">
          <Upload className="w-4 h-4" />
          Files
        </MenubarTrigger>
        <MenubarContent className="w-56">
          <MenubarItem className="flex items-center gap-2">
            <Upload className="w-4 h-4" />
            Upload File
          </MenubarItem>
          <MenubarItem className="flex items-center gap-2">
            <FileText className="w-4 h-4" />
            My Files
          </MenubarItem>
        </MenubarContent>
      </MenubarMenu>
    </Menubar>
  );
}

demo.tsx
"use client";

import AppMenuBar from "@/components/ui/app-menu-bar";

export default function MenuDemoPage() {
  return (
    <div className="p-6">
      <AppMenuBar />
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
npx shadcn@latest add menubar
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
