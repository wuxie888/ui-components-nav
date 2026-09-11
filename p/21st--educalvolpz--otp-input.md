<!-- Otp Input · @educalvolpz · https://21st.dev/@educalvolpz/components/otp-input
     license: unspecified · category: form
     Here is Otp Input component -->

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

import { cn } from "@/lib/utils";
import { OTPInput, OTPInputContext } from "input-otp";
import { MinusIcon } from "lucide-react";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import { type ReactNode, useContext, useEffect, useState } from "react";

// Animation constants
const EASE_OUT_QUINT_X1 = 0.22;
const EASE_OUT_QUINT_Y1 = 1;
const EASE_OUT_QUINT_X2 = 0.36;
const EASE_OUT_QUINT_Y2 = 1;
const EASE_OUT_QUINT = [
  EASE_OUT_QUINT_X1,
  EASE_OUT_QUINT_Y1,
  EASE_OUT_QUINT_X2,
  EASE_OUT_QUINT_Y2,
] as const;
const ANIMATION_DURATION_SHORT = 0.1;
const ANIMATION_DURATION_MEDIUM = 0.15;
const ANIMATION_DURATION_STANDARD = 0.2;
const ANIMATION_DURATION_LONG = 0.3;
const STAGGER_DELAY = 0.05;
const SCALE_FILLED = 1.05;
const SCALE_HOVER = 1.02;
const SCALE_TAP = 0.98;
const INITIAL_SCALE = 0.8;
const INITIAL_Y = 10;
const SEPARATOR_DELAY = 0.15;

export interface AnimatedInputOTPProps {
  "aria-describedby"?: string;
  "aria-label"?: string;
  className?: string;
  containerClassName?: string;
  maxLength?: number;
  onChange?: (value: string) => void;
  onComplete?: (value: string) => void;
  value?: string;
}

function AnimatedInputOTP({
  className,
  containerClassName,
  value,
  onChange,
  onComplete,
  maxLength = 6,
  children,
  ...props
}: AnimatedInputOTPProps & { children: ReactNode }) {
  const handleChange = (newValue: string) => {
    // Only allow numeric characters
    const numericValue = newValue.replace(/[^0-9]/g, "");
    onChange?.(numericValue);
  };

  return (
    <OTPInput
      aria-describedby={props["aria-describedby"]}
      aria-label={props["aria-label"] || "One-time password input"}
      className={cn("disabled:cursor-not-allowed", className)}
      containerClassName={cn(
        "flex items-center gap-2 has-disabled:opacity-50",
        containerClassName
      )}
      data-slot="input-otp"
      maxLength={maxLength}
      onChange={handleChange}
      onComplete={onComplete}
      value={value}
      {...props}
    >
      {children}
    </OTPInput>
  );
}

interface AnimatedInputOTPGroupProps {
  children?: ReactNode;
  className?: string;
}

function AnimatedInputOTPGroup({
  className,
  children,
}: AnimatedInputOTPGroupProps) {
  const shouldReduceMotion = useReducedMotion();
  return (
    <motion.div
      animate={shouldReduceMotion ? { opacity: 1 } : { opacity: 1, y: 0 }}
      className={cn("flex items-center gap-2", className)}
      data-slot="input-otp-group"
      initial={
        shouldReduceMotion ? { opacity: 1 } : { opacity: 0, y: INITIAL_Y }
      }
      transition={
        shouldReduceMotion
          ? { duration: 0 }
          : {
              duration: ANIMATION_DURATION_LONG,
              ease: EASE_OUT_QUINT,
            }
      }
    >
      {children}
    </motion.div>
  );
}

interface AnimatedInputOTPSlotProps {
  className?: string;
  index: number;
}

