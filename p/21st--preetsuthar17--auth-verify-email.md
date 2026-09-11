<!-- Auth Verify Email · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/auth-verify-email
     license: MIT · category: card
     Email verification status card with pending, verifying, verified, expired, and error states plus a resend link with cooldown. -->

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
components/ui/auth-verify-email.tsx
"use client";

import {
  CheckCircle2,
  Clock,
  Loader2,
  Mail,
  RefreshCw,
  XCircle,
} from "lucide-react";
import { useCallback, useEffect, useState } from "react";
import { cn } from "@/lib/utils";
import { Button } from "@/registry/new-york/ui/button";
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from "@/registry/new-york/ui/card";

export type VerificationStatus =
  | "pending"
  | "verifying"
  | "verified"
  | "expired"
  | "error";

interface StatusIconProps {
  status: VerificationStatus;
}

function StatusIcon({ status }: StatusIconProps) {
  switch (status) {
    case "verified":
      return (
        <div className="flex size-16 items-center justify-center rounded-full bg-primary/10">
          <CheckCircle2 aria-hidden="true" className="size-8 text-primary" />
        </div>
      );
    case "verifying":
      return (
        <div className="flex size-16 items-center justify-center rounded-full bg-primary/10">
          <Loader2
            aria-hidden="true"
            className="size-8 animate-spin text-primary"
          />
        </div>
      );
    case "expired":
    case "error":
      return (
        <div className="flex size-16 items-center justify-center rounded-full bg-destructive/10">
          <XCircle aria-hidden="true" className="size-8 text-destructive" />
        </div>
      );
    default:
      return (
        <div className="flex size-16 items-center justify-center rounded-full bg-muted">
          <Mail aria-hidden="true" className="size-8 text-muted-foreground" />
        </div>
      );
  }
}

interface StatusHeaderProps {
  status: VerificationStatus;
  email?: string;
  errorMessage?: string;
}

function StatusHeader({ status, email, errorMessage }: StatusHeaderProps) {
  const getStatusContent = (): { title: string; description: string } => {
    switch (status) {
      case "verified":
        return {
          title: "Email verified",
          description: "Your email address has been successfully verified.",
        };
      case "verifying":
        return {
          title: "Verifying email",
          description: "Please wait while we verify your email address…",
        };
      case "expired":
        return {
          title: "Verification link expired",
          description:
            "The verification link has expired. Please request a new one.",
        };
      case "error":
        return {
          title: "Verification failed",
          description:
            errorMessage ||
            "Something went wrong. Please try requesting a new verification link.",
        };
      default:
        return {
          title: "Verify your email",
          description: email
            ? `We've sent a verification link to ${email}. Please check your inbox and click the link to verify your email address.`
            : "We've sent a verification link to your email address. Please check your inbox and click the link to verify your email address.",
        };
    }
  };

  const content = getStatusContent();

  return (
    <>
      <CardTitle>{content.title}</CardTitle>
      <CardDescription>{content.description}</CardDescription>
    </>
  );
}

function PendingStateHelp() {
  return (
    <div className="flex items-center gap-2 rounded-lg border bg-muted/50 p-4">
      <Clock
        aria-hidden="true"
        className="size-4 shrink-0 text-muted-foreground"
      />
      <div className="flex flex-col gap-1">
        <p className="font-medium text-sm">Didn&apos;t receive the email?</p>
        <p className="text-muted-foreground text-xs">
          Check your spam folder or try resending the verification link.
        </p>
      </div>
    </div>
  );
}

interface ResendButtonProps {
  cooldown: number;
  isResending: boolean;
  onResend: () => void;
  variant?: "default" | "outline";
  label?: string;
}

