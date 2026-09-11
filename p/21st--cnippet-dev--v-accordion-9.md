<!-- User List Accordion · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-accordion-9
     license: MIT · category: team
     An accordion listing team members with avatars, names, and role badges that expand to reveal each member's permission details. -->

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
components/ui/v-accordion-9.tsx
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/registry/default/ui/accordion";
import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from "@/registry/default/ui/avatar";
import { Badge } from "@/registry/default/ui/badge";
import { Frame, FramePanel } from "@/registry/default/ui/frame";

const users = [
  {
    avatar:
      "https://images.unsplash.com/photo-1535713875002-d1d0cf377fde?w=96&h=96&dpr=2&q=80",
    content:
      "Alex has full administrative access to the platform, including billing management, user provisioning, and security configurations.",
    email: "alex@apple.com",
    id: "1",
    initials: "AJ",
    name: "Alex Johnson",
    role: "Admin",
  },
  {
    avatar:
      "https://images.unsplash.com/photo-1519699047748-de8e457a634e?w=96&h=96&dpr=2&q=80",
    content:
      "Sarah has read-only access to projects and reports. She cannot modify settings or invite new members.",
    email: "sarah@openai.com",
    id: "2",
    initials: "SC",
    name: "Sarah Chen",
    role: "Viewer",
  },
  {
    avatar:
      "https://images.unsplash.com/photo-1584308972272-9e4e7685e80f?w=96&h=96&dpr=2&q=80",
    content:
      "Michael is part of the design team and has permissions to edit projects, manage assets, and update design system components.",
    email: "michael@meta.com",
    id: "3",
    initials: "MR",
    name: "Michael Rodriguez",
    role: "Editor",
  },
];

export function Pattern() {
  return (
    <div className="mx-auto mb-auto w-full max-w-lg">
      <Frame>
        {users.map((user) => (
          <FramePanel key={user.id}>
            <Accordion
              className="border-none"
              defaultValue={["1"]}
              multiple={false}
            >
              <AccordionItem
                className="border-none bg-transparent p-0 **:data-[slot=accordion-content]:p-0!"
                value={user.id}
              >
                <AccordionTrigger className="items-center px-1 py-1 hover:no-underline">
                  <div className="flex items-center gap-2">
                    <Avatar className="size-8 border">
                      <AvatarImage alt={user.name} src={user.avatar} />
                      <AvatarFallback className="text-xs">
                        {user.initials}
                      </AvatarFallback>
                    </Avatar>
                    <div className="inline-flex items-center gap-2">
                      <span className="font-semibold text-foreground/90 tracking-tight">
                        {user.name}
                      </span>
                      <Badge
                        size="sm"
                        variant={
                          user.role === "Admin" ? "success" : "secondary"
                        }
                      >
                        {user.role}
                      </Badge>
                    </div>
                  </div>
                </AccordionTrigger>
                <AccordionContent className="py-0 pl-11 text-muted-foreground">
                  {user.content}
                </AccordionContent>
              </AccordionItem>
            </Accordion>
          </FramePanel>
        ))}
      </Frame>
    </div>
  );
}

demo.tsx
import Component from "@/components/ui/v-accordion-9";

export default function Default() {
  return (
    <div className="flex w-full items-center justify-center p-8">
      <Component />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add accordion avatar badge
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