function AnimatedInputOTPSlot({ index, className }: AnimatedInputOTPSlotProps) {
  const inputOTPContext = useContext(OTPInputContext);
  const { char, hasFakeCaret, isActive } = inputOTPContext?.slots[index] ?? {};
  const [isFilled, setIsFilled] = useState(false);
  const shouldReduceMotion = useReducedMotion();

  useEffect(() => {
    if (char && !isFilled) {
      setIsFilled(true);
    } else if (!char && isFilled) {
      setIsFilled(false);
    }
  }, [char, isFilled]);

  return (
    <motion.div
      animate={
        shouldReduceMotion
          ? { opacity: 1 }
          : {
              opacity: 1,
              scale: isFilled ? SCALE_FILLED : 1,
              y: 0,
            }
      }
      className={cn(
        "relative flex h-9 w-9 items-center justify-center rounded-md border border-zinc-300 bg-background text-sm shadow-sm outline-none transition-all aria-invalid:border-destructive data-[active=true]:z-10 data-[active=true]:border-ring data-[active=true]:ring-[3px] data-[active=true]:ring-ring/50 data-[active=true]:aria-invalid:border-destructive data-[active=true]:aria-invalid:ring-destructive/20 dark:border-zinc-700 dark:bg-input/30 dark:data-[active=true]:aria-invalid:ring-destructive/40",
        className
      )}
      data-active={isActive}
      data-slot="input-otp-slot"
      initial={
        shouldReduceMotion
          ? { opacity: 1 }
          : { opacity: 0, scale: INITIAL_SCALE, y: INITIAL_Y }
      }
      transition={
        shouldReduceMotion
          ? { duration: 0 }
          : {
              delay: index * STAGGER_DELAY,
              duration: ANIMATION_DURATION_STANDARD,
              ease: EASE_OUT_QUINT,
              scale: {
                duration: ANIMATION_DURATION_MEDIUM,
                ease: EASE_OUT_QUINT,
              },
            }
      }
      whileHover={
        shouldReduceMotion
          ? {}
          : {
              scale: SCALE_HOVER,
              transition: {
                duration: ANIMATION_DURATION_MEDIUM,
                ease: EASE_OUT_QUINT,
              },
            }
      }
      whileTap={
        shouldReduceMotion
          ? {}
          : {
              scale: SCALE_TAP,
              transition: {
                duration: ANIMATION_DURATION_SHORT,
                ease: EASE_OUT_QUINT,
              },
            }
      }
    >
      <AnimatePresence mode="wait">
        {char ? (
          <motion.span
            animate={
              shouldReduceMotion
                ? { opacity: 1, scale: 1 }
                : { opacity: 1, rotateY: 0, scale: 1 }
            }
            className="font-medium"
            exit={
              shouldReduceMotion
                ? { opacity: 0, transition: { duration: 0 } }
                : { opacity: 0, rotateY: 90, scale: 0.5 }
            }
            initial={
              shouldReduceMotion
                ? { opacity: 1, scale: 1 }
                : { opacity: 0, rotateY: -90, scale: 0.5 }
            }
            key={char}
            transition={
              shouldReduceMotion
                ? { duration: 0 }
                : {
                    duration: ANIMATION_DURATION_STANDARD,
                    ease: EASE_OUT_QUINT,
                  }
            }
          >
            {char}
          </motion.span>
        ) : null}
      </AnimatePresence>

      {hasFakeCaret && !shouldReduceMotion && (
        <motion.div
          animate={{ opacity: 1 }}
          className="pointer-events-none absolute inset-0 flex items-center justify-center"
          exit={{ opacity: 0 }}
          initial={{ opacity: 0 }}
          transition={{ duration: ANIMATION_DURATION_SHORT }}
        >
          <motion.div
            animate={{ opacity: [0, 1, 0] }}
            className="h-4 w-px bg-foreground"
            transition={{
              duration: 1,
              ease: [0.645, 0.045, 0.355, 1],
              repeat: Number.POSITIVE_INFINITY,
            }}
          />
        </motion.div>
      )}
    </motion.div>
  );
}

