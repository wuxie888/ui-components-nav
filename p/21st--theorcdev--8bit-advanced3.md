<!-- 8-bit Newsletter Signup · @theorcdev · https://21st.dev/@theorcdev/components/8bit-advanced3
     license: MIT · category: input
     Newsletter signup card with an email input and subscribe CTA, built in the retro 8-bit pixel style. -->

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
components/ui/8bit/blocks/advanced3.tsx
import { cn } from "@/lib/utils";

import { Badge } from "@/components/ui/8bit/badge";
import { Button } from "@/components/ui/8bit/button";
import {
  Card,
  CardContent,
} from "@/components/ui/8bit/card";
import { Input } from "@/components/ui/8bit/input";

import "@/components/ui/8bit/styles/retro.css";

interface Advanced3Props {
  badge?: string;
  className?: string;
  cta?: string;
  description?: string;
  placeholder?: string;
  title?: string;
}

export default function Advanced3({
  title = "Join the Guild",
  description = "Get notified when new components and blocks drop. No spam, just pixels.",
  placeholder = "warrior@email.com",
  cta = "SUBSCRIBE",
  badge = "FREE",
  className,
}: Advanced3Props) {
  return (
    <section className={cn("w-full px-4 py-16", className)}>
      <div className="mx-auto max-w-xl">
        <Card>
          <CardContent className="pt-8 pb-8">
            <div className="text-center">
              {badge && (
                <div className="mb-4">
                  <Badge>{badge}</Badge>
                </div>
              )}

              <h2 className="retro mb-3 font-bold text-2xl tracking-tight">
                {title}
              </h2>

              <p className="mx-auto mb-6 max-w-sm text-muted-foreground retro text-[9px] leading-relaxed">
                {description}
              </p>

              <div className="flex flex-col md:flex-row items-center gap-4">
                <Input
                  aria-label="Email address"
                  autoComplete="email"
                  className="retro flex-1 text-xs"
                  name="email"
                  placeholder={placeholder}
                  type="email"
                />
                <Button className="text-xs">{cta}</Button>
              </div>

              <p className="retro mt-3 text-muted-foreground text-[10px]">
                No spam. Unsubscribe anytime.
              </p>
            </div>
          </CardContent>
        </Card>
      </div>
    </section>
  );
}

components/ui/8bit/styles/retro.css
@import url("https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap");

.retro {
  font-family:
    "Press Start 2P",
    system-ui,
    -apple-system,
    sans-serif;
  line-height: 1.5;
  letter-spacing: 0.5px;
}

.pixelated {
  image-rendering: pixelated;
  image-rendering: crisp-edges;
}

components/ui/8bit/badge.tsx
import { type VariantProps, cva } from "class-variance-authority";

import { cn } from "@/lib/utils";

import { Badge as ShadcnBadge } from "@/components/ui/badge";

export const badgeVariants = cva("", {
  variants: {
    font: {
      normal: "",
      retro: "retro",
    },
    variant: {
      default: "border-primary bg-primary",
      destructive: "border-destructive bg-destructive",
      outline: "border-background bg-background",
      secondary: "border-secondary bg-secondary",
    },
  },
  defaultVariants: {
    variant: "default",
  },
});

export interface BitButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof badgeVariants> {
  asChild?: boolean;
}

function Badge({
  children,
  className = "",
  font,
  variant,
  ...props
}: BitButtonProps) {
  const color = badgeVariants({ variant, font });

  const classes = className.split(" ");

  // visual classes for badge and sidebars
  const visualClasses = classes.filter(
    (c) =>
      c.startsWith("bg-") ||
      c.startsWith("border-") ||
      c.startsWith("text-") ||
      c.startsWith("rounded-")
  );

  // Container should accept all non-visual utility classes (e.g., size, spacing, layout)
  const containerClasses = classes.filter(
    (c) =>
      !(
        c.startsWith("bg-") ||
        c.startsWith("border-") ||
        c.startsWith("text-") ||
        c.startsWith("rounded-")
      )
  );

  return (
    <div className={cn("relative inline-flex items-stretch", containerClasses)}>
      <ShadcnBadge
        {...props}
        className={cn(
          "h-full",
          "rounded-none",
          "w-full",
          font !== "normal" && "retro",
          visualClasses
        )}
        variant={variant}
      >
        {children}
      </ShadcnBadge>

      {/* Left pixel bar */}
      <div
        className={cn(
          "-left-1.5 absolute inset-y-[4px] w-1.5",
          color,
          visualClasses
        )}
      />
      {/* Right pixel bar */}
      <div
        className={cn(
          "-right-1.5 absolute inset-y-[4px] w-1.5",
          color,
          visualClasses
        )}
      />
    </div>
  );
}

