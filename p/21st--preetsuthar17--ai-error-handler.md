<!-- AI Error Handler · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/ai-error-handler
     license: MIT · category: ai-chat
     An alert component that displays AI or API error messages with optional retry and dismiss actions. -->

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
components/ui/ai-error-handler.tsx
"use client";

import { AlertTriangle, Loader2, RefreshCw, X } from "lucide-react";
import { useCallback, useState } from "react";
import { cn } from "@/lib/utils";
import {
  Alert,
  AlertDescription,
  AlertTitle,
} from "@/registry/new-york/ui/alert";
import { Button } from "@/registry/new-york/ui/button";

export interface AIErrorHandlerProps {
  error?: string | Error | null;
  title?: string;
  onRetry?: () => Promise<void> | void;
  onDismiss?: () => void;
  className?: string;
}

function getErrorMessage(error: string | Error | null | undefined): string {
  if (!error) return "";
  if (typeof error === "string") return error;
  return error.message || "An error occurred";
}

export default function AIErrorHandler({
  error,
  title = "An Error Occurred",
  onRetry,
  onDismiss,
  className,
}: AIErrorHandlerProps) {
  const [isRetrying, setIsRetrying] = useState(false);

  const message = getErrorMessage(error);

  const handleRetry = useCallback(async () => {
    if (!onRetry) return;

    setIsRetrying(true);
    try {
      await onRetry();
    } finally {
      setIsRetrying(false);
    }
  }, [onRetry]);

  if (!(error && message)) return null;

  return (
    <Alert
      className={cn("w-full", className)}
      live="assertive"
      variant="destructive"
    >
      <AlertTriangle aria-hidden="true" className="size-4" />
      <div className="flex min-w-0 flex-1 flex-col gap-3">
        <div className="flex flex-col gap-2 sm:flex-row sm:items-start sm:justify-between">
          <div className="flex min-w-0 flex-1 flex-col gap-2">
            <AlertTitle className="wrap-break-word font-medium text-base">
              {title}
            </AlertTitle>
            <AlertDescription className="wrap-break-word">
              {message}
            </AlertDescription>
          </div>
          {onDismiss && (
            <Button
              aria-label="Dismiss error"
              className="min-h-[32px] min-w-[32px] shrink-0 touch-manipulation"
              onClick={onDismiss}
              size="icon-sm"
              type="button"
              variant="ghost"
            >
              <X aria-hidden="true" className="size-4" />
            </Button>
          )}
        </div>

        {onRetry && (
          <Button
            aria-busy={isRetrying}
            className="ml-auto min-h-[32px] w-fit touch-manipulation sm:ml-0"
            data-loading={isRetrying}
            disabled={isRetrying}
            onClick={handleRetry}
            type="button"
            variant={"secondary"}
          >
            {isRetrying ? (
              <>
                <Loader2 aria-hidden="true" className="size-4 animate-spin" />
                <span>Retrying…</span>
              </>
            ) : (
              <>
                <RefreshCw aria-hidden="true" className="size-4" />
                <span>Retry</span>
              </>
            )}
          </Button>
        )}
      </div>
    </Alert>
  );
}

demo.tsx
"use client";

import AIErrorHandler from "@/components/ui/ai-error-handler";

export default function AIErrorHandlerDemo() {
  const handleRetry = async () => {
    await new Promise((resolve) => setTimeout(resolve, 1500));
  };

  return (
    <div className="flex w-full max-w-md items-center justify-center p-6">
      <AIErrorHandler
        error="Failed to generate a response. The AI service is temporarily unavailable."
        onDismiss={() => {}}
        onRetry={handleRetry}
        title="Request Failed"
      />
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
npx shadcn@latest add alert badge button separator
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
