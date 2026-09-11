<!-- Chat Scroll Area · @bundui · https://21st.dev/@bundui/components/scroll-area6
     license: no-license · category: scroll-area
     A scrollable chat conversation panel with avatars, message bubbles, timestamps and a message input that auto-scrolls to the latest message. -->

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
components/ui/index.tsx
"use client";

import { useState, useEffect, useRef } from "react";
import { ScrollArea } from "@/components/ui/scroll-area";
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";
import { Bubble, BubbleContent } from "@/components/ui/bubble";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { cn } from "@/lib/utils";
import { SendIcon } from "lucide-react";
import { format, parseISO } from "date-fns";

const initialMessages = [
  {
    id: 1,
    sender: "Sarah Miller",
    avatar: "https://i.pravatar.cc/150?img=2",
    message: "Hey! Are we still on for the meeting tomorrow?",
    timestamp: "2024-01-15T09:15:00",
    isOwn: false,
  },
  {
    id: 2,
    sender: "You",
    avatar: "https://i.pravatar.cc/150?img=1",
    message: "Yes, absolutely! 2 PM works for me.",
    timestamp: "2024-01-15T09:18:00",
    isOwn: true,
  },
  {
    id: 3,
    sender: "Sarah Miller",
    avatar: "https://i.pravatar.cc/150?img=2",
    message: "Perfect! I'll send the agenda later today.",
    timestamp: "2024-01-15T09:20:00",
    isOwn: false,
  },
  {
    id: 4,
    sender: "Mike Johnson",
    avatar: "https://i.pravatar.cc/150?img=3",
    message: "Can I join the meeting as well?",
    timestamp: "2024-01-15T10:30:00",
    isOwn: false,
  },
  {
    id: 5,
    sender: "You",
    avatar: "https://i.pravatar.cc/150?img=1",
    message: "Of course! The more the merrier.",
    timestamp: "2024-01-15T10:32:00",
    isOwn: true,
  },
  {
    id: 6,
    sender: "Emma Wilson",
    avatar: "https://i.pravatar.cc/150?img=4",
    message: "I've prepared the presentation slides. Should I share them now?",
    timestamp: "2024-01-15T11:45:00",
    isOwn: false,
  },
  {
    id: 7,
    sender: "You",
    avatar: "https://i.pravatar.cc/150?img=1",
    message: "Yes please, that would be great!",
    timestamp: "2024-01-15T11:47:00",
    isOwn: true,
  },
  {
    id: 8,
    sender: "David Brown",
    avatar: "https://i.pravatar.cc/150?img=5",
    message: "Just reviewed the slides. They look excellent!",
    timestamp: "2024-01-15T14:20:00",
    isOwn: false,
  },
  {
    id: 9,
    sender: "Sarah Miller",
    avatar: "https://i.pravatar.cc/150?img=2",
    message: "Thanks everyone for the quick responses. See you all tomorrow!",
    timestamp: "2024-01-15T15:00:00",
    isOwn: false,
  },
  {
    id: 10,
    sender: "You",
    avatar: "https://i.pravatar.cc/150?img=1",
    message: "Looking forward to it!",
    timestamp: "2024-01-15T15:02:00",
    isOwn: true,
  },
];

function formatMessageTime(timestamp: string): string {
  const date = parseISO(timestamp);
  return format(date, "h:mm a");
}

export default function Example() {
  const [messages, setMessages] = useState(initialMessages);
  const [message, setMessage] = useState("");
  const scrollAreaRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const scrollToBottom = () => {
      if (scrollAreaRef.current) {
        const viewport = scrollAreaRef.current.querySelector(
          '[data-slot="scroll-area-viewport"]'
        ) as HTMLElement;
        if (viewport) {
          viewport.scrollTop = viewport.scrollHeight;
        }
      }
    };

    scrollToBottom();
    const timeout = setTimeout(scrollToBottom, 100);
    return () => clearTimeout(timeout);
  }, [messages]);

  const handleSend = () => {
    if (!message.trim()) {
      return;
    }

    setMessages((prev) => [
      ...prev,
      {
        id: prev.length + 1,
        sender: "You",
        avatar: "https://i.pravatar.cc/150?img=1",
        message: message.trim(),
        timestamp: new Date().toISOString(),
        isOwn: true,
      },
    ]);
    setMessage("");
  };

  const handleKeyDown = (e: React.KeyboardEvent<HTMLInputElement>) => {
    if (e.key === "Enter" && !e.shiftKey) {
      e.preventDefault();
      handleSend();
    }
  };

  return (
    <div className="bg-muted/30 relative flex h-[500px] w-full max-w-sm flex-col overflow-hidden rounded-lg border">
      <div className="relative min-h-0 flex-1">
        <div ref={scrollAreaRef} className="h-full">
          <ScrollArea className="h-full">
            <div className="space-y-4 p-4">
              {messages.map((msg) => (
                <div
                  key={msg.id}
                  className={cn("flex gap-3", msg.isOwn && "flex-row-reverse")}
                >
                  <Avatar className="shrink-0">
                    <AvatarImage src={msg.avatar} alt={msg.sender} />
                    <AvatarFallback>
                      {msg.sender === "You"
                        ? "Y"
                        : msg.sender
                            .split(" ")
                            .map((n) => n[0])
                            .join("")}
                    </AvatarFallback>
                  </Avatar>
                  <Bubble
                    align={msg.isOwn ? "end" : "start"}
                    className="max-w-[75%]"
                    variant={msg.isOwn ? "default" : "outline"}
                  >
                    <div
                      className={cn(
                        "flex items-center gap-2",
                        msg.isOwn && "flex-row-reverse self-end"
                      )}
                    >
                      <span className="text-foreground text-xs font-medium">
                        {msg.sender}
                      </span>
                      <span className="text-muted-foreground text-xs">
                        {formatMessageTime(msg.timestamp)}
                      </span>
                    </div>
                    <BubbleContent>{msg.message}</BubbleContent>
                  </Bubble>
                </div>
              ))}
            </div>
          </ScrollArea>
        </div>
        <div className="from-muted/30 pointer-events-none absolute right-0 bottom-0 left-0 z-10 h-12 bg-linear-to-t to-transparent" />
      </div>
      <div className="bg-background relative z-20 shrink-0 border-t p-3">
        <div className="flex gap-2">
          <Input
            placeholder="Type a message..."
            value={message}
            onChange={(e) => setMessage(e.target.value)}
            onKeyDown={handleKeyDown}
            className="flex-1"
          />
          <Button onClick={handleSend} size="icon" disabled={!message.trim()}>
            <SendIcon />
          </Button>
        </div>
      </div>
    </div>
  );
}

demo.tsx
import Example from "@/components/ui/scroll-area6";

export default function Default() {
  return (
    <div className="flex w-full items-center justify-center p-6">
      <Example />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install date-fns lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar button input scroll-area
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
