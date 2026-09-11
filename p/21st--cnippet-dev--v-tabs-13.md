<!-- Vertical Settings Tabs · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-tabs-13
     license: MIT · category: navigation-menu
     A settings panel with vertical underline tabs for switching between profile, security, notifications, and integrations sections. -->

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
components/ui/v-tabs-13.tsx
import { BellIcon, GlobeIcon, LockIcon, UserIcon } from "lucide-react";
import { Separator } from "@/registry/default/ui/separator";
import { Tabs, TabsList, TabsPanel, TabsTab } from "@/registry/default/ui/tabs";

export function Pattern() {
  return (
    <div className="mx-auto w-full max-w-lg">
      <Tabs className="gap-6" defaultValue="profile" orientation="vertical">
        <TabsList className="w-44 shrink-0" variant="underline">
          <TabsTab className="justify-start gap-2.5" value="profile">
            <UserIcon className="size-4" />
            Profile
          </TabsTab>
          <TabsTab className="justify-start gap-2.5" value="security">
            <LockIcon className="size-4" />
            Security
          </TabsTab>
          <TabsTab className="justify-start gap-2.5" value="notifications">
            <BellIcon className="size-4" />
            Notifications
          </TabsTab>
          <TabsTab className="justify-start gap-2.5" value="integrations">
            <GlobeIcon className="size-4" />
            Integrations
          </TabsTab>
        </TabsList>

        <TabsPanel value="profile">
          <div className="flex flex-col gap-4">
            <div>
              <p className="font-semibold text-sm">Profile Settings</p>
              <p className="mt-0.5 text-muted-foreground text-xs">
                Manage your public profile and personal details.
              </p>
            </div>
            <Separator />
            <div className="flex flex-col gap-3">
              <div className="grid grid-cols-2 gap-3">
                <div>
                  <p className="mb-1 font-medium text-xs">First name</p>
                  <div className="rounded-md border border-input bg-muted/40 px-3 py-2 text-sm">
                    Olivia
                  </div>
                </div>
                <div>
                  <p className="mb-1 font-medium text-xs">Last name</p>
                  <div className="rounded-md border border-input bg-muted/40 px-3 py-2 text-sm">
                    Martin
                  </div>
                </div>
              </div>
              <div>
                <p className="mb-1 font-medium text-xs">Email</p>
                <div className="rounded-md border border-input bg-muted/40 px-3 py-2 text-muted-foreground text-sm">
                  olivia@example.com
                </div>
              </div>
              <div>
                <p className="mb-1 font-medium text-xs">Bio</p>
                <div className="min-h-16 rounded-md border border-input bg-muted/40 px-3 py-2 text-muted-foreground text-sm">
                  Software engineer & open source contributor.
                </div>
              </div>
            </div>
          </div>
        </TabsPanel>

        <TabsPanel value="security">
          <div className="flex flex-col gap-4">
            <div>
              <p className="font-semibold text-sm">Security</p>
              <p className="mt-0.5 text-muted-foreground text-xs">
                Manage your password and two-factor authentication.
              </p>
            </div>
            <Separator />
            <div className="flex flex-col gap-3">
              <div>
                <p className="mb-1 font-medium text-xs">Current password</p>
                <div className="rounded-md border border-input bg-muted/40 px-3 py-2 text-muted-foreground text-sm">
                  ••••••••
                </div>
              </div>
              <div>
                <p className="mb-1 font-medium text-xs">Two-factor auth</p>
                <div className="flex items-center justify-between rounded-md border border-input bg-muted/40 px-3 py-2">
                  <span className="text-muted-foreground text-sm">
                    Authenticator app
                  </span>
                  <span className="font-medium text-emerald-500 text-xs">
                    Enabled
                  </span>
                </div>
              </div>
            </div>
          </div>
        </TabsPanel>

        <TabsPanel value="notifications">
          <div className="flex flex-col gap-4">
            <div>
              <p className="font-semibold text-sm">Notification Preferences</p>
              <p className="mt-0.5 text-muted-foreground text-xs">
                Choose how and when you receive notifications.
              </p>
            </div>
            <Separator />
            <div className="flex flex-col gap-2">
              {[
                "New comments on my posts",
                "Mentions and replies",
                "Weekly digest email",
                "Security alerts",
              ].map((item) => (
                <div
                  className="flex items-center justify-between rounded-md border border-input px-3 py-2"
                  key={item}
                >
                  <span className="text-sm">{item}</span>
                  <div className="h-4 w-8 rounded-full bg-primary" />
                </div>
              ))}
            </div>
          </div>
        </TabsPanel>

        <TabsPanel value="integrations">
          <div className="flex flex-col gap-4">
            <div>
              <p className="font-semibold text-sm">Integrations</p>
              <p className="mt-0.5 text-muted-foreground text-xs">
                Connect third-party tools and services to your account.
              </p>
            </div>
            <Separator />
            <div className="flex flex-col gap-2">
              {[
                { connected: true, name: "GitHub" },
                { connected: true, name: "Slack" },
                { connected: false, name: "Jira" },
                { connected: false, name: "Figma" },
              ].map((i) => (
                <div
                  className="flex items-center justify-between rounded-md border border-input px-3 py-2"
                  key={i.name}
                >
                  <span className="text-sm">{i.name}</span>
                  <span
                    className={`text-xs ${i.connected ? "text-emerald-500" : "text-muted-foreground"}`}
                  >
                    {i.connected ? "Connected" : "Not connected"}
                  </span>
                </div>
              ))}
            </div>
          </div>
        </TabsPanel>
      </Tabs>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-tabs-13";

export default function Default() {
  return (
    <div className="flex w-full items-center justify-center p-6">
      <Pattern />
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
npx shadcn@latest add separator tabs
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
