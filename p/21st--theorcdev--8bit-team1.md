<!-- 8bit Team Grid · @theorcdev · https://21st.dev/@theorcdev/components/8bit-team1
     license: MIT · category: team
     An 8-bit styled team grid block. Responsive card grid of team members with pixel avatars, name, role, and an optional corner badge. Built from retro avatar, badge, and card primitives. -->

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
components/ui/8bit/blocks/team1.tsx
import { cn } from "@/lib/utils";

import {
  Avatar,
  AvatarFallback,
  AvatarImage,
} from "@/components/ui/8bit/avatar";
import { Badge } from "@/components/ui/8bit/badge";
import {
  Card,
  CardContent,
} from "@/components/ui/8bit/card";

import "@/components/ui/8bit/styles/retro.css";

export interface TeamMember {
  avatar?: string;
  badge?: string;
  initials: string;
  name: string;
  role: string;
}

interface Team1Props {
  className?: string;
  description?: string;
  members?: TeamMember[];
  title?: string;
}

const defaultMembers: TeamMember[] = [
  { initials: "OD", name: "OrcDev", role: "Founder / Lead Dev", badge: "ADMIN" },
  { initials: "PK", name: "PixelKnight", role: "Core Contributor" },
  { initials: "CM", name: "CodeMage", role: "Component Engineer" },
  { initials: "RR", name: "RetroRogue", role: "Design Lead" },
  { initials: "DM", name: "DungeonMaster", role: "Documentation" },
  { initials: "SS", name: "SwordSmith", role: "Community Manager", badge: "MOD" },
];

