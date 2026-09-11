<!-- Message · @shadcn · https://21st.dev/@shadcn/components/message
     license: MIT · category: avatar
     Layout wrapper that arranges a single conversation message with avatar, alignment, header, and footer for chat and AI messaging interfaces. -->

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
components/ui/message.tsx
import * as React from "react"
import { cn } from "cn"

function MessageGroup({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="message-group"
      className={cn("flex min-w-0 flex-col gap-2", className)}
      {...props}
    />
  )
}

function Message({
  className,
  align = "start",
  ...props
}: React.ComponentProps<"div"> & { align?: "start" | "end" }) {
  return (
    <div
      data-slot="message"
      data-align={align}
      className={cn(
        "group/message relative flex w-full min-w-0 gap-2 text-sm data-[align=end]:flex-row-reverse",
        className
      )}
      {...props}
    />
  )
}

function MessageAvatar({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="message-avatar"
      className={cn(
        "flex w-fit min-w-8 shrink-0 items-center justify-center self-end overflow-hidden rounded-full bg-muted group-has-data-[slot=message-footer]/message:-translate-y-8",
        className
      )}
      {...props}
    />
  )
}

function MessageContent({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="message-content"
      className={cn(
        "flex w-full min-w-0 flex-col gap-2.5 wrap-break-word group-data-[align=end]/message:*:data-slot:self-end",
        className
      )}
      {...props}
    />
  )
}

function MessageHeader({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="message-header"
      className={cn(
        "flex max-w-full min-w-0 items-center px-3 text-xs font-medium text-muted-foreground group-has-data-[variant=ghost]/message:px-0",
        className
      )}
      {...props}
    />
  )
}

function MessageFooter({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="message-footer"
      className={cn(
        "flex max-w-full min-w-0 items-center px-3 text-xs font-medium text-muted-foreground group-has-data-[variant=ghost]/message:px-0 group-data-[align=end]/message:justify-end",
        className
      )}
      {...props}
    />
  )
}

export {
  MessageGroup,
  Message,
  MessageAvatar,
  MessageContent,
  MessageFooter,
  MessageHeader,
}

demo.tsx
import {
  Message,
  MessageAvatar,
  MessageContent,
  MessageFooter,
  MessageHeader,
} from "@/components/ui/message"
import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from "@/components/ui/avatar"

export default function MessageDemo() {
  return (
    <div className="mx-auto flex w-full max-w-md flex-col gap-8 p-6">
      <Message>
        <MessageAvatar className="size-8">
          <Avatar className="size-8">
            <AvatarImage src="https://cdn.21st.dev/assets/mirror/51/513d06f542692036ff0bb17938f04b3efd60567ba2cae08049469d21f18e0524.jpg" alt="@shadcn" />
            <AvatarFallback>CN</AvatarFallback>
          </Avatar>
        </MessageAvatar>
        <MessageContent className="ml-2 gap-1">
          <MessageHeader className="text-sm font-medium">shadcn</MessageHeader>
          <div className="w-fit rounded-2xl rounded-bl-md bg-muted px-4 py-2 text-sm text-foreground">
            Deploying to prod real quick.
          </div>
        </MessageContent>
      </Message>

      <Message align="end">
        <MessageContent className="mr-2 items-end gap-1">
          <div className="w-fit rounded-2xl rounded-br-md bg-primary px-4 py-2 text-sm text-primary-foreground">
            It&apos;s 4:55 PM. On a Friday.
          </div>
          <MessageFooter className="text-xs text-muted-foreground">
            Delivered
          </MessageFooter>
        </MessageContent>
      </Message>

      <Message>
        <MessageAvatar className="size-8">
          <Avatar className="size-8">
            <AvatarImage
              src="https://cdn.21st.dev/assets/mirror/ef/ef407ec862dd072b8624fad7a331df80f56694e98bc1d89b0feb0ea0185e8954.png"
              alt="@evilrabbit"
            />
            <AvatarFallback>ER</AvatarFallback>
          </Avatar>
        </MessageAvatar>
        <MessageContent className="ml-2 gap-1">
          <MessageHeader className="text-sm font-medium">evilrabbit</MessageHeader>
          <div className="w-fit rounded-2xl rounded-bl-md bg-muted px-4 py-2 text-sm text-foreground">
            It&apos;s always a one-line change 😭.
          </div>
        </MessageContent>
      </Message>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install cn
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar
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
