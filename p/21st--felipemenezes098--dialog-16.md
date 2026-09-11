<!-- Invite Team Dialog · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/dialog-16
     license: agpl-3.0 · category: team
     A modal dialog with an icon header and email field for inviting teammates to join a workspace. -->

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
components/ui/dialog-16.tsx
import { UsersIcon } from 'lucide-react'

import { Button } from '@/components/ui/button'
import {
  Dialog,
  DialogClose,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from '@/components/ui/dialog'
import { Field, FieldDescription, FieldLabel } from '@/components/ui/field'
import { Input } from '@/components/ui/input'

export function Dialog16() {
  return (
    <Dialog>
      <DialogTrigger render={<Button variant="outline">Invite team</Button>} />
      <DialogContent className="sm:max-w-md">
        <DialogHeader>
          <div className="flex flex-col gap-3">
            <div className="bg-primary/10 text-primary flex size-10 items-center justify-center rounded-full">
              <UsersIcon className="size-5" />
            </div>
            <div className="flex flex-col gap-2">
              <DialogTitle>Invite your team</DialogTitle>
              <DialogDescription>
                Add colleagues by email. They will join as editors on this
                workspace.
              </DialogDescription>
            </div>
          </div>
        </DialogHeader>
        <Field>
          <FieldLabel htmlFor="dialog-16-email">Email address</FieldLabel>
          <Input
            id="dialog-16-email"
            type="email"
            placeholder="colleague@company.com"
          />
          <FieldDescription>
            Separate multiple addresses with commas.
          </FieldDescription>
        </Field>
        <DialogFooter>
          <DialogClose render={<Button variant="outline">Cancel</Button>} />
          <Button>Send invites</Button>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  )
}

demo.tsx
import { UsersIcon } from "lucide-react";

import { Button } from "@/components/ui/dialog-16-utils/button";
import {
  Dialog,
  DialogClose,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from "@/components/ui/dialog-16-utils/dialog";
import {
  Field,
  FieldDescription,
  FieldLabel,
} from "@/components/ui/dialog-16-utils/field";
import { Input } from "@/components/ui/dialog-16-utils/input";

export default function Default() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center p-6">
      <Dialog defaultOpen>
        <DialogTrigger
          render={<Button variant="outline">Invite team</Button>}
        />
        <DialogContent className="sm:max-w-md">
          <DialogHeader>
            <div className="flex flex-col gap-3">
              <div className="bg-primary/10 text-primary flex size-10 items-center justify-center rounded-full">
                <UsersIcon className="size-5" />
              </div>
              <div className="flex flex-col gap-2">
                <DialogTitle>Invite your team</DialogTitle>
                <DialogDescription>
                  Add colleagues by email. They will join as editors on this
                  workspace.
                </DialogDescription>
              </div>
            </div>
          </DialogHeader>
          <Field>
            <FieldLabel htmlFor="dialog-16-email">Email address</FieldLabel>
            <Input
              id="dialog-16-email"
              type="email"
              placeholder="colleague@company.com"
            />
            <FieldDescription>
              Separate multiple addresses with commas.
            </FieldDescription>
          </Field>
          <DialogFooter>
            <DialogClose render={<Button variant="outline">Cancel</Button>} />
            <Button>Send invites</Button>
          </DialogFooter>
        </DialogContent>
      </Dialog>
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
npx shadcn@latest add button dialog field input label separator
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