function AnimatedInputOTPSeparator() {
  const shouldReduceMotion = useReducedMotion();
  return (
    <motion.div
      animate={shouldReduceMotion ? { opacity: 1 } : { opacity: 1, scale: 1 }}
      data-slot="input-otp-separator"
      initial={
        shouldReduceMotion
          ? { opacity: 1 }
          : { opacity: 0, scale: INITIAL_SCALE }
      }
      transition={
        shouldReduceMotion
          ? { duration: 0 }
          : {
              delay: SEPARATOR_DELAY,
              duration: ANIMATION_DURATION_LONG,
              ease: EASE_OUT_QUINT,
            }
      }
    >
      <MinusIcon className="h-4 w-4 text-muted-foreground" />
    </motion.div>
  );
}

// Main component that combines everything
export function AnimatedOTPInput({
  maxLength = 6,
  className,
  value,
  onChange,
  onComplete,
  ...props
}: AnimatedInputOTPProps) {
  return (
    <AnimatedInputOTP
      className={className}
      maxLength={maxLength}
      onChange={onChange}
      onComplete={onComplete}
      value={value}
      {...props}
    >
      <AnimatedInputOTPGroup>
        <AnimatedInputOTPSlot index={0} />
        <AnimatedInputOTPSlot index={1} />
        <AnimatedInputOTPSlot index={2} />
      </AnimatedInputOTPGroup>
      <AnimatedInputOTPSeparator />
      <AnimatedInputOTPGroup>
        <AnimatedInputOTPSlot index={3} />
        <AnimatedInputOTPSlot index={4} />
        <AnimatedInputOTPSlot index={5} />
      </AnimatedInputOTPGroup>
    </AnimatedInputOTP>
  );
}

export {
  AnimatedInputOTP,
  AnimatedInputOTPGroup,
  AnimatedInputOTPSeparator,
  AnimatedInputOTPSlot,
};

export default AnimatedOTPInput;

demo.tsx
"use client"

import * as React from "react"
import { CheckCircle, RefreshCw } from "lucide-react"
import { motion } from "motion/react"

import { Button } from "@/components/ui/button"
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from "@/components/ui/card"

import { AnimatedOTPInput } from "@/components/ui/otp-input"

export function AnimatedOTPInputDemo() {
  const [value, setValue] = React.useState("")
  const [isComplete, setIsComplete] = React.useState(false)
  const [isLoading, setIsLoading] = React.useState(false)

  const handleComplete = (otp: string) => {
    setValue(otp)
    setIsComplete(true)
    setIsLoading(true)

    // Simulate verification process
    setTimeout(() => {
      setIsLoading(false)
    }, 2000)
  }

  const handleReset = () => {
    setValue("")
    setIsComplete(false)
    setIsLoading(false)
  }

  return (
    <div className="flex min-h-[400px] flex-col items-center justify-center p-6">
      <Card className="w-full max-w-md">
        <CardHeader className="text-center">
          <CardTitle className="text-2xl font-bold">Verify Your Code</CardTitle>
          <CardDescription>
            Enter the 6-digit code sent to your device
          </CardDescription>
        </CardHeader>
        <CardContent className="space-y-6">
          <div className="flex justify-center">
            <AnimatedOTPInput
              value={value}
              onChange={setValue}
              onComplete={handleComplete}
              maxLength={6}
            />
          </div>

          {isComplete && (
            <motion.div
              initial={{ opacity: 0, y: 10 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ duration: 0.3, ease: [0.22, 1, 0.36, 1] }}
              className="space-y-4 text-center"
            >
              {isLoading ? (
                <div className="text-muted-foreground flex items-center justify-center space-x-2">
                  <RefreshCw className="h-4 w-4 animate-spin" />
                  <span>Verifying code...</span>
                </div>
              ) : (
                <div className="flex items-center justify-center space-x-2 text-green-600">
                  <CheckCircle className="h-4 w-4" />
                  <span className="font-medium">
                    Code verified successfully!
                  </span>
                </div>
              )}
            </motion.div>
          )}

          <div className="flex justify-center">
            <Button variant="outline" onClick={handleReset} className="w-full">
              Reset Code
            </Button>
          </div>
        </CardContent>
      </Card>
    </div>
  )
}

export default AnimatedOTPInputDemo
```

Install NPM dependencies:
```bash
npm install input-otp lucide-react motion
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