export default function Team1({
  title = "The Guild",
  description = "The people behind the pixels",
  members = defaultMembers,
  className,
}: Team1Props) {
  return (
    <section className={cn("w-full px-4 py-16", className)}>
      <div className="mx-auto max-w-4xl">
        {(title || description) && (
          <div className="mb-10 text-center">
            {title && (
              <h2 className="retro mb-3 font-bold text-2xl tracking-tight md:text-3xl">
                {title}
              </h2>
            )}
            {description && (
              <p className="retro text-muted-foreground text-[9px]">{description}</p>
            )}
          </div>
        )}

        <div className="grid gap-x-4 gap-y-1 sm:grid-cols-2 lg:grid-cols-3">
          {members.map((member) => (
            <Card className="relative" key={member.name}>
              {member.badge && (
                <div className="absolute top-2 right-4 z-10">
                  <Badge className="text-[7px]">{member.badge}</Badge>
                </div>
              )}
              <CardContent className="flex items-center gap-4">
                <Avatar>
                  {member.avatar && (
                    <AvatarImage alt={member.name} src={member.avatar} />
                  )}
                  <AvatarFallback>{member.initials}</AvatarFallback>
                </Avatar>
                <div>
                  <p className="retro font-bold text-xs">{member.name}</p>
                  <p className="retro text-muted-foreground text-[10px]">
                    {member.role}
                  </p>
                </div>
              </CardContent>
            </Card>
          ))}
        </div>
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

components/ui/8bit/avatar.tsx
import type React from "react";
import { forwardRef } from "react";

import * as AvatarPrimitive from "@radix-ui/react-avatar";
import { cva } from "class-variance-authority";

import { cn } from "@/lib/utils";

import "@/components/ui/8bit/styles/retro.css";

export const avatarVariants = cva("", {
  variants: {
    font: {
      normal: "",
      retro: "retro",
    },
    variant: {
      default: "",
      retro: "",
      pixel: "",
    },
  },
  defaultVariants: {
    font: "retro",
    variant: "pixel",
  },
});

const Avatar = forwardRef<
  React.ComponentRef<typeof AvatarPrimitive.Root>,
  React.ComponentPropsWithoutRef<typeof AvatarPrimitive.Root> & {
    font?: "normal" | "retro";
    variant?: "default" | "retro" | "pixel";
  }
>(({ className = "", font, variant = "pixel", ...props }, ref) => {
  const isPixel = variant === "pixel";

  return (
    <div className={cn("relative size-max", className)}>
      {/* Pixel frame (only show if pixel variant) */}
      {isPixel && (
        <div
          className="absolute inset-0 w-full h-full pointer-events-none"
          style={{ zIndex: 10 }}
        >
          {/* Top section - Row 1 */}
          <div className="absolute top-0 left-[23%] right-[23%] h-[7%] bg-foreground dark:bg-ring"></div>

          {/* Top section - Row 2 */}
          <div className="absolute top-[6.25%] left-[17%] right-[17%] h-[7%] bg-foreground dark:bg-ring"></div>

          {/* Top section - Row 3 */}
          <div className="absolute top-[12.5%] left-[11%] h-[7%] bg-foreground dark:bg-ring w-[20%]"></div>
          <div className="absolute top-[12.5%] right-[11%] h-[7%] bg-foreground dark:bg-ring w-[20%]"></div>

          {/* Top section - Row 4 */}
          <div className="absolute top-[18.75%] left-[5%] w-[20%] h-[7%] bg-foreground dark:bg-ring"></div>
          <div className="absolute top-[18.75%] right-[5%] w-[20%] h-[7%] bg-foreground dark:bg-ring"></div>

          {/* Top section - Row 5 */}
          <div className="absolute top-[25%] left-0 w-[20%] h-[7%] bg-foreground dark:bg-ring"></div>
          <div className="absolute top-[25%] right-0 w-[20%] h-[7%] bg-foreground dark:bg-ring"></div>

          {/* Top section - Rows 6-7 */}
          <div className="absolute top-[31.25%] left-0 w-[13.5%] h-[13%] bg-foreground dark:bg-ring"></div>
          <div className="absolute top-[31.25%] right-0 w-[13.5%] h-[13%] bg-foreground dark:bg-ring"></div>

          {/* Top section - Rows 8-10 */}
          <div className="absolute top-[43.75%] left-0 w-[13.5%] h-[7%] bg-foreground dark:bg-ring"></div>
          <div className="absolute top-[43.75%] right-0 w-[13.5%] h-[7%] bg-foreground dark:bg-ring"></div>

          {/* Bottom section - Rows 8-10 (mirror) */}
          <div className="absolute top-[50%] left-0 w-[13.5%] h-[7%] bg-foreground dark:bg-ring"></div>
          <div className="absolute top-[50%] right-0 w-[13.5%] h-[7%] bg-foreground dark:bg-ring"></div>

          {/* Bottom section - Rows 6-7 (mirror) */}
          <div className="absolute top-[56.25%] left-0 w-[13.5%] h-[13%] bg-foreground dark:bg-ring"></div>
          <div className="absolute top-[56.25%] right-0 w-[13.5%] h-[13%] bg-foreground dark:bg-ring"></div>

          {/* Bottom section - Row 5 (mirror) */}
          <div className="absolute top-[68.75%] left-0 w-[20%] h-[7%] bg-foreground dark:bg-ring"></div>
          <div className="absolute top-[68.75%] right-0 w-[20%] h-[7%] bg-foreground dark:bg-ring"></div>

          {/* Bottom section - Row 4 (mirror) */}
          <div className="absolute top-[75%] left-[5%] w-[20%] h-[7%] bg-foreground dark:bg-ring"></div>
          <div className="absolute top-[75%] right-[5%] w-[20%] h-[7%] bg-foreground dark:bg-ring"></div>

          {/* Bottom section - Row 3 (mirror) */}
          <div className="absolute top-[81.25%] left-[11%] h-[7%] bg-foreground dark:bg-ring w-[20%]"></div>
          <div className="absolute top-[81.25%] right-[11%] h-[7%] bg-foreground dark:bg-ring w-[20%]"></div>

          {/* Bottom section - Row 2 (mirror) */}
          <div className="absolute top-[87.5%] left-[17%] right-[17%] h-[7%] bg-foreground dark:bg-ring"></div>

          {/* Bottom section - Row 1 (mirror) */}
          <div className="absolute bottom-0 left-[23%] right-[23%] h-[7%] bg-foreground dark:bg-ring"></div>
        </div>
      )}

      <AvatarPrimitive.Root
        ref={ref}
        data-slot="avatar"
        className={cn(
          "relative flex size-10 shrink-0 overflow-hidden text-xs",
          !isPixel && "rounded-none",
          isPixel && "rounded-full",
          font !== "normal" && "retro",
          variant === "retro" && "image-rendering-pixelated",
          className
        )}
        {...props}
      />

      {/* Original border styling (only show if not pixel variant) */}
      {!isPixel && (
        <>
          <div className="absolute top-0 left-0 w-full h-1.5 bg-foreground dark:bg-ring pointer-events-none" />
          <div className="absolute bottom-0 w-full h-1.5 bg-foreground dark:bg-ring pointer-events-none" />
          <div className="absolute top-1.5 -left-1.5 w-1.5 h-1/2 bg-foreground dark:bg-ring pointer-events-none" />
          <div className="absolute bottom-1.5 -left-1.5 w-1.5 h-1/2 bg-foreground dark:bg-ring pointer-events-none" />
          <div className="absolute top-1.5 -right-1.5 w-1.5 h-1/2 bg-foreground dark:bg-ring pointer-events-none" />
          <div className="absolute bottom-1.5 -right-1.5 w-1.5 h-1/2 bg-foreground dark:bg-ring pointer-events-none" />
        </>
      )}
    </div>
  );
});
Avatar.displayName = AvatarPrimitive.Root.displayName;

interface BitAvatarImageProps
  extends React.ComponentPropsWithoutRef<typeof AvatarPrimitive.Image> {
  font?: "normal" | "retro";
  variant?: "default" | "retro" | "pixel";
}

const AvatarImage = forwardRef<
  React.ComponentRef<typeof AvatarPrimitive.Image>,
  BitAvatarImageProps
>(({ className, font, ...props }, ref) => {
  return (
    <AvatarPrimitive.Image
      ref={ref}
      data-slot="avatar-image"
      className={cn(
        "aspect-square h-full w-full",
        font === "retro" && "retro",
        className
      )}
      {...props}
    />
  );
});
AvatarImage.displayName = AvatarPrimitive.Image.displayName;

const AvatarFallback = forwardRef<
  React.ComponentRef<typeof AvatarPrimitive.Fallback>,
  React.ComponentPropsWithoutRef<typeof AvatarPrimitive.Fallback>
>(({ className, ...props }, ref) => (
  <AvatarPrimitive.Fallback
    ref={ref}
    data-slot="avatar-fallback"
    className={cn(
      "flex h-full w-full items-center justify-center rounded-full bg-muted text-foreground",
      className
    )}
    {...props}
  />
));
AvatarFallback.displayName = AvatarPrimitive.Fallback.displayName;

export { Avatar, AvatarImage, AvatarFallback };

demo.tsx
"use client";

import Team1 from "@/components/ui/8bit-team1";

export default function Default() {
  return (
    <div className="flex w-full min-h-screen items-center justify-center bg-background retro">
      <Team1 />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar badge card
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