function ResendButton({
  cooldown,
  isResending,
  onResend,
  variant = "default",
  label = "Resend verification email",
}: ResendButtonProps) {
  return (
    <Button
      aria-busy={isResending}
      className="min-h-[44px] w-full touch-manipulation sm:min-h-[32px]"
      data-loading={isResending}
      disabled={cooldown > 0 || isResending}
      onClick={onResend}
      type="button"
      variant={variant}
    >
      {isResending ? (
        <>
          <Loader2 aria-hidden="true" className="size-4 animate-spin" />
          Sending…
        </>
      ) : cooldown > 0 ? (
        <>
          <RefreshCw aria-hidden="true" className="size-4" />
          Resend in {cooldown}s
        </>
      ) : (
        <>
          <RefreshCw aria-hidden="true" className="size-4" />
          {label}
        </>
      )}
    </Button>
  );
}

interface PendingStateProps {
  onResend?: () => void;
  cooldown: number;
  isResending: boolean;
}

function PendingState({ onResend, cooldown, isResending }: PendingStateProps) {
  return (
    <div className="flex w-full flex-col gap-4">
      <PendingStateHelp />
      {onResend && (
        <ResendButton
          cooldown={cooldown}
          isResending={isResending}
          onResend={onResend}
          variant="outline"
        />
      )}
    </div>
  );
}

interface ExpiredErrorStateProps {
  onResend?: () => void;
  cooldown: number;
  isResending: boolean;
}

function ExpiredErrorState({
  onResend,
  cooldown,
  isResending,
}: ExpiredErrorStateProps) {
  if (!onResend) return null;

  return (
    <ResendButton
      cooldown={cooldown}
      isResending={isResending}
      label="Request new verification link"
      onResend={onResend}
    />
  );
}

function VerifiedState() {
  return (
    <div className="w-full rounded-lg border border-primary/20 bg-primary/5 p-4 text-center">
      <p className="font-medium text-primary text-sm">
        You can now use all features of your account.
      </p>
    </div>
  );
}

export interface AuthVerifyEmailProps {
  email?: string;
  status?: VerificationStatus;
  onResend?: () => void;
  onVerify?: (token: string) => void;
  className?: string;
  isLoading?: boolean;
  errorMessage?: string;
  resendCooldown?: number;
}

export default function AuthVerifyEmail({
  email,
  status = "pending",
  onResend,
  onVerify,
  className,
  isLoading = false,
  errorMessage,
  resendCooldown = 60,
}: AuthVerifyEmailProps) {
  const [cooldown, setCooldown] = useState(0);
  const [isResending, setIsResending] = useState(false);

  useEffect(() => {
    if (cooldown > 0) {
      const timer = setTimeout(() => {
        setCooldown((prev) => prev - 1);
      }, 1000);
      return () => clearTimeout(timer);
    }
  }, [cooldown]);

  const handleResend = useCallback(async () => {
    if (cooldown > 0 || isResending) return;
    setIsResending(true);
    try {
      await onResend?.();
      setCooldown(resendCooldown);
    } finally {
      setIsResending(false);
    }
  }, [cooldown, isResending, onResend, resendCooldown]);

  return (
    <Card className={cn("w-full max-w-sm shadow-xs", className)}>
      <CardHeader>
        <StatusHeader
          email={email}
          errorMessage={errorMessage}
          status={status}
        />
      </CardHeader>
      <CardContent>
        <div className="flex flex-col items-center gap-6">
          <StatusIcon status={status} />

          {status === "pending" && (
            <PendingState
              cooldown={cooldown}
              isResending={isResending}
              onResend={onResend}
            />
          )}

          {(status === "expired" || status === "error") && (
            <ExpiredErrorState
              cooldown={cooldown}
              isResending={isResending}
              onResend={onResend}
            />
          )}

          {status === "verified" && <VerifiedState />}
        </div>
      </CardContent>
    </Card>
  );
}

demo.tsx
"use client";

import AuthVerifyEmail from "@/components/ui/auth-verify-email";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <AuthVerifyEmail
        email="you@example.com"
        status="pending"
        onResend={() => {}}
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
npx shadcn@latest add button card
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