export { Badge };

components/ui/8bit/button.tsx
import { type VariantProps, cva } from "class-variance-authority";
import { Slot } from "radix-ui";
import type { ButtonHTMLAttributes, Ref } from "react";

import { Button as ShadcnButton } from "@/components/ui/button";
import { cn } from "@/lib/utils";

import "@/components/ui/8bit/styles/retro.css";

export const buttonVariants = cva("", {
  variants: {
    font: {
      normal: "",
      retro: "retro",
    },
    variant: {
      default: "bg-foreground",
      destructive: "bg-foreground",
      outline: "bg-foreground",
      secondary: "bg-secondary text-secondary-foreground hover:bg-secondary/80",
      ghost: "hover:bg-accent hover:text-accent-foreground",
      link: "text-primary underline-offset-4 hover:underline",
    },
    size: {
      default: "",
      sm: "",
      lg: "",
      icon: "",
    },
  },
  defaultVariants: {
    variant: "default",
    size: "default",
  },
});

export interface BitButtonProps
  extends ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean;
  ref?: Ref<HTMLButtonElement>;
}

interface ButtonDecorationsProps {
  size: BitButtonProps["size"];
  variant: BitButtonProps["variant"];
}

function ButtonDecorations({ size, variant }: ButtonDecorationsProps) {
  return (
    <span
      aria-hidden="true"
      className="pointer-events-none contents"
      data-slot="button-decorations"
    >
      {variant !== "ghost" && variant !== "link" && size !== "icon" && (
        <>
          {/* Pixelated border */}
          <span className="absolute -top-1.5 left-1.5 h-1.5 w-1/2 bg-foreground dark:bg-ring" />
          <span className="absolute -top-1.5 right-1.5 h-1.5 w-1/2 bg-foreground dark:bg-ring" />
          <span className="absolute -bottom-1.5 left-1.5 h-1.5 w-1/2 bg-foreground dark:bg-ring" />
          <span className="absolute -bottom-1.5 right-1.5 h-1.5 w-1/2 bg-foreground dark:bg-ring" />
          <span className="absolute top-0 left-0 size-1.5 bg-foreground dark:bg-ring" />
          <span className="absolute top-0 right-0 size-1.5 bg-foreground dark:bg-ring" />
          <span className="absolute bottom-0 left-0 size-1.5 bg-foreground dark:bg-ring" />
          <span className="absolute right-0 bottom-0 size-1.5 bg-foreground dark:bg-ring" />
          <span className="absolute top-1.5 -left-1.5 h-[calc(100%-12px)] w-1.5 bg-foreground dark:bg-ring" />
          <span className="absolute top-1.5 -right-1.5 h-[calc(100%-12px)] w-1.5 bg-foreground dark:bg-ring" />
          {variant !== "outline" && (
            <>
              {/* Top shadow */}
              <span className="absolute top-0 left-0 h-1.5 w-full bg-foreground/20" />
              <span className="absolute top-1.5 left-0 h-1.5 w-3 bg-foreground/20" />

              {/* Bottom shadow */}
              <span className="absolute bottom-0 left-0 h-1.5 w-full bg-foreground/20" />
              <span className="absolute right-0 bottom-1.5 h-1.5 w-3 bg-foreground/20" />
            </>
          )}
        </>
      )}

      {size === "icon" && (
        <>
          <span className="absolute top-0 left-0 h-[5px] w-full bg-foreground md:h-1.5 dark:bg-ring" />
          <span className="absolute bottom-0 h-[5px] w-full bg-foreground md:h-1.5 dark:bg-ring" />
          <span className="absolute top-1 -left-1 h-1/2 w-[5px] bg-foreground md:w-1.5 dark:bg-ring" />
          <span className="absolute bottom-1 -left-1 h-1/2 w-[5px] bg-foreground md:w-1.5 dark:bg-ring" />
          <span className="absolute top-1 -right-1 h-1/2 w-[5px] bg-foreground md:w-1.5 dark:bg-ring" />
          <span className="absolute -right-1 bottom-1 h-1/2 w-[5px] bg-foreground md:w-1.5 dark:bg-ring" />
        </>
      )}
    </span>
  );
}

function Button({
  asChild = false,
  children,
  className,
  font,
  size,
  variant,
  ...props
}: BitButtonProps) {
  const decorations = <ButtonDecorations size={size} variant={variant} />;

  return (
    <ShadcnButton
      {...props}
      className={cn(
        "rounded-none active:translate-y-1 transition-transform relative inline-flex items-center justify-center gap-1.5 border-none",
        size === "icon" && "mx-1 my-0",
        font !== "normal" && "retro",
        className
      )}
      size={size}
      variant={variant}
      asChild={asChild}
    >
      {asChild ? <Slot.Slottable>{children}</Slot.Slottable> : children}
      {decorations}
    </ShadcnButton>
  );
}

