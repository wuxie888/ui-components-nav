<!-- Pill Breadcrumb · @shadcnspace · https://21st.dev/@shadcnspace/components/breadcrumb-01
     license: MIT · category: navigation-menu
     A compact breadcrumb navigation with a rounded pill container, a home icon, and chevron separators for showing the current page path. -->

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
components/shadcn-space/breadcrumb/breadcrumb-01.tsx
import { HomeIcon } from 'lucide-react'
import { Breadcrumb,BreadcrumbItem,BreadcrumbLink,BreadcrumbList,BreadcrumbPage,BreadcrumbSeparator} from '@/components/ui/breadcrumb'

const BreadcrumbOutlineDemo = () => {
  return (
    <Breadcrumb>
      <BreadcrumbList className='h-8 gap-2 rounded-full border px-3 text-sm'>
        <BreadcrumbItem>
          <BreadcrumbLink href='#'>
            <HomeIcon className='size-4' />
            <span className='sr-only'>Home</span>
          </BreadcrumbLink>
        </BreadcrumbItem>
        <BreadcrumbSeparator />
        <BreadcrumbItem>
          <BreadcrumbLink href='#'>Profile</BreadcrumbLink>
        </BreadcrumbItem>
        <BreadcrumbSeparator />
        <BreadcrumbItem>
          <BreadcrumbPage>Settings</BreadcrumbPage>
        </BreadcrumbItem>
      </BreadcrumbList>
    </Breadcrumb>
  )
}

export default BreadcrumbOutlineDemo

demo.tsx
import BreadcrumbOutlineDemo from "@/components/ui/breadcrumb-01";

export default function Default() {
  return (
    <div className="flex min-h-[240px] w-full items-center justify-center bg-background p-8 text-foreground">
      <BreadcrumbOutlineDemo />
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
