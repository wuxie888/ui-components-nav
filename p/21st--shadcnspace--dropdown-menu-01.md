<!-- User Profile Dropdown Menu · @shadcnspace · https://21st.dev/@shadcnspace/components/dropdown-menu-01
     license: no-license · category: dropdown
     A user profile dropdown menu with avatar, online status indicator, and grouped links for profile, subscription, settings, and sign out. -->

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
components/shadcn-space/dropdown-menu/dropdown-menu-01.tsx
"use client";

import type { ReactElement } from "react";
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuGroup,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuSeparator,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";
import {
  LucideIcon,
  CircleUserRound,
  CreditCard,
  ReceiptText,
  Settings,
  LogOut,
} from "lucide-react";

type Props = {
  trigger: ReactElement;
  defaultOpen?: boolean;
  align?: "start" | "center" | "end";
};

type MenuItem = {
  label: string;
  icon: LucideIcon;
  destructive?: boolean;
};

const PROFILE_ITEMS: MenuItem[] = [
  { label: "My Profile", icon: CircleUserRound },
  { label: "My Subscription", icon: CreditCard },
  { label: "My Invoice", icon: ReceiptText },
];

const SETTINGS_ITEMS: MenuItem[] = [
  { label: "Account Settings", icon: Settings },
];

const LOGOUT_ITEM: MenuItem = {
  label: "Signout",
  icon: LogOut,
  destructive: true,
};

const itemClass =
  "p-2 text-sm font-medium text-popover-foreground cursor-pointer gap-2";

const Dropdown = ({ trigger, defaultOpen, align = "end" }: Props) => {
  return (
    <div className="flex items-start justify-center p-4 sm:p-8">
      <DropdownMenu defaultOpen={defaultOpen}>
        <DropdownMenuTrigger className="cursor-pointer">
          {trigger}
        </DropdownMenuTrigger>

        <DropdownMenuContent
          align={align}
          className="w-3xs rounded-2xl data-open:slide-in-from-bottom-20! data-closed:slide-out-to-bottom-20 data-open:fade-in-0 data-closed:fade-out-0 data-closed:zoom-out-100 duration-400"
        >
          <DropdownMenuGroup>
            {/* User Info */}
            <DropdownMenuLabel className="flex items-center gap-3 px-4 py-3">
              <div className="relative">
                <Avatar className="size-10">
                  <AvatarImage
                    src="https://images.shadcnspace.com/assets/profiles/user-11.jpg"
                    alt="David McMichael"
                  />
                  <AvatarFallback>DM</AvatarFallback>
                </Avatar>
                <span className="ring-card absolute right-0 bottom-0 size-2 rounded-full bg-green-600 ring-2" />
              </div>

              <div className="flex flex-col">
                <span className="text-popover-foreground text-sm font-medium">
                  David McMichael
                </span>
                <span className="text-muted-foreground text-sm">
                  david@shadcnspace.com
                </span>
              </div>
            </DropdownMenuLabel>

            <DropdownMenuSeparator />

            {/* Main Links */}
            {PROFILE_ITEMS.map(({ label, icon: Icon }) => (
              <DropdownMenuItem key={label} className={itemClass}>
                <Icon size={20} />
                <span>{label}</span>
              </DropdownMenuItem>
            ))}

            <DropdownMenuSeparator />

            {/* Settings */}
            <DropdownMenuGroup>
              {SETTINGS_ITEMS.map(({ label, icon: Icon }) => (
                <DropdownMenuItem key={label} className={itemClass}>
                  <Icon size={20} />
                  <span>{label}</span>
                </DropdownMenuItem>
              ))}
            </DropdownMenuGroup>

            <DropdownMenuSeparator />

            {/* Logout */}
            <DropdownMenuItem variant="destructive" className={itemClass}>
              <LOGOUT_ITEM.icon size={20} />
              <span>{LOGOUT_ITEM.label}</span>
            </DropdownMenuItem>
          </DropdownMenuGroup>
        </DropdownMenuContent>
      </DropdownMenu>
    </div>
  );
};

const DropdownMenu01 = () => {
  return (
    <Dropdown
      align="center"
      trigger={
        <div className="rounded-full">
          <Avatar className="size-10 cursor-pointer">
            <AvatarImage
              src="https://images.shadcnspace.com/assets/profiles/user-11.jpg"
              alt="David McMichael"
            />
            <AvatarFallback>DM</AvatarFallback>
          </Avatar>
        </div>
      }
    />
  );
};

export default DropdownMenu01;

demo.tsx
import DropdownMenu01 from "@/components/ui/dropdown-menu-01";

export default function Default() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center">
      <DropdownMenu01 />
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
npx shadcn@latest add avatar dropdown-menu
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
