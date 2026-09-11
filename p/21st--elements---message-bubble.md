<!-- AI Message Bubble · @elements- · https://21st.dev/@elements-/components/message-bubble
     license: MIT · category: ai-chat
     Chat message bubble with user and assistant roles, provider avatars, timestamps, streaming cursor, and copy-on-hover. -->

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
components/ui/ai-message-bubble.tsx
"use client";

import * as React from "react";

import { Check, Copy, User } from "lucide-react";

import { cn } from "@/lib/utils";

import { AnthropicLogo } from "@/components/ui/logos/anthropic";
import { CohereLogo } from "@/components/ui/logos/cohere";
import { DeepSeekLogo } from "@/components/ui/logos/deepseek";
import { GeminiLogo } from "@/components/ui/logos/gemini";
import { GroqLogo } from "@/components/ui/logos/groq";
import { MetaLogo } from "@/components/ui/logos/meta";
import { MistralLogo } from "@/components/ui/logos/mistral";
import { OpenAILogo } from "@/components/ui/logos/openai";
import { XAILogo } from "@/components/ui/logos/xai";

type MessageRole = "user" | "assistant";

type Provider =
  | "openai"
  | "anthropic"
  | "google"
  | "xai"
  | "deepseek"
  | "mistral"
  | "groq"
  | "cohere"
  | "meta";

const PROVIDER_LOGOS: Record<
  Provider,
  React.ComponentType<{ className?: string }>
> = {
  openai: OpenAILogo,
  anthropic: AnthropicLogo,
  google: GeminiLogo,
  xai: XAILogo,
  deepseek: DeepSeekLogo,
  mistral: MistralLogo,
  groq: GroqLogo,
  cohere: CohereLogo,
  meta: MetaLogo,
};

interface AiMessageBubbleProps {
  role: MessageRole;
  content?: string;
  provider?: Provider;
  timestamp?: Date;
  avatar?: React.ReactNode;
  isStreaming?: boolean;
  className?: string;
  children?: React.ReactNode;
}

export function AiMessageBubble({
  role,
  content,
  provider,
  timestamp,
  avatar,
  isStreaming = false,
  className,
  children,
}: AiMessageBubbleProps) {
  const [copied, setCopied] = React.useState(false);

  const isUser = role === "user";

  const handleCopy = React.useCallback(async () => {
    await navigator.clipboard.writeText(content);
    setCopied(true);
    setTimeout(() => setCopied(false), 2000);
  }, [content]);

  const ProviderLogo = provider ? PROVIDER_LOGOS[provider] : null;

  const defaultAvatar = isUser ? (
    <div className="flex size-7 items-center justify-center border bg-foreground text-background">
      <User className="size-3.5" />
    </div>
  ) : ProviderLogo ? (
    <div className="flex size-7 items-center justify-center border bg-background">
      <ProviderLogo className="size-3.5" />
    </div>
  ) : (
    <div className="flex size-7 items-center justify-center border bg-background">
      <svg
        className="size-3.5"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        strokeWidth="2"
        strokeLinecap="round"
        strokeLinejoin="round"
      >
        <title>AI</title>
        <path d="M12 2a2 2 0 0 1 2 2c0 .74-.4 1.39-1 1.73V7h1a7 7 0 0 1 7 7h1a1 1 0 0 1 1 1v3a1 1 0 0 1-1 1h-1v1a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-1H2a1 1 0 0 1-1-1v-3a1 1 0 0 1 1-1h1a7 7 0 0 1 7-7h1V5.73c-.6-.34-1-.99-1-1.73a2 2 0 0 1 2-2z" />
        <path d="M7.5 13a1.5 1.5 0 1 0 3 0 1.5 1.5 0 0 0-3 0Z" />
        <path d="M13.5 13a1.5 1.5 0 1 0 3 0 1.5 1.5 0 0 0-3 0Z" />
        <path d="M8 17h8" />
      </svg>
    </div>
  );

  return (
    <div
      data-slot="ai-message-bubble"
      role="article"
      aria-label={isUser ? "Your message" : "AI response"}
      className={cn(
        "group flex gap-3 font-mono",
        isUser && "flex-row-reverse",
        className
      )}
    >
      <div className="shrink-0">{avatar || defaultAvatar}</div>

      <div
        className={cn(
          "relative max-w-[80%] px-3 py-2",
          isUser ? "border bg-foreground text-background" : "text-foreground"
        )}
      >
        <div aria-live={isStreaming ? "polite" : undefined}>
          {children ? (
            <div className="prose prose-xs dark:prose-invert max-w-none text-[13px] leading-relaxed [&_h1]:text-base [&_h2]:text-sm [&_h3]:text-[13px] [&_h4]:text-[13px] [&_p]:text-[13px] [&_li]:text-[13px] [&_code]:text-xs [&_pre]:text-xs">
              {children}
            </div>
          ) : (
            <p className="m-0 whitespace-pre-wrap text-[13px] leading-relaxed">
              {content}
            </p>
          )}
          {isStreaming && (
            <span className="ml-1 inline-block h-3.5 w-[2px] animate-pulse bg-current" />
          )}
        </div>

        {timestamp && (
          <time className="mt-2 block text-[10px] uppercase tracking-wider opacity-60">
            {timestamp.toLocaleTimeString([], {
              hour: "2-digit",
              minute: "2-digit",
            })}
          </time>
        )}

        {!isUser && !isStreaming && (
          <button
            type="button"
            className="absolute -right-8 top-0.5 flex size-6 items-center justify-center border bg-background opacity-0 transition-opacity hover:bg-muted group-hover:opacity-100"
            onClick={handleCopy}
          >
            {copied ? (
              <Check className="size-3" />
            ) : (
              <Copy className="size-3" />
            )}
            <span className="sr-only">Copy message</span>
          </button>
        )}
      </div>
    </div>
  );
}

export type { AiMessageBubbleProps, MessageRole, Provider };

demo.tsx
"use client";

import { AiMessageBubble } from "@/components/ui/message-bubble";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center bg-background p-6 text-foreground">
      <div className="flex w-full max-w-md flex-col gap-6">
        <AiMessageBubble
          role="user"
          content="What's the difference between let and const in JavaScript?"
          timestamp={new Date()}
        />
        <AiMessageBubble
          role="assistant"
          provider="anthropic"
          content={`Both declare block-scoped variables.

- "let" can be reassigned later.
- "const" can't be reassigned after it's set.

Reach for const by default, and use let only when you actually need to reassign.`}
          timestamp={new Date()}
        />
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
npx shadcn@latest add button
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
