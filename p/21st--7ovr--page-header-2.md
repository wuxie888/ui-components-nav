<!-- Page Header with Breadcrumbs · @7ovr · https://21st.dev/@7ovr/components/page-header-2
     license: no-license · category: dashboard
     An application page header with breadcrumbs, a status badge, and action buttons. -->

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
components/ui/page-header-block.tsx
import { Badge } from "@/components/ui/badge"
import {
  Breadcrumb,
  BreadcrumbItem,
  BreadcrumbLink,
  BreadcrumbList,
  BreadcrumbPage,
  BreadcrumbSeparator,
} from "@/components/ui/breadcrumb"
import { Button } from "@/components/ui/button"
import { IconPlaceholder } from "@/components/icons/icon-placeholder"

export default function PageHeaderBlock() {
  return (
    <section className="min-h-svh w-full bg-background px-6 py-12 text-foreground">
      <div className="mx-auto w-full max-w-5xl">
        <Breadcrumb className="mb-4">
          <BreadcrumbList>
            <BreadcrumbItem>
              <BreadcrumbLink render={<a href="#" />}>Home</BreadcrumbLink>
            </BreadcrumbItem>
            <BreadcrumbSeparator />
            <BreadcrumbItem>
              <BreadcrumbLink render={<a href="#" />}>Projects</BreadcrumbLink>
            </BreadcrumbItem>
            <BreadcrumbSeparator />
            <BreadcrumbItem>
              <BreadcrumbPage>Acme App</BreadcrumbPage>
            </BreadcrumbItem>
          </BreadcrumbList>
        </Breadcrumb>

        <div className="flex flex-col gap-4 border-b border-border pb-6 sm:flex-row sm:items-end sm:justify-between">
          <div className="flex items-center gap-3">
            <h1 className="font-heading text-2xl font-bold tracking-tight sm:text-3xl">
              Acme App
            </h1>
            <Badge variant="secondary">Active</Badge>
          </div>

          <div className="flex items-center gap-2">
            <Button variant="outline">
              <IconPlaceholder
                lucide="Download"
                tabler="IconDownload"
                hugeicons="DownloadIcon"
                phosphor="Download"
                remixicon="RiDownloadLine"
                data-icon="inline-start"
                aria-hidden="true"
              />
              Export
            </Button>
            <Button>Save Changes</Button>
          </div>
        </div>

        <div className="mt-6 grid gap-4 sm:grid-cols-3">
          {Array.from({ length: 3 }).map((_, i) => (
            <div
              key={i}
              className="h-28 rounded-lg border border-border bg-muted/30"
            />
          ))}
        </div>
      </div>
    </section>
  )
}

demo.tsx
import PageHeaderBlock from "@/components/ui/page-header-2";

export default function Default() {
  return (
    <div className="w-full [&>section]:min-h-0 [&>section]:px-8 [&>section]:py-10">
      <PageHeaderBlock />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @remixicon/react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge breadcrumb button
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
