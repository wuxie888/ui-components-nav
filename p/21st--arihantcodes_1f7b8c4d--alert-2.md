<!-- Team Invitation Alert · @arihantcodes_1f7b8c4d · https://21st.dev/@arihantcodes_1f7b8c4d/components/alert-2
     license: MIT · category: notification
     A notification alert card showing a team invitation with an avatar, online status, and accept/decline actions. -->

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
components/ui/alerttwo.tsx
import { Alert } from "@/components/ui/alert";
import { Button } from "@/components/ui/button";
import { cn } from "@/lib/utils";
import { Check, X } from "lucide-react";
import Image from "next/image";

export default function Alert02() {
  return (
    <div className="w-full max-w-xl mx-auto">
      <Alert className="relative bg-white dark:bg-zinc-900 border border-zinc-100 dark:border-zinc-800 shadow-[0_1px_6px_0_rgba(0,0,0,0.02)] rounded-xl p-4">
        <div className="flex items-center gap-4">
          <div className="relative h-10 w-10 shrink-0">
            <Image
              src="https://img.freepik.com/premium-photo/anime-male-avatar_950633-956.jpg"
              alt="Arihant jain"
              fill
              className="rounded-full object-cover"
            />
            <div className="absolute bottom-0 right-0 h-2.5 w-2.5 rounded-full bg-green-500 ring-2 ring-white dark:ring-zinc-950" />
          </div>

          <div className="flex-1 min-w-0">
            <div className="flex items-center justify-between gap-4">
              <div>
                <p className="text-sm font-medium text-zinc-700 dark:text-zinc-300">
                  Team Invitation
                </p>
                <p className="text-[13px] text-zinc-500 dark:text-zinc-400 mt-0.5">
                  Arihant invited you to join{" "}
                  <span className="font-medium text-zinc-700 dark:text-zinc-300">
                    Design Team
                  </span>
                </p>
              </div>
            </div>
          </div>

          {/* Actions */}
          <div className="flex items-center gap-2">
            <Button
              size="sm"
              variant="ghost"
              className="h-8 w-8 p-0 hover:bg-red-50 dark:hover:bg-red-950/50 text-zinc-400 hover:text-red-600 dark:text-zinc-500 dark:hover:text-red-400 transition-colors"
              aria-label="Decline invitation"
            >
              <X className="h-4 w-4" />
            </Button>
            <Button
              size="sm"
              variant="ghost"
              className={cn(
                "h-8 w-8 p-0",
                "hover:bg-emerald-50 dark:hover:bg-emerald-950/50",
                "text-zinc-400 hover:text-emerald-600",
                "dark:text-zinc-500 dark:hover:text-emerald-400",
                "transition-colors",
              )}
              aria-label="Accept invitation"
            >
              <Check className="h-4 w-4" />
            </Button>
          </div>
        </div>

        <div className="mt-2 ml-14">
          <p className="text-[12px] text-zinc-400 dark:text-zinc-500">
            Invited 5 minutes ago
          </p>
        </div>
      </Alert>
    </div>
  );
}

demo.tsx
import Alert02 from "@/components/ui/alert-2";

export default function Default() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center p-6">
      <Alert02 />
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
npx shadcn@latest add alert button
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
