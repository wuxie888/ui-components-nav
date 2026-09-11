<!-- Not Found Page · @pulkitxm · https://21st.dev/@pulkitxm/components/not-found-page
     license: MIT · category: empty-state
     Animated 404 not found page with a subtly moving icon, customizable copy, and reduced-motion support. -->

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
components/ui/not-found-page.tsx
"use client";

import { motion, useReducedMotion } from "framer-motion";
import { ArrowLeft, Frown } from "lucide-react";
import Link from "next/link";
import { cn } from "@/lib/utils";

interface NotFoundPageProps {
  className?: string;
  homeHref?: string;
  title?: string;
  description?: string;
  helperText?: string;
  backLabel?: string;
  icon?: React.ReactNode;
  buttonClassName?: string;
}

export function NotFoundPage({
  className,
  homeHref = "/",
  title = "404",
  description = "Oops! Page not found",
  helperText = "The page you are looking for might have been removed, had its name changed, or is temporarily unavailable.",
  backLabel = "Back to Home",
  icon,
  buttonClassName,
}: NotFoundPageProps) {
  const shouldReduceMotion = useReducedMotion();

  return (
    <motion.div
      initial={{ opacity: shouldReduceMotion ? 1 : 0, y: shouldReduceMotion ? 0 : 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={shouldReduceMotion ? { duration: 0 } : { duration: 0.5 }}
      className={cn("flex min-h-[60svh] flex-col items-center justify-center space-y-6 text-center", className)}
    >
      <motion.div
        animate={shouldReduceMotion ? { rotate: 0 } : { rotate: [0, 5, -5, 0] }}
        transition={
          shouldReduceMotion
            ? { duration: 0 }
            : {
                duration: 2,
                ease: "easeInOut",
                repeat: Number.POSITIVE_INFINITY,
              }
        }
        className="inline-block"
      >
        {icon ?? <Frown className="mx-auto h-24 w-24 text-muted-foreground" />}
      </motion.div>
      <h1 className="font-bold text-4xl text-foreground">{title}</h1>
      <p className="text-muted-foreground text-xl">{description}</p>
      <p className="mx-auto max-w-md text-muted-foreground">{helperText}</p>
      <Link
        href={homeHref}
        className={cn(
          "mt-4 inline-flex items-center rounded-md bg-primary px-4 py-2 font-medium text-primary-foreground text-sm transition-colors hover:bg-primary/90",
          buttonClassName,
        )}
      >
        <ArrowLeft className="mr-2 h-4 w-4" />
        {backLabel}
      </Link>
    </motion.div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion lucide-react
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
