<!-- Underline Tabs Card · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-tabs-6
     license: MIT · category: form
     A tabbed settings card with underline-style tabs for switching between account, password, and preference forms. -->

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
components/ui/v-tabs-6.tsx
import { Button } from "@/registry/default/ui/button";
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from "@/registry/default/ui/card";
import { Input } from "@/registry/default/ui/input";
import { Label } from "@/registry/default/ui/label";
import {
  Tabs,
  TabsContent,
  TabsList,
  TabsTrigger,
} from "@/registry/default/ui/tabs";

export function Pattern() {
  return (
    <div className="flex w-full max-w-xs flex-col gap-6">
      <Tabs defaultValue="account">
        <TabsList className="mb-3.5 w-full" variant="underline">
          <TabsTrigger value="account">Account</TabsTrigger>
          <TabsTrigger value="password">Password</TabsTrigger>
          <TabsTrigger value="settings">Settings</TabsTrigger>
        </TabsList>
        <TabsContent value="account">
          <Card>
            <CardHeader className="pb-3">
              <CardTitle className="text-base">Account</CardTitle>
              <CardDescription className="text-sm">
                Update your account information.
              </CardDescription>
            </CardHeader>
            <CardContent className="space-y-4">
              <div className="space-y-2">
                <Label className="text-sm" htmlFor="underline-name">
                  Name
                </Label>
                <Input
                  className="h-9"
                  defaultValue="Alex Chen"
                  id="underline-name"
                />
              </div>
              <div className="space-y-2">
                <Label className="text-sm" htmlFor="underline-email">
                  Email
                </Label>
                <Input
                  className="h-9"
                  defaultValue="alex.chen@example.com"
                  id="underline-email"
                  type="email"
                />
              </div>
            </CardContent>
            <CardFooter className="pt-3">
              <Button size="sm">Save changes</Button>
            </CardFooter>
          </Card>
        </TabsContent>
        <TabsContent value="password">
          <Card>
            <CardHeader className="pb-3">
              <CardTitle className="text-base">Password</CardTitle>
              <CardDescription className="text-sm">
                Change your password here.
              </CardDescription>
            </CardHeader>
            <CardContent className="space-y-4">
              <div className="space-y-2">
                <Label className="text-sm" htmlFor="underline-current">
                  Current password
                </Label>
                <Input className="h-9" id="underline-current" type="password" />
              </div>
              <div className="space-y-2">
                <Label className="text-sm" htmlFor="underline-new">
                  New password
                </Label>
                <Input className="h-9" id="underline-new" type="password" />
              </div>
            </CardContent>
            <CardFooter className="pt-3">
              <Button size="sm">Update password</Button>
            </CardFooter>
          </Card>
        </TabsContent>
        <TabsContent value="settings">
          <Card>
            <CardHeader className="pb-3">
              <CardTitle className="text-base">Settings</CardTitle>
              <CardDescription className="text-sm">
                Manage your preferences.
              </CardDescription>
            </CardHeader>
            <CardContent className="space-y-4">
              <div className="space-y-2">
                <Label className="text-sm" htmlFor="underline-theme">
                  Theme
                </Label>
                <Input
                  className="h-9"
                  defaultValue="Light"
                  id="underline-theme"
                />
              </div>
              <div className="space-y-2">
                <Label className="text-sm" htmlFor="underline-language">
                  Language
                </Label>
                <Input
                  className="h-9"
                  defaultValue="English"
                  id="underline-language"
                />
              </div>
            </CardContent>
            <CardFooter className="pt-3">
              <Button size="sm">Save settings</Button>
            </CardFooter>
          </Card>
        </TabsContent>
      </Tabs>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-tabs-6";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Pattern />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button card input label tabs
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
