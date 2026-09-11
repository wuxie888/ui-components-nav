<!-- Pill · @haydenbleasel · https://21st.dev/@haydenbleasel/components/pill
     license: MIT · category: avatar
     Pill UI component. -->

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
import { ChevronDownIcon, ChevronUpIcon, MinusIcon } from "lucide-react";
import type { ComponentProps, ReactNode } from "react";
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";
import { Badge } from "@/components/ui/badge";
import { Button } from "@/components/ui/button";
import { cn } from "@/lib/utils";

export type PillProps = ComponentProps<typeof Badge> & {
  themed?: boolean;
};

export const Pill = ({
  variant = "secondary",
  themed = false,
  className,
  ...props
}: PillProps) => (
  <Badge
    className={cn("gap-2 rounded-full px-3 py-1.5 font-normal", className)}
    variant={variant}
    {...props}
  />
);

export type PillAvatarProps = ComponentProps<typeof AvatarImage> & {
  fallback?: string;
};

export const PillAvatar = ({
  fallback,
  className,
  ...props
}: PillAvatarProps) => (
  <Avatar className={cn("-ml-1 h-4 w-4", className)}>
    <AvatarImage {...props} />
    <AvatarFallback>{fallback}</AvatarFallback>
  </Avatar>
);

export type PillButtonProps = ComponentProps<typeof Button>;

export const PillButton = ({ className, ...props }: PillButtonProps) => (
  <Button
    className={cn(
      "-my-2 -mr-2 size-6 rounded-full p-0.5 hover:bg-foreground/5",
      className
    )}
    size="icon"
    variant="ghost"
    {...props}
  />
);

export type PillStatusProps = {
  children: ReactNode;
  className?: string;
};

export const PillStatus = ({
  children,
  className,
  ...props
}: PillStatusProps) => (
  <div
    className={cn(
      "flex items-center gap-2 border-r pr-2 font-medium",
      className
    )}
    {...props}
  >
    {children}
  </div>
);

export type PillIndicatorProps = {
  variant?: "success" | "error" | "warning" | "info";
  pulse?: boolean;
};

export const PillIndicator = ({
  variant = "success",
  pulse = false,
}: PillIndicatorProps) => (
  <span className="relative flex size-2">
    {pulse && (
      <span
        className={cn(
          "absolute inline-flex h-full w-full animate-ping rounded-full opacity-75",
          variant === "success" && "bg-emerald-400",
          variant === "error" && "bg-rose-400",
          variant === "warning" && "bg-amber-400",
          variant === "info" && "bg-sky-400"
        )}
      />
    )}
    <span
      className={cn(
        "relative inline-flex size-2 rounded-full",
        variant === "success" && "bg-emerald-500",
        variant === "error" && "bg-rose-500",
        variant === "warning" && "bg-amber-500",
        variant === "info" && "bg-sky-500"
      )}
    />
  </span>
);

export type PillDeltaProps = {
  className?: string;
  delta: number;
};

export const PillDelta = ({ className, delta }: PillDeltaProps) => {
  if (!delta) {
    return (
      <MinusIcon className={cn("size-3 text-muted-foreground", className)} />
    );
  }

  if (delta > 0) {
    return (
      <ChevronUpIcon className={cn("size-3 text-emerald-500", className)} />
    );
  }

  return <ChevronDownIcon className={cn("size-3 text-rose-500", className)} />;
};

export type PillIconProps = {
  icon: typeof ChevronUpIcon;
  className?: string;
};

export const PillIcon = ({
  icon: Icon,
  className,
  ...props
}: PillIconProps) => (
  <Icon
    className={cn("size-3 text-muted-foreground", className)}
    size={12}
    {...props}
  />
);

export type PillAvatarGroupProps = {
  children: ReactNode;
  className?: string;
};

export const PillAvatarGroup = ({
  children,
  className,
  ...props
}: PillAvatarGroupProps) => (
  <div
    className={cn(
      "-space-x-1 flex items-center",
      "[&>*:not(:first-of-type)]:[mask-image:radial-gradient(circle_9px_at_-4px_50%,transparent_99%,white_100%)]",
      className
    )}
    {...props}
  >
    {children}
  </div>
);

demo.tsx
import {
  Pill,
  PillAvatar,
  PillButton,
  PillStatus,
  PillIndicator,
  PillDelta,
  PillIcon,
  PillAvatarGroup,
} from "@/components/ui/pill";
import {
  ArrowUpRightIcon,
  XIcon,
  CheckCircleIcon,
  UsersIcon,
} from "lucide-react";

function Demo() {
  return (
    <div className="flex flex-wrap gap-2 items-center justify-center">
      <Pill>
        <PillAvatar
          src="https://cdn.21st.dev/assets/localized/760921add96cda047fb9d92050b00b3697e3f9436fb8b4276c4303623dad9188.jpg"
          fallback="HB"
        />
        @haydenbleasel
      </Pill>
      <Pill>
        <PillStatus>
          <CheckCircleIcon size={12} className="text-emerald-500" />
          Passed
        </PillStatus>
        Approval Status
      </Pill>
      <Pill>
        #kibo-ui
        <PillButton variant="ghost" size="icon">
          <XIcon size={12} />
        </PillButton>
      </Pill>
      <Pill>
        <PillIndicator variant="success" pulse />
        Active
      </Pill>
      <Pill>
        <PillIndicator variant="error" />
        Error
      </Pill>
      <Pill>
        <PillDelta delta={10} />
        Up 10%
      </Pill>
      <Pill>
        <PillDelta delta={-5} />
        Down 5%
      </Pill>
      <Pill>
        <PillDelta delta={0} />
        No change
      </Pill>
      <Pill>
        <PillIcon icon={UsersIcon} />
        17 users
      </Pill>
      <Pill>
        <PillAvatarGroup>
          <PillAvatar
            src="https://cdn.21st.dev/assets/localized/760921add96cda047fb9d92050b00b3697e3f9436fb8b4276c4303623dad9188.jpg"
            fallback="HB"
          />
          <PillAvatar
            src="https://cdn.21st.dev/assets/mirror/e9/e941f86e0ca2b1c9eefd47618a92040443f85598470bad47399cee7f259c0756.jpg"
            fallback="SC"
          />
          <PillAvatar
            src="https://cdn.21st.dev/assets/mirror/ee/ee27a8a550eaa8658cae673aa17a1bf4f9111dd65a44c0540ffa52a46dee5a65.jpg"
            fallback="LR"
          />
        </PillAvatarGroup>
        Loved by millions
      </Pill>
    </div>
  );
}

export default Demo;
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar badge button
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
