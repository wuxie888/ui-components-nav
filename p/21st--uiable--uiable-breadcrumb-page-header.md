<!-- Page Header Breadcrumb · @uiable · https://21st.dev/@uiable/components/uiable-breadcrumb-page-header
     license: MIT · category: card
     A page header block that pairs a breadcrumb trail with the current page title in a rounded, shadowed card. -->

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
components/uiable/breadcrumb/breadcrumb-page-header.tsx
// shadcn
import {
  Breadcrumb,
  BreadcrumbItem,
  BreadcrumbLink,
  BreadcrumbList,
  BreadcrumbPage,
  BreadcrumbSeparator,
} from "@/components/ui/breadcrumb"

//  ------------------------------ | BREADCRUMB - PAGE HEADER | ------------------------------  //

export default function BreadcrumbPageHeader() {
  return (
    <div className="flex flex-col gap-2 rounded-[16px] p-5 shadow-[0_4px_24px_0_rgba(62,57,107,.18)]">
      <Breadcrumb>
        <BreadcrumbList>
          <BreadcrumbItem>
            <BreadcrumbLink href="#">Home</BreadcrumbLink>
          </BreadcrumbItem>
          <BreadcrumbSeparator />
          <BreadcrumbItem>
            <BreadcrumbLink href="/dashboard">Dashboard</BreadcrumbLink>
          </BreadcrumbItem>
          <BreadcrumbSeparator />
          <BreadcrumbItem>
            <BreadcrumbPage>Current Page</BreadcrumbPage>
          </BreadcrumbItem>
        </BreadcrumbList>
      </Breadcrumb>
      <h2 className="tracking-tight text-foreground">Current Page</h2>
    </div>
  )
}

demo.tsx
import BreadcrumbPageHeader from "@/components/ui/uiable-breadcrumb-page-header";

export default function Default() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center bg-background p-8">
      <BreadcrumbPageHeader />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add breadcrumb
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