export { Button };

components/ui/8bit/card.tsx
import { type VariantProps, cva } from "class-variance-authority";

import { cn } from "@/lib/utils";

import {
  Card as ShadcnCard,
  CardAction as ShadcnCardAction,
  CardContent as ShadcnCardContent,
  CardDescription as ShadcnCardDescription,
  CardFooter as ShadcnCardFooter,
  CardHeader as ShadcnCardHeader,
  CardTitle as ShadcnCardTitle,
} from "@/components/ui/card";

import "@/components/ui/8bit/styles/retro.css";

export const cardVariants = cva("", {
  variants: {
    font: {
      normal: "",
      retro: "retro",
    },
  },
  defaultVariants: {
    font: "retro",
  },
});

export interface BitCardProps
  extends React.ComponentProps<"div">,
    VariantProps<typeof cardVariants> {
  asChild?: boolean;
}

function Card({ className, font, ...props }: BitCardProps) {
  return (
    <div
      className={cn(
        "relative bg-card text-card-foreground border-y-6 border-foreground dark:border-ring p-0!",
        className
      )}
    >
      <ShadcnCard
        {...props}
        className={cn(
          "rounded-none border-0 w-full! h-full flex flex-col bg-card text-card-foreground shadow-none",
          font !== "normal" && "retro",
          className
        )}
      />

      <div
        className={cn("absolute inset-0 border-x-6 -mx-1.5 border-inherit pointer-events-none")}
        aria-hidden="true"
      />
    </div>
  );
}

function CardHeader({ ...props }: BitCardProps) {
  const { className, font } = props;

  return (
    <ShadcnCardHeader
      className={cn(font !== "normal" && "retro", className)}
      {...props}
    />
  );
}

function CardTitle({ ...props }: BitCardProps) {
  const { className, font } = props;

  return (
    <ShadcnCardTitle
      className={cn(font !== "normal" && "retro", className)}
      {...props}
    />
  );
}

function CardDescription({ ...props }: BitCardProps) {
  const { className, font } = props;

  return (
    <ShadcnCardDescription
      className={cn(font !== "normal" && "retro", className)}
      {...props}
    />
  );
}

function CardAction({ ...props }: BitCardProps) {
  const { className, font } = props;

  return (
    <ShadcnCardAction
      className={cn(font !== "normal" && "retro", className)}
      {...props}
    />
  );
}

function CardContent({ ...props }: BitCardProps) {
  const { className, font } = props;

  return (
    <ShadcnCardContent
      className={cn("flex-1", font !== "normal" && "retro", className)}
      {...props}
    />
  );
}

function CardFooter({ ...props }: BitCardProps) {
  const { className, font } = props;

  return (
    <ShadcnCardFooter
      data-slot="card-footer"
      className={cn(font !== "normal" && "retro", className)}
      {...props}
    />
  );
}

export {
  Card,
  CardHeader,
  CardFooter,
  CardTitle,
  CardAction,
  CardDescription,
  CardContent,
};

components/ui/8bit/input.tsx
import { type VariantProps, cva } from "class-variance-authority";

import { cn } from "@/lib/utils";

import { Input as ShadcnInput } from "@/components/ui/input";

import "@/components/ui/8bit/styles/retro.css";

export const inputVariants = cva("", {
  variants: {
    font: {
      normal: "",
      retro: "retro",
    },
  },
  defaultVariants: {
    font: "retro",
  },
});

export interface BitInputProps
  extends React.InputHTMLAttributes<HTMLInputElement>,
    VariantProps<typeof inputVariants> {
  asChild?: boolean;
}

function Input({ ...props }: BitInputProps) {
  const { className, font } = props;

  return (
    <div
      className={cn(
        "relative border-y-6 border-foreground dark:border-ring !p-0 flex items-center",
        className
      )}
    >
      <ShadcnInput
        {...props}
        className={cn(
          "rounded-none ring-0 !w-full",
          font !== "normal" && "retro",
          className
        )}
      />

      <div
        className="absolute inset-0 border-x-6 -mx-1.5 border-foreground dark:border-ring pointer-events-none"
        aria-hidden="true"
      />
    </div>
  );
}

export { Input };

demo.tsx
"use client";

import Advanced3 from "@/components/ui/8bit-advanced3";

export default function Default() {
  return (
    <div className="dark flex w-full min-h-screen items-center justify-center bg-background p-8 retro">
      <Advanced3 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install tailwindcss tw-animate-css
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge button card input
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
