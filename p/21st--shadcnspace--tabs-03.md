<!-- Tabs With Icons · @shadcnspace · https://21st.dev/@shadcnspace/components/tabs-03
     license: MIT · category: navigation-menu
     A tabbed interface with an icon beside each tab label for switching between account settings sections like details, profile, password, and team. -->

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
components/shadcn-space/tabs/tabs-03.tsx
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs";
import {
  LockKeyhole,
  LucideIcon,
  NotebookTabs,
  UserRoundPen,
  Users,
} from "lucide-react";

type TabsWithIconProps = {
  tabs: {
    name: string;
    value: string;
    icon: LucideIcon;
    content: React.ReactNode;
  }[];
};

const tabs: TabsWithIconProps["tabs"] = [
  {
    name: "My details",
    value: "my-details",
    icon: NotebookTabs,
    content: (
      <>
        Manage your personal{" "}
        <span className="text-foreground font-semibold">account details</span> .
        Keep everything up to date so we can serve you better.
      </>
    ),
  },
  {
    name: "Profile",
    value: "profile",
    icon: UserRoundPen,
    content: (
      <>
        Customize how others see you. Update your{" "}
        <span className="text-foreground font-semibold">
          profile information
        </span>
        , bio, and preferences to reflect who you are.
      </>
    ),
  },
  {
    name: "Password",
    value: "password",
    icon: LockKeyhole,
    content: (
      <>
        Keep your account{" "}
        <span className="text-foreground font-semibold">secure</span> by
        updating your password regularly. Choose a strong password to protect
        your data.
      </>
    ),
  },
  {
    name: "Team",
    value: "team",
    icon: Users,
    content: (
      <>
        Manage your{" "}
        <span className="text-foreground font-semibold">team members</span>,
        invite new collaborators, assign roles, and control access permissions
        from one place.
      </>
    ),
  },
];

const TabsWithIconDemo = () => {
  return (
    <div className="w-full max-w-md">
      <Tabs defaultValue={tabs[0].value} className="gap-4">
        <TabsList>
          {tabs.map(({ icon: Icon, name, value }) => (
            <TabsTrigger
              key={value}
              value={value}
              className="flex items-center gap-1 px-2.5 sm:px-3"
            >
              <Icon />
              {name}
            </TabsTrigger>
          ))}
        </TabsList>

        {tabs.map((tab) => (
          <TabsContent key={tab.value} value={tab.value}>
            <p className="text-muted-foreground text-sm">{tab.content}</p>
          </TabsContent>
        ))}
      </Tabs>
    </div>
  );
};

export default TabsWithIconDemo;

demo.tsx
import TabsWithIconDemo from "@/components/ui/tabs-03";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-8">
      <TabsWithIconDemo />
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
