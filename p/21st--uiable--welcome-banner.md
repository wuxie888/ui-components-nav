<!-- Welcome Banner · @uiable · https://21st.dev/@uiable/components/welcome-banner
     license: MIT · category: announcement
     A dashboard hero banner that announces a redesigned interface with a headline, short pitch, download CTA and illustration, used as the first widget on an admin/app homepage. -->

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
components/uiable/widget/welcome-banner.tsx
"use client"

// shadcn
import { buttonVariants } from "@/components/ui/button"
import { Card, CardContent } from "@/components/ui/card"

// project-imports
import branding from "@/branding.json"
import { cn } from "@/lib/utils"

// assets
import { Download } from "lucide-react"

//  ------------------------------ | BLOCK - WELCOME BANNER | ------------------------------  //

export default function WelcomeBanner() {
  return (
    <Card className="welcome-banner relative overflow-hidden border-none bg-chart-4">
      <div
        className="absolute inset-0 z-10 bg-[length:100%] bg-right-bottom bg-no-repeat opacity-50"
        style={{
          backgroundImage: `url(https://cdn.uiable.com/widget/img-dropbox-bg.svg)`,
        }}
      ></div>
      <CardContent className="relative z-20 p-6 md:p-10">
        <div className="grid grid-cols-12 items-center gap-6">
          <div className="col-span-12 sm:col-span-7">
            <div className="space-y-6">
              <h2 className="text-2xl leading-tight font-bold text-white md:text-3xl">
                Explore Redesigned {branding.brandName}
              </h2>
              <p className="text-lg leading-relaxed text-white/80">
                The Brand new User Interface with power of Shadcn Components.
                Explore the Endless possibilities with {branding.brandName}.
              </p>
              <a
                href="https://1.envato.market/zNkqj6"
                className={cn(
                  buttonVariants({ variant: "outline" }),
                  "h-12 rounded-xl border-white bg-transparent px-8 font-bold text-white hover:bg-white hover:text-primary"
                )}
              >
                Download
                <Download className="ml-2 h-5 w-5" />
              </a>
            </div>
          </div>
          <div className="col-span-12 flex justify-center sm:col-span-5">
            <img
              src="https://cdn.uiable.com/og/components-to-complete-interfaces.png"
              alt="Welcome Banner"
              className="w-full max-w-[200px] animate-in drop-shadow-2xl duration-700 fade-in zoom-in"
            />
          </div>
        </div>
      </CardContent>
    </Card>
  )
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button card
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
