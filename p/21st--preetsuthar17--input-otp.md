<!-- Input OTP · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/input-otp
     license: unspecified · category: form
     A flexible and accessible one-time password input component with customizable slots, patterns, and animations. -->

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
components/ui/input-otp.tsx
"use client";

import { OTPInput, OTPInputContext } from "input-otp";
import { MinusIcon } from "lucide-react";
import * as React from "react";

import { cn } from "@/lib/utils";

const InputOTP = React.forwardRef<
  React.ComponentRef<typeof OTPInput>,
  React.ComponentProps<typeof OTPInput> & { containerClassName?: string }
>(function InputOTP({ className, containerClassName, ...props }, ref) {
  return (
    <OTPInput
      aria-busy={
        typeof (props as any).value === "string" &&
        typeof (props as any).maxLength === "number"
          ? (props as any).value.length < (props as any).maxLength
            ? true
            : undefined
          : undefined
      }
      aria-disabled={(props as any).disabled ? true : undefined}
      aria-invalid={(props as any)["aria-invalid"] ? true : undefined}
      className={cn("disabled:cursor-not-allowed", className)}
      containerClassName={cn(
        "flex touch-manipulation items-center gap-2 has-disabled:opacity-50",
        containerClassName
      )}
      data-slot="input-otp"
      ref={ref}
      {...props}
    />
  );
});

const InputOTPGroup = React.forwardRef<
  HTMLDivElement,
  React.ComponentProps<"div">
>(function InputOTPGroup({ className, ...props }, ref) {
  return (
    <div
      className={cn("flex items-center", className)}
      data-slot="input-otp-group"
      ref={ref}
      {...props}
    />
  );
});

const InputOTPSlot = React.forwardRef<
  HTMLDivElement,
  React.ComponentProps<"div"> & { index: number }
>(function InputOTPSlot({ index, className, ...props }, ref) {
  const inputOTPContext = React.useContext(OTPInputContext);
  const { char, hasFakeCaret, isActive } = inputOTPContext?.slots[index] ?? {};

  return (
    <div
      className={cn(
        "relative flex size-9 items-center justify-center border-input border-y border-r text-sm tabular-nums shadow-xs outline-none transition-[color,box-shadow] first:rounded-l-md first:border-l last:rounded-r-md aria-invalid:border-destructive data-[active=true]:z-10 data-[active=true]:border-ring data-[active=true]:ring-[3px] data-[active=true]:ring-ring/50 data-[active=true]:aria-invalid:border-destructive data-[active=true]:aria-invalid:ring-destructive/20 motion-safe:duration-200 dark:bg-input/30 dark:data-[active=true]:aria-invalid:ring-destructive/40",
        className
      )}
      data-active={isActive}
      data-slot="input-otp-slot"
      ref={ref}
      {...props}
    >
      {char}
      {hasFakeCaret && (
        <div className="pointer-events-none absolute inset-0 flex items-center justify-center">
          <div className="h-4 w-px animate-caret-blink bg-foreground duration-1000 motion-reduce:animate-none" />
        </div>
      )}
    </div>
  );
});

const InputOTPSeparator = React.forwardRef<
  HTMLDivElement,
  React.ComponentProps<"div">
>(function InputOTPSeparator({ ...props }, ref) {
  return (
    <div data-slot="input-otp-separator" ref={ref} role="separator" {...props}>
      <MinusIcon aria-hidden="true" />
    </div>
  );
});

export { InputOTP, InputOTPGroup, InputOTPSlot, InputOTPSeparator };

demo.tsx
import { 
  InputOTP,
  InputOTPGroup,
  InputOTPSlot,
  InputOTPSeparator
} from "@/components/ui/input-otp";

export default function DemoOne() {
  return (
    <div className="flex flex-col gap-8 max-w-md mx-auto">
      <div className="flex flex-col gap-3 text-center">
        <InputOTP maxLength={6}>
          <InputOTPGroup>
            <InputOTPSlot index={0} />
            <InputOTPSlot index={1} />
            <InputOTPSlot index={2} />
          </InputOTPGroup>
          <InputOTPSeparator />
          <InputOTPGroup>
            <InputOTPSlot index={3} />
            <InputOTPSlot index={4} />
            <InputOTPSlot index={5} />
          </InputOTPGroup>
        </InputOTP>
        <div className="text-sm text-muted-foreground">
          Enter your one-time password.
        </div>
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority input-otp motion
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
