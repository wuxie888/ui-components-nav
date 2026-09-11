<!-- Draggable List · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/draggable-list
     license: MIT · category: list
     A reorderable list component with drag and drop functionality. -->

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
components/ui/item.tsx
import { Slot } from "@radix-ui/react-slot";
import { cva, type VariantProps } from "class-variance-authority";
import * as React from "react";
import { cn } from "@/lib/utils";
import { Separator } from "@/registry/new-york/ui/separator";

const ItemGroup = React.forwardRef<HTMLDivElement, React.ComponentProps<"div">>(
  function ItemGroup({ className, ...props }, ref) {
    return (
      <div
        className={cn(
          "group/item-group flex touch-manipulation flex-col",
          className
        )}
        data-slot="item-group"
        ref={ref}
        role="list"
        {...props}
      />
    );
  }
);

function ItemSeparator({
  className,
  ...props
}: React.ComponentProps<typeof Separator>) {
  return (
    <Separator
      className={cn("my-0", className)}
      data-slot="item-separator"
      orientation="horizontal"
      {...props}
    />
  );
}

const itemVariants = cva(
  "group/item flex touch-manipulation flex-wrap items-center rounded-md border border-transparent text-sm outline-none transition-colors focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50 motion-safe:duration-200 [a]:transition-colors [a]:hover:bg-accent/50",
  {
    variants: {
      variant: {
        default: "bg-transparent",
        outline: "border-border",
        muted: "bg-muted/50",
      },
      size: {
        default: "gap-4 p-4",
        sm: "gap-2.5 px-4 py-3",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  }
);

const Item = React.forwardRef<
  HTMLDivElement,
  React.ComponentProps<"div"> &
    VariantProps<typeof itemVariants> & { asChild?: boolean }
>(function Item(
  {
    className,
    variant = "default",
    size = "default",
    asChild = false,
    ...props
  },
  ref
) {
  const Comp = asChild ? Slot : "div";
  return (
    <Comp
      className={cn(itemVariants({ variant, size }), className)}
      data-size={size}
      data-slot="item"
      data-variant={variant}
      ref={ref}
      role="listitem"
      {...props}
    />
  );
});

const itemMediaVariants = cva(
  "flex shrink-0 items-center justify-center gap-2 group-has-[[data-slot=item-description]]/item:translate-y-0.5 group-has-[[data-slot=item-description]]/item:self-start [&_svg]:pointer-events-none",
  {
    variants: {
      variant: {
        default: "bg-transparent",
        icon: "size-8 rounded-sm border bg-muted [&_svg:not([class*='size-'])]:size-4",
        image:
          "size-10 overflow-hidden rounded-sm [&_img]:size-full [&_img]:object-cover",
      },
    },
    defaultVariants: {
      variant: "default",
    },
  }
);

const ItemMedia = React.forwardRef<
  HTMLDivElement,
  React.ComponentProps<"div"> & VariantProps<typeof itemMediaVariants>
>(function ItemMedia({ className, variant = "default", ...props }, ref) {
  return (
    <div
      className={cn(itemMediaVariants({ variant }), className)}
      data-slot="item-media"
      data-variant={variant}
      ref={ref}
      {...props}
    />
  );
});

const ItemContent = React.forwardRef<
  HTMLDivElement,
  React.ComponentProps<"div">
>(function ItemContent({ className, ...props }, ref) {
  return (
    <div
      className={cn(
        "flex flex-1 flex-col gap-1 [&+[data-slot=item-content]]:flex-none",
        className
      )}
      data-slot="item-content"
      ref={ref}
      {...props}
    />
  );
});

const ItemTitle = React.forwardRef<HTMLDivElement, React.ComponentProps<"div">>(
  function ItemTitle({ className, ...props }, ref) {
    return (
      <div
        className={cn(
          "flex w-fit items-center gap-2 font-medium text-sm leading-snug",
          className
        )}
        data-slot="item-title"
        ref={ref}
        {...props}
      />
    );
  }
);

const ItemDescription = React.forwardRef<
  HTMLParagraphElement,
  React.ComponentProps<"p">
>(function ItemDescription({ className, ...props }, ref) {
  return (
    <p
      className={cn(
        "line-clamp-2 text-balance font-normal text-muted-foreground text-sm tabular-nums leading-normal",
        "[&>a:hover]:text-primary [&>a]:underline [&>a]:underline-offset-4",
        className
      )}
      data-slot="item-description"
      ref={ref}
      {...props}
    />
  );
});

const ItemActions = React.forwardRef<
  HTMLDivElement,
  React.ComponentProps<"div">
>(function ItemActions({ className, ...props }, ref) {
  return (
    <div
      className={cn("flex items-center gap-2", className)}
      data-slot="item-actions"
      ref={ref}
      {...props}
    />
  );
});

const ItemHeader = React.forwardRef<
  HTMLDivElement,
  React.ComponentProps<"div">
>(function ItemHeader({ className, ...props }, ref) {
  return (
    <div
      className={cn(
        "flex basis-full items-center justify-between gap-2",
        className
      )}
      data-slot="item-header"
      ref={ref}
      {...props}
    />
  );
});

const ItemFooter = React.forwardRef<
  HTMLDivElement,
  React.ComponentProps<"div">
>(function ItemFooter({ className, ...props }, ref) {
  return (
    <div
      className={cn(
        "flex basis-full items-center justify-between gap-2",
        className
      )}
      data-slot="item-footer"
      ref={ref}
      {...props}
    />
  );
});

export {
  Item,
  ItemMedia,
  ItemContent,
  ItemActions,
  ItemGroup,
  ItemSeparator,
  ItemTitle,
  ItemDescription,
  ItemHeader,
  ItemFooter,
};

demo.tsx
import { DraggableList,DraggableItem } from "@/components/ui/draggable-list"
import { useState } from "react";

const Demo = () => {
  const [items, setItems] = useState([
    { id: "1", content: <DraggableItem>First Item</DraggableItem> },
    { id: "2", content: <DraggableItem>Second Item</DraggableItem> },
    { id: "3", content: <DraggableItem>Third Item</DraggableItem> },
  ]);
 
  const handleReorder = (newItems: DraggableItemProps[]) => {
    setItems(newItems);
    // Do something with the new order
  };
 
  return (
    <DraggableList
      items={items}
      onChange={handleReorder}
      className="max-w-sm w-full"
    />
  );
}

export {Demo}
```

Install NPM dependencies:
```bash
npm install clsx motion tailwind-merge
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add separator
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
