<!-- Edit Profile Dialog · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-dialog-9
     license: no-license · category: form
     An edit-profile dialog with an avatar, name, email and bio fields opened from an ellipsis menu button, built on Base UI primitives. -->

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
components/ui/v-dialog-9.tsx
import { EllipsisIcon } from "lucide-react";
import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from "@/registry/default/ui/avatar";
import { Button } from "@/registry/default/ui/button";
import {
  Dialog,
  DialogClose,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogPanel,
  DialogPopup,
  DialogTitle,
  DialogTrigger,
} from "@/registry/default/ui/dialog";
import { Field, FieldLabel } from "@/registry/default/ui/field";
import { Form } from "@/registry/default/ui/form";
import { Input } from "@/registry/default/ui/input";
import { Textarea } from "@/registry/default/ui/textarea";

export default function Component() {
  return (
    <div className="flex items-center gap-3">
      <Avatar className="size-10">
        <AvatarImage alt="Margaret Welsh" src="https://github.com/shadcn.png" />
        <AvatarFallback>MW</AvatarFallback>
      </Avatar>
      <div className="min-w-0 flex-1">
        <p className="font-medium text-sm leading-none">Margaret Welsh</p>
        <p className="mt-0.5 truncate text-muted-foreground text-sm">
          margaret@example.com
        </p>
      </div>
      <Dialog>
        <DialogTrigger
          aria-label="Edit user details"
          render={
            <Button className="size-8 shrink-0" size="icon" variant="ghost" />
          }
        >
          <EllipsisIcon className="size-4" />
        </DialogTrigger>
        <DialogPopup className="sm:max-w-sm">
          <DialogHeader>
            <div className="flex items-center gap-3">
              <Avatar className="size-10">
                <AvatarImage
                  alt="Margaret Welsh"
                  src="https://github.com/shadcn.png"
                />
                <AvatarFallback>MW</AvatarFallback>
              </Avatar>
              <div>
                <DialogTitle>Edit profile</DialogTitle>
                <DialogDescription>
                  Update your personal information.
                </DialogDescription>
              </div>
            </div>
          </DialogHeader>
          <Form className="contents">
            <DialogPanel className="grid gap-4">
              <Field>
                <FieldLabel>Name</FieldLabel>
                <Input defaultValue="Margaret Welsh" type="text" />
              </Field>
              <Field>
                <FieldLabel>Email</FieldLabel>
                <Input defaultValue="margaret@example.com" type="email" />
              </Field>
              <Field>
                <FieldLabel>Bio</FieldLabel>
                <Textarea defaultValue="Product designer based in San Francisco." />
              </Field>
            </DialogPanel>
            <DialogFooter>
              <DialogClose render={<Button variant="ghost" />}>
                Cancel
              </DialogClose>
              <Button type="submit">Save changes</Button>
            </DialogFooter>
          </Form>
        </DialogPopup>
      </Dialog>
    </div>
  );
}

demo.tsx
import Component from "@/components/ui/v-dialog-9";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-6">
      <div className="w-full max-w-sm">
        <Component />
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
npx shadcn@latest add avatar button dialog form input scroll-area textarea
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
