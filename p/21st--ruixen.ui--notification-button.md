<!-- Notification Button · @ruixen.ui · https://21st.dev/@ruixen.ui/components/notification-button
     license: unspecified · category: notification
     The Notification Button is a shadcn/ui-based component designed to display real-time alerts or counts for messages, tasks, or cart items. It combines a button with an icon (defaulting to a bell) and a dynamic badge that shows the count, making it highly visible and actionable. The component supports multiple sizes (sm, md, lg) and ensures that the badge scales proportionally with the icon and button, maintaining a clean, modern, and responsive design. Its flexibility allows developers to replace the icon or integrate it into dashboards, navbars, or overlays, making it a practical solution for interactive UIs that require user attention to notifications. -->

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
components/ui/notification-button.tsx
import React from "react";
import { Button } from "@/components/ui/button";
import { Badge } from "@/components/ui/badge";
import { cn } from "@/lib/utils";
import { Bell } from "lucide-react";

/**
 * NotificationButton
 *
 * Button with a badge count, ideal for notifications, messages, cart items, or tasks.
 * Shows an icon (default: Bell) with optional badge count.
 */

interface NotificationButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  count?: number;
  icon?: React.ReactNode;
  size?: "sm" | "md" | "lg";
}

const sizeConfig = {
  sm: { padding: "p-2", icon: "w-4 h-4", badge: "text-xs h-4 min-w-[1rem]" },
  md: { padding: "p-3", icon: "w-5 h-5", badge: "text-sm h-5 min-w-[1.25rem]" },
  lg: { padding: "p-4", icon: "w-6 h-6", badge: "text-sm h-5 min-w-[1.25rem]" },
};

export function NotificationButton({
  count,
  icon,
  size = "md",
  className,
  ...props
}: NotificationButtonProps) {
  const s = sizeConfig[size];

  return (
    <Button
      className={cn(
        "relative inline-flex items-center justify-center rounded-full",
        s.padding,
        className,
      )}
      {...props}
    >
      {/* Icon */}
      {icon ?? <Bell className={cn(s.icon)} />}

      {/* Badge */}
      {count !== undefined && count > 0 && (
        <span className="absolute -top-1 -right-1">
          <Badge className={cn(s.badge, "p-1")}>{count}</Badge>
        </span>
      )}
    </Button>
  );
}

export default NotificationButton;

demo.tsx
import NotificationButton from "@/components/ui/notification-button"

export default function DemoNotificationButton() {
  return (
    <div className="flex gap-4">
      <NotificationButton count={5} />
      <NotificationButton count={12} size="lg" />
      <NotificationButton count={0} />
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
npx shadcn@latest add badge button
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
