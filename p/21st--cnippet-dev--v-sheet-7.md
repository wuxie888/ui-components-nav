<!-- Team Invite Sheet · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-sheet-7
     license: MIT · category: form
     A slide-out sheet for inviting team members by email with role selection and a live pending-invites list. -->

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
components/ui/v-sheet-7.tsx
"use client";

import { SendIcon, UserPlusIcon, XIcon } from "lucide-react";
import { useState } from "react";
import { Avatar, AvatarFallback } from "@/registry/default/ui/avatar";
import { Button } from "@/registry/default/ui/button";
import { Field, FieldLabel } from "@/registry/default/ui/field";
import { Input } from "@/registry/default/ui/input";
import {
  Sheet,
  SheetClose,
  SheetDescription,
  SheetFooter,
  SheetHeader,
  SheetPanel,
  SheetPopup,
  SheetTitle,
  SheetTrigger,
} from "@/registry/default/ui/sheet";

type Role = "Admin" | "Editor" | "Viewer";
type Invite = { id: number; email: string; role: Role; initials: string };

const roles: Role[] = ["Admin", "Editor", "Viewer"];

const roleBadge: Record<Role, string> = {
  Admin: "bg-rose-100 text-rose-700 dark:bg-rose-950 dark:text-rose-400",
  Editor:
    "bg-violet-100 text-violet-700 dark:bg-violet-950 dark:text-violet-400",
  Viewer: "bg-slate-100 text-slate-700 dark:bg-slate-800 dark:text-slate-300",
};

const pending: Invite[] = [
  { email: "sarah@example.com", id: 1, initials: "SC", role: "Editor" },
  { email: "marcus@example.com", id: 2, initials: "MK", role: "Viewer" },
];

export function Pattern() {
  const [email, setEmail] = useState("");
  const [role, setRole] = useState<Role>("Editor");
  const [invites, setInvites] = useState<Invite[]>(pending);

  const send = () => {
    if (!email.trim()) return;
    const initials = email.slice(0, 2).toUpperCase();
    setInvites((prev) => [
      ...prev,
      { email: email.trim(), id: Date.now(), initials, role },
    ]);
    setEmail("");
  };

  const remove = (id: number) =>
    setInvites((prev) => prev.filter((i) => i.id !== id));

  return (
    <div className="flex items-center justify-center">
      <Sheet>
        <SheetTrigger render={<Button variant="outline" />}>
          <UserPlusIcon aria-hidden="true" />
          Invite Members
        </SheetTrigger>
        <SheetPopup>
          <SheetHeader>
            <SheetTitle>Invite Team Members</SheetTitle>
            <SheetDescription>
              Send invitations to collaborate on this project.
            </SheetDescription>
          </SheetHeader>
          <SheetPanel className="space-y-5">
            <div className="space-y-3">
              <Field>
                <FieldLabel>Email address</FieldLabel>
                <Input
                  onChange={(e) => setEmail(e.target.value)}
                  onKeyDown={(e) => e.key === "Enter" && send()}
                  placeholder="colleague@company.com"
                  type="email"
                  value={email}
                />
              </Field>
              <Field>
                <FieldLabel>Role</FieldLabel>
                <div className="flex gap-2">
                  {roles.map((r) => (
                    <button
                      className={`flex-1 rounded-md border px-3 py-1.5 text-sm transition-colors ${
                        role === r
                          ? "border-primary bg-primary text-primary-foreground"
                          : "hover:bg-muted"
                      }`}
                      key={r}
                      onClick={() => setRole(r)}
                    >
                      {r}
                    </button>
                  ))}
                </div>
              </Field>
              <Button className="w-full" onClick={send}>
                <SendIcon className="size-3.5" />
                Send Invitation
              </Button>
            </div>

            {invites.length > 0 && (
              <div className="space-y-2">
                <p className="font-semibold text-[10px] text-muted-foreground uppercase tracking-wider">
                  Pending ({invites.length})
                </p>
                <div className="space-y-2">
                  {invites.map((invite) => (
                    <div
                      className="flex items-center gap-3 rounded-lg border p-2.5"
                      key={invite.id}
                    >
                      <Avatar className="size-8">
                        <AvatarFallback className="text-xs">
                          {invite.initials}
                        </AvatarFallback>
                      </Avatar>
                      <div className="min-w-0 flex-1">
                        <p className="truncate text-sm">{invite.email}</p>
                      </div>
                      <span
                        className={`rounded px-1.5 py-0.5 font-medium text-[10px] ${roleBadge[invite.role]}`}
                      >
                        {invite.role}
                      </span>
                      <Button
                        aria-label="Remove invite"
                        className="size-6"
                        onClick={() => remove(invite.id)}
                        size="icon"
                        variant="ghost"
                      >
                        <XIcon className="size-3" />
                      </Button>
                    </div>
                  ))}
                </div>
              </div>
            )}
          </SheetPanel>
          <SheetFooter>
            <SheetClose render={<Button variant="ghost" />}>Cancel</SheetClose>
            <Button>Done</Button>
          </SheetFooter>
        </SheetPopup>
      </Sheet>
    </div>
  );
}

demo.tsx
import Component from "@/components/ui/v-sheet-7";

export default function Default() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center p-6">
      <Component />
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
npx shadcn@latest add avatar avatar?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068&publisher_install_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdXJwb3NlIjoicHVibGlzaGVyLXJlZ2lzdHJ5LWluc3RhbGwiLCJpYXQiOjE3ODg0NzA1ODMsImV4cCI6MTc4ODQ3MTE4M30.WGlCfx6HzttNPU-0Qxzw5l4RGwJCritkmKkixYLhyQc button button?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068&publisher_install_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdXJwb3NlIjoicHVibGlzaGVyLXJlZ2lzdHJ5LWluc3RhbGwiLCJpYXQiOjE3ODg0NzA1ODMsImV4cCI6MTc4ODQ3MTE4M30.WGlCfx6HzttNPU-0Qxzw5l4RGwJCritkmKkixYLhyQc field field?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068&publisher_install_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdXJwb3NlIjoicHVibGlzaGVyLXJlZ2lzdHJ5LWluc3RhbGwiLCJpYXQiOjE3ODg0NzA1ODMsImV4cCI6MTc4ODQ3MTE4M30.WGlCfx6HzttNPU-0Qxzw5l4RGwJCritkmKkixYLhyQc input scroll-area sheet
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
