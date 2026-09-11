<!-- Breadcrumb with Project, User & Document Info · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-breadcrumb-10
     license: MIT · category: navigation-menu
     A breadcrumb navigation that shows an organization logo, a user avatar with name and email, and the current document with an icon. -->

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
components/ui/v-breadcrumb-10.tsx
import { FileTextIcon } from "lucide-react";
import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from "@/registry/default/ui/avatar";
import {
  Breadcrumb,
  BreadcrumbItem,
  BreadcrumbLink,
  BreadcrumbList,
  BreadcrumbPage,
  BreadcrumbSeparator,
} from "@/registry/default/ui/breadcrumb";
import { Frame, FramePanel } from "@/registry/default/ui/frame";

export function Pattern() {
  return (
    <Frame>
      <FramePanel>
        <Breadcrumb>
          <BreadcrumbList className="gap-3">
            <BreadcrumbItem>
              <BreadcrumbLink
                className="flex items-center gap-2 text-foreground"
                href="#"
              >
                <Avatar className="size-6">
                  <AvatarImage src="https://github.com/vercel.png" />
                  <AvatarFallback>VC</AvatarFallback>
                </Avatar>
              </BreadcrumbLink>
            </BreadcrumbItem>

            <BreadcrumbSeparator className="text-muted-foreground/60">
              /
            </BreadcrumbSeparator>

            <BreadcrumbItem>
              <BreadcrumbLink className="flex items-center gap-3" href="#">
                <Avatar className="size-6">
                  <AvatarImage
                    className="object-cover"
                    src="https://github.com/shadcn.png"
                  />
                  <AvatarFallback>MP</AvatarFallback>
                </Avatar>
                <div className="flex flex-col">
                  <span className="font-medium text-foreground leading-tight">
                    shadcn
                  </span>
                  <span className="text-muted-foreground leading-tight">
                    ui@shadcn.com
                  </span>
                </div>
              </BreadcrumbLink>
            </BreadcrumbItem>

            <BreadcrumbSeparator className="text-muted-foreground/60">
              /
            </BreadcrumbSeparator>

            <BreadcrumbItem>
              <BreadcrumbPage className="flex items-center gap-2.5">
                <span className="flex size-6 items-center justify-center rounded-md bg-sky-100 text-sky-500 dark:bg-sky-500/10 dark:text-sky-400">
                  <FileTextIcon className="size-3.5" />
                </span>
                <div className="flex flex-col">
                  <span className="font-medium text-foreground leading-tight">
                    Document
                  </span>
                  <span className="flex items-center gap-1 text-muted-foreground leading-tight">
                    agents.md
                  </span>
                </div>
              </BreadcrumbPage>
            </BreadcrumbItem>
          </BreadcrumbList>
        </Breadcrumb>
      </FramePanel>
    </Frame>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-breadcrumb-10";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-8">
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
npx shadcn@latest add avatar breadcrumb
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
