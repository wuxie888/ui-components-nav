<!-- Use Mobile · @shadcn · https://21st.dev/@shadcn/components/use-mobile
     license: MIT · category: icon
     use-mobile hook -->

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
components/ui/use-mobile.ts
import * as React from "react"

const MOBILE_BREAKPOINT = 768

export function useIsMobile() {
  const [isMobile, setIsMobile] = React.useState<boolean | undefined>(undefined)

  React.useEffect(() => {
    const mql = window.matchMedia(`(max-width: ${MOBILE_BREAKPOINT - 1}px)`)
    const onChange = () => {
      setIsMobile(window.innerWidth < MOBILE_BREAKPOINT)
    }
    mql.addEventListener("change", onChange)
    setIsMobile(window.innerWidth < MOBILE_BREAKPOINT)
    return () => mql.removeEventListener("change", onChange)
  }, [])

  return !!isMobile
}

demo.tsx
import { useIsMobile }  from "@/hooks/use-mobile"
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"
import { Smartphone, Monitor } from "lucide-react"

export function MobileDemo() {
  const isMobile = useIsMobile()

  return (
    <Card className="w-[300px] mx-auto mt-4">
      <CardHeader>
        <CardTitle>Device Detection Demo</CardTitle>
      </CardHeader>
      <CardContent className="flex flex-col items-center gap-4">
        {isMobile ? (
          <div className="flex items-center gap-2">
            <Smartphone className="w-5 h-5" />
            <span>Mobile View</span>
          </div>
        ) : (
          <div className="flex items-center gap-2">
            <Monitor className="w-5 h-5" />
            <span>Desktop View</span>
          </div>
        )}
      </CardContent>
    </Card>
  )
}
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
