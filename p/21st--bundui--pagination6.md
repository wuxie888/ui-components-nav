<!-- Pagination · @bundui · https://21st.dev/@bundui/components/pagination6
     license: no-license · category: pagination
     A pagination control with previous/next buttons, numbered page links and truncating ellipsis for navigating multi-page content. -->

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
import { usePagination } from "@/hooks/use-pagination";
import {
  Pagination,
  PaginationContent,
  PaginationEllipsis,
  PaginationItem,
  PaginationLink,
  PaginationNext,
  PaginationPrevious
} from "@/components/ui/pagination";

type PaginationProps = {
  currentPage: number;
  totalPages: number;
  paginationItemsToDisplay?: number;
};

export default function PaginationComponent() {
  return <PaginationRender currentPage={1} totalPages={10} paginationItemsToDisplay={5} />;
}

function PaginationRender({ currentPage, totalPages, paginationItemsToDisplay }: PaginationProps) {
  const { pages, showLeftEllipsis, showRightEllipsis } = usePagination({
    currentPage,
    paginationItemsToDisplay: paginationItemsToDisplay ?? 5,
    totalPages
  });

  return (
    <div className="w-full">
      <Pagination className="w-full">
        <PaginationContent>
          {/* Previous page button */}
          <PaginationItem>
            <PaginationPrevious
              aria-disabled={currentPage === 1 ? true : undefined}
              className="aria-disabled:pointer-events-none aria-disabled:opacity-50"
              href={currentPage === 1 ? undefined : `#/page/${currentPage - 1}`}
              role={currentPage === 1 ? "link" : undefined}
            />
          </PaginationItem>

          {/* Left ellipsis (...) */}
          {showLeftEllipsis && (
            <PaginationItem>
              <PaginationEllipsis />
            </PaginationItem>
          )}

          {/* Page number links */}
          {pages.map((page) => (
            <PaginationItem key={page}>
              <PaginationLink href={`#/page/${page}`} isActive={page === currentPage}>
                {page}
              </PaginationLink>
            </PaginationItem>
          ))}

          {/* Right ellipsis (...) */}
          {showRightEllipsis && (
            <PaginationItem>
              <PaginationEllipsis />
            </PaginationItem>
          )}

          {/* Next page button */}
          <PaginationItem>
            <PaginationNext
              aria-disabled={currentPage === totalPages ? true : undefined}
              className="aria-disabled:pointer-events-none aria-disabled:opacity-50"
              href={currentPage === totalPages ? undefined : `#/page/${currentPage + 1}`}
              role={currentPage === totalPages ? "link" : undefined}
            />
          </PaginationItem>
        </PaginationContent>
      </Pagination>
    </div>
  );
}

demo.tsx
import PaginationComponent from "@/components/ui/pagination6";

export default function Default() {
  return (
    <div className="flex w-full max-w-md items-center justify-center">
      <PaginationComponent />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add pagination use-pagination use-pagination?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068&publisher_install_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdXJwb3NlIjoicHVibGlzaGVyLXJlZ2lzdHJ5LWluc3RhbGwiLCJpYXQiOjE3ODcxNzQ1NTQsImV4cCI6MTc4NzE3NTE1NH0.oUbxhA82INZOnoXp8CQGcHvNbk62YF1kT69jOslBHGQ
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
