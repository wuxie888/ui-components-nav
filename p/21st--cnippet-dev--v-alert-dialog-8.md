<!-- System Update Notification Alert Dialog · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-alert-dialog-8
     license: MIT · category: modal
     An alert dialog that notifies users about an available system update, showing the release version badge and "Update Now" / "Remind Me Later" actions. -->

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
components/ui/v-alert-dialog-8.tsx
import { ShieldAlertIcon } from "lucide-react";
import {
  AlertDialog,
  AlertDialogClose,
  AlertDialogContent,
  AlertDialogDescription,
  AlertDialogFooter,
  AlertDialogTitle,
  AlertDialogTrigger,
} from "@/registry/default/ui/alert-dialog";
import { Badge } from "@/registry/default/ui/badge";
import { Button } from "@/registry/default/ui/button";

export function Pattern() {
  return (
    <AlertDialog>
      <AlertDialogTrigger
        render={<Button variant="outline">System Update</Button>}
      />
      <AlertDialogContent className="gap-0 p-0 sm:max-w-sm">
        <div className="mx-auto flex flex-col items-center justify-center gap-2 p-8">
          <div className="flex size-12 items-center justify-center rounded-full bg-info/10 text-info dark:bg-info/20">
            <ShieldAlertIcon className="size-6" />
          </div>
          <AlertDialogTitle className="text-center">
            System Update Available!
          </AlertDialogTitle>
          <Badge variant="success">Release v28.1.0 (2026-01-12)</Badge>
        </div>

        <div className="flex flex-col items-center justify-center gap-5 rounded-b-2xl bg-muted/60 pt-6">
          <AlertDialogDescription className="px-6 text-center text-muted-foreground">
            A new version of the application is ready. Restarting now will apply
            the latest security patches and features.
          </AlertDialogDescription>
          <AlertDialogFooter className="flex w-full gap-4 sm:items-center sm:justify-center">
            <AlertDialogClose render={<Button variant="ghost" />}>
              Remind Me Later
            </AlertDialogClose>
            <AlertDialogClose render={<Button />}>Update Now</AlertDialogClose>
          </AlertDialogFooter>
        </div>
      </AlertDialogContent>
    </AlertDialog>
  );
}

demo.tsx
import { ShieldAlertIcon } from "lucide-react";
import {
  AlertDialog,
  AlertDialogClose,
  AlertDialogContent,
  AlertDialogDescription,
  AlertDialogFooter,
  AlertDialogTitle,
  AlertDialogTrigger,
} from "@/components/ui/v-alert-dialog-8-utils/alert-dialog";
import { Badge } from "@/components/ui/v-alert-dialog-8-utils/badge";
import { Button } from "@/components/ui/v-alert-dialog-8-utils/button";

export default function Default() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center p-6">
      <AlertDialog defaultOpen>
        <AlertDialogTrigger
          render={<Button variant="outline">System Update</Button>}
        />
        <AlertDialogContent className="gap-0 p-0 sm:max-w-md">
          <div className="mx-auto flex flex-col items-center justify-center gap-2 p-6">
            <div className="flex size-12 items-center justify-center rounded-full bg-info/10 text-info dark:bg-info/20">
              <ShieldAlertIcon className="size-6" />
            </div>
            <AlertDialogTitle className="text-center">
              System Update Available!
            </AlertDialogTitle>
            <Badge variant="success">Release v28.1.0 (2026-01-12)</Badge>
          </div>

          <div className="flex flex-col items-center justify-center gap-4 rounded-b-2xl bg-muted/60 pt-5">
            <AlertDialogDescription className="px-6 text-center text-muted-foreground">
              A new version of the application is ready. Restarting now will
              apply the latest security patches and features.
            </AlertDialogDescription>
            <AlertDialogFooter className="flex w-full gap-4 sm:items-center sm:justify-center">
              <AlertDialogClose render={<Button variant="ghost" />}>
                Remind Me Later
              </AlertDialogClose>
              <AlertDialogClose render={<Button />}>
                Update Now
              </AlertDialogClose>
            </AlertDialogFooter>
          </div>
        </AlertDialogContent>
      </AlertDialog>
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
npx shadcn@latest add alert-dialog badge button
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
