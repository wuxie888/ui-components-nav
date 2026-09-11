<!-- Pending · @diceui · https://21st.dev/@diceui/components/pending
     license: MIT · category: sign-in
     A utility component and hook that applies pending/loading state to any interactive element, blocking clicks and keyboard activation with proper ARIA attributes. -->

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
components/pending.tsx
/**
 * Based on React Aria's Button implementation
 * @see https://github.com/adobe/react-spectrum/blob/main/packages/react-aria-components/src/Button.tsx
 */

import { Slot as SlotPrimitive } from "radix-ui";
import * as React from "react";

interface UsePendingOptions {
  id?: string;
  isPending?: boolean;
  disabled?: boolean;
}

interface UsePendingReturn<T extends HTMLElement = HTMLElement> {
  pendingProps: React.HTMLAttributes<T> & {
    "aria-busy"?: "true";
    "aria-disabled"?: "true";
    "data-pending"?: true;
    "data-disabled"?: true;
  };
  isPending: boolean;
}

function usePending<T extends HTMLElement = HTMLElement>(
  options: UsePendingOptions = {},
): UsePendingReturn<T> {
  const { id, isPending = false, disabled = false } = options;

  const instanceId = React.useId();
  const pendingId = id || instanceId;

  const pendingProps = React.useMemo(() => {
    const props: React.HTMLAttributes<T> & {
      "aria-busy"?: "true";
      "aria-disabled"?: "true";
      "data-pending"?: true;
      "data-disabled"?: true;
    } = {
      id: pendingId,
    };

    if (isPending) {
      props["aria-busy"] = "true";
      props["aria-disabled"] = "true";
      props["data-pending"] = true;

      function onEventPrevent(event: React.SyntheticEvent) {
        event.preventDefault();
      }

      function onKeyEventPrevent(event: React.KeyboardEvent<T>) {
        if (event.key === "Enter" || event.key === " ") {
          event.preventDefault();
        }
      }

      props.onClick = onEventPrevent;
      props.onPointerDown = onEventPrevent;
      props.onPointerUp = onEventPrevent;
      props.onMouseDown = onEventPrevent;
      props.onMouseUp = onEventPrevent;
      props.onKeyDown = onKeyEventPrevent;
      props.onKeyUp = onKeyEventPrevent;
    }

    if (disabled) {
      props["data-disabled"] = true;
    }

    return props;
  }, [isPending, disabled, pendingId]);

  return React.useMemo(() => {
    return {
      pendingProps,
      isPending,
    };
  }, [pendingProps, isPending]);
}

interface PendingProps extends React.ComponentProps<typeof SlotPrimitive.Slot> {
  isPending?: boolean;
  disabled?: boolean;
}

function Pending({ id, isPending, disabled, ...props }: PendingProps) {
  const { pendingProps } = usePending({ id, isPending, disabled });

  return <SlotPrimitive.Slot {...props} {...pendingProps} />;
}

export { Pending, usePending };

demo.tsx
"use client";

import { Pending } from "@/components/ui/pending";
import { Loader2 } from "lucide-react";
import * as React from "react";
import { Button } from "@/components/ui/button";

export default function PendingWrapperDemo() {
  const [isSubmitting, setIsSubmitting] = React.useState(false);

  const onSubmit = React.useCallback(() => {
    setIsSubmitting(true);
    // Simulate API call
    setTimeout(() => {
      setIsSubmitting(false);
    }, 2000);
  }, []);

  return (
    <div className="flex flex-col items-center gap-4">
      <Pending isPending={isSubmitting}>
        <Button onClick={onSubmit}>
          {isSubmitting && <Loader2 className="size-4 animate-spin" />}
          {isSubmitting ? "Submitting..." : "Submit with Wrapper"}
        </Button>
      </Pending>

      <p className="text-muted-foreground text-sm">
        Using the <code className="text-xs">{"<Pending>"}</code> wrapper
        component
      </p>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install radix-ui
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button input label switch
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
