<!-- Partition Bar · @8starlabs · https://21st.dev/@8starlabs/components/partition-bar
     license: MIT · category: stat
     A horizontal bar chart that splits into proportional colored segments to visualize parts of a whole, with labeled titles and values under each segment. -->

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
components/ui/partition-bar.tsx
"use client";

import {
  Children,
  createContext,
  HTMLAttributes,
  isValidElement,
  type ReactElement,
  useContext
} from "react";
import { cn } from "@/lib/utils";
import { cva, VariantProps } from "class-variance-authority";

type PartitionBarContextType = {
  total: number;
  size: VariantProps<typeof partitionBarVariants>["size"];
};

const PartitionBarCtxt = createContext<PartitionBarContextType | null>(null);

function usePartitionBarContext(): PartitionBarContextType {
  const context = useContext(PartitionBarCtxt);
  if (!context) {
    throw new Error(
      "usePartitionBarContext must be used within a PartitionBarProvider"
    );
  }
  return context;
}

//////////////////////////////////////////////////////////////////////////////

const partitionBarVariants = cva("flex flex-row", {
  variants: {
    size: {
      sm: "text-xs",
      md: "text-sm",
      lg: "text-md"
    }
  },
  defaultVariants: {
    size: "md"
  }
});

interface PartitionBar
  extends HTMLAttributes<HTMLUListElement>,
    VariantProps<typeof partitionBarVariants> {
  children?:
    | ReactElement<PartitionBarSegment>
    | ReactElement<PartitionBarSegment>[];
  gap?: number;
}

export default function PartitionBar({
  children,
  className,
  gap = 1,
  size,
  ...props
}: PartitionBar) {
  const total = Children.toArray(children).reduce<number>(
    (sum, child) =>
      isValidElement(child)
        ? sum + ((child.props as PartitionBarSegment).num || 0)
        : sum,
    0
  );

  return (
    <PartitionBarCtxt.Provider value={{ total, size }}>
      <ul
        className={cn("w-full", partitionBarVariants({ size }), className)}
        style={{
          gap: `${gap * 4}px`
        }}
        {...props}
      >
        {children}
      </ul>
    </PartitionBarCtxt.Provider>
  );
}

////////////////////////////////////////////////////////////////////////////

const partitionBarLineVariants = cva("", {
  variants: {
    variant: {
      default: "bg-primary",
      secondary: "bg-primary/60",
      destructive: "bg-destructive",
      outline: "border border-input bg-background",
      muted: "bg-primary/40"
    }
  },
  defaultVariants: {
    variant: "default"
  }
});

const partitionBarTitleVariants = cva("", {
  variants: {
    variant: {
      default: "text-primary",
      secondary: "text-primary/60",
      destructive: "text-destructive",
      outline: "text-foreground",
      muted: "text-primary/40"
    }
  },
  defaultVariants: {
    variant: "default"
  }
});

interface PartitionBarSegment
  extends HTMLAttributes<HTMLLIElement>,
    VariantProps<typeof partitionBarLineVariants> {
  children?: React.ReactNode;
  num?: number;
  variant?: VariantProps<typeof partitionBarLineVariants>["variant"];
  alignment?: "left" | "center" | "right";
}

export function PartitionBarSegment({
  children,
  num = 0,
  variant = "default",
  alignment = "center",
  className,
  ...props
}: PartitionBarSegment) {
  const { total, size } = usePartitionBarContext();

  const widthPercent = total > 0 ? (num / total) * 100 : 0;

  return (
    <li
      className="flex flex-col min-w-0"
      style={{
        flexBasis: `${widthPercent}%`,
        flexGrow: 0,
        flexShrink: 0
      }}
      {...props}
    >
      <div
        className={cn(
          partitionBarLineVariants({ variant }),
          "rounded-full w-full shrink-0",
          size === "sm" ? "h-2" : size === "md" ? "h-3" : "h-4",
          className
        )}
      />
      <div
        className={cn(
          partitionBarTitleVariants({ variant }),
          "w-full whitespace-normal flex flex-col",
          size === "sm" ? "mt-2" : size === "md" ? "mt-3" : "mt-4",
          alignment === "left" && "items-start",
          alignment === "center" && "items-center",
          alignment === "right" && "items-end"
        )}
      >
        {children}
      </div>
    </li>
  );
}

/////////////////////////////////////////////////////////////////////////////

interface PartitionBarSegmentTitle extends HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode;
}

export function PartitionBarSegmentTitle({
  children,
  className
}: PartitionBarSegmentTitle) {
  return <div className={cn("w-fit font-semibold", className)}>{children}</div>;
}

interface PartitionBarSegmentValue extends HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode;
}

export function PartitionBarSegmentValue({
  children,
  className
}: PartitionBarSegmentValue) {
  return (
    <div className={cn("w-fit text-slate-500 text-[80%]", className)}>
      {children}
    </div>
  );
}

demo.tsx
import PartitionBar, {
  PartitionBarSegment,
  PartitionBarSegmentTitle,
  PartitionBarSegmentValue,
} from "@/components/ui/partition-bar";

export default function PartitionBarDemo() {
  return (
    <div className="w-full max-w-md">
      <PartitionBar size="md">
        <PartitionBarSegment num={3}>
          <PartitionBarSegmentTitle>Apples</PartitionBarSegmentTitle>
          <PartitionBarSegmentValue>30%</PartitionBarSegmentValue>
        </PartitionBarSegment>

        <PartitionBarSegment num={7} variant="secondary">
          <PartitionBarSegmentTitle>Oranges</PartitionBarSegmentTitle>
          <PartitionBarSegmentValue>70%</PartitionBarSegmentValue>
        </PartitionBarSegment>
      </PartitionBar>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority
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
