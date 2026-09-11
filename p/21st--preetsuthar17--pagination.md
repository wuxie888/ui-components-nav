<!-- Pagination · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/pagination
     license: unspecified · category: pagination
     Navigation component for splitting content across multiple pages with previous/next controls and page numbers -->

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
components/ui/pagination.tsx
import {
  ChevronLeftIcon,
  ChevronRightIcon,
  MoreHorizontalIcon,
} from "lucide-react";
import * as React from "react";
import { cn } from "@/lib/utils";
import { type Button, buttonVariants } from "@/registry/new-york/ui/button";

function Pagination({ className, ...props }: React.ComponentProps<"nav">) {
  return (
    <nav
      aria-label="pagination"
      className={cn("mx-auto flex w-full justify-center", className)}
      data-slot="pagination"
      role="navigation"
      {...props}
    />
  );
}

function PaginationContent({
  className,
  ...props
}: React.ComponentProps<"ul">) {
  return (
    <ul
      className={cn("flex flex-row items-center gap-1", className)}
      data-slot="pagination-content"
      {...props}
    />
  );
}

function PaginationItem({ ...props }: React.ComponentProps<"li">) {
  return <li data-slot="pagination-item" {...props} />;
}

type PaginationLinkProps = {
  isActive?: boolean;
} & Pick<React.ComponentProps<typeof Button>, "size"> &
  React.ComponentProps<"a">;

const PaginationLink = React.forwardRef<HTMLAnchorElement, PaginationLinkProps>(
  function PaginationLink(
    { className, isActive, size = "icon", ...props },
    ref
  ) {
    return (
      <a
        aria-current={isActive ? "page" : undefined}
        aria-disabled={(props as any).disabled ? true : undefined}
        className={cn(
          buttonVariants({ variant: isActive ? "outline" : "ghost", size }),
          "touch-manipulation tabular-nums",
          className
        )}
        data-active={isActive}
        data-slot="pagination-link"
        ref={ref}
        {...props}
      />
    );
  }
);

function PaginationPrevious({
  className,
  ...props
}: React.ComponentProps<typeof PaginationLink>) {
  return (
    <PaginationLink
      aria-label="Go to previous page"
      className={cn("gap-1 px-2.5 sm:pl-2.5", className)}
      rel={(props as any).rel ?? "prev"}
      {...props}
      size={props.size ?? "default"}
    >
      <ChevronLeftIcon aria-hidden="true" />
      <span className="hidden sm:block">Previous</span>
    </PaginationLink>
  );
}

function PaginationNext({
  className,
  ...props
}: React.ComponentProps<typeof PaginationLink>) {
  return (
    <PaginationLink
      aria-label="Go to next page"
      className={cn("gap-1 px-2.5 sm:pr-2.5", className)}
      rel={(props as any).rel ?? "next"}
      {...props}
      size={props.size ?? "default"}
    >
      <span className="hidden sm:block">Next</span>
      <ChevronRightIcon aria-hidden="true" />
    </PaginationLink>
  );
}

function PaginationEllipsis({
  className,
  ...props
}: React.ComponentProps<"span">) {
  return (
    <span
      aria-hidden
      className={cn("flex size-9 items-center justify-center", className)}
      data-slot="pagination-ellipsis"
      {...props}
    >
      <MoreHorizontalIcon className="size-4" />
      <span className="sr-only">More pages</span>
    </span>
  );
}

export {
  Pagination,
  PaginationContent,
  PaginationLink,
  PaginationItem,
  PaginationPrevious,
  PaginationNext,
  PaginationEllipsis,
};

demo.tsx
import { Pagination, PaginationPrevious, PaginationItem, PaginationNext, PaginationEllipsis } from "@/components/ui/pagination";
import React, { useState, useEffect } from "react";

function useMediaQuery(query) {
  const [matches, setMatches] = useState(false);

  useEffect(() => {
    const media = window.matchMedia(query);
    if (media.matches !== matches) {
      setMatches(media.matches);
    }
    const listener = () => setMatches(media.matches);
    media.addListener(listener);
    return () => media.removeListener(listener);
  }, [matches, query]);

  return matches;
}

export default function DemoOne() { 
  const [currentPage, setCurrentPage] = React.useState(5);

  const totalPages = 20;
  const isMobile = useMediaQuery("(max-width: 640px)");
  const isTablet = useMediaQuery("(max-width: 768px)");     
  
  const getVisiblePages = () => {

    const delta = isMobile ? 1 : isTablet ? 1 : 2;
    const rangeWithDots = [];
    if (totalPages <= 7) {
      return Array.from({ length: totalPages }, (_, i) => i + 1);
    }
    rangeWithDots.push(1);
    
    let startPage = Math.max(2, currentPage - delta);
    let endPage = Math.min(totalPages - 1, currentPage + delta);

    if (currentPage === 1) {
      endPage = Math.min(totalPages - 1, 1 + (delta * 2));
    } else if (currentPage === totalPages) {
      startPage = Math.max(2, totalPages - (delta * 2));
    } else {
      startPage = Math.max(2, Math.min(startPage, currentPage));
      endPage = Math.min(totalPages - 1, Math.max(endPage, currentPage));
    }

    if (startPage > 2) {
      rangeWithDots.push("...");
    }

    for (let i = startPage; i <= endPage; i++) {
      if (i !== 1 && i !== totalPages) {
        rangeWithDots.push(i);
      }
    }

    if (endPage < totalPages - 1) {
      rangeWithDots.push("...");
    }

    if (totalPages > 1) {
      rangeWithDots.push(totalPages);
    }
    return rangeWithDots;
  };

  return(
    <>
      <div className="w-full overflow-x-auto">
          <Pagination className="flex-wrap min-w-fit">
            <PaginationPrevious
              onClick={() => setCurrentPage(Math.max(1, currentPage - 1))}
              disabled={currentPage === 1}
              size={isMobile ? "sm" : "default"}
            >
              {isMobile ? "Prev" : "Previous"}
            </PaginationPrevious>
            {getVisiblePages().map((page, index) =>
              page === "..." ? (
                <PaginationEllipsis key={`ellipsis-${index}`} />
              ) : (
                <PaginationItem
                  key={page}
                  isActive={page === currentPage}
                  onClick={() => setCurrentPage(page as number)}
                  size={isMobile ? "sm" : "default"}
                >
                  {page}
                </PaginationItem>
              ),
            )}
            <PaginationNext
              onClick={() => setCurrentPage(Math.min(totalPages, currentPage + 1))}
              disabled={currentPage === totalPages}
              size={isMobile ? "sm" : "default"}
            >
              {isMobile ? "Next" : "Next"}
            </PaginationNext>
          </Pagination>
        </div>
    </>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
