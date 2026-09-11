<!-- CTA Section · @shadcnstore · https://21st.dev/@shadcnstore/components/cta-section-3
     license: MIT · category: hero
     A call-to-action marketing section with a headline, badge, action buttons, and resource cards for documentation and video tutorials. -->

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
components/ui/page.tsx
import { CtaSection3 } from "@/components/blocks/marketing/cta/cta-section-3/components/cta-section-3"

export default function Page() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center">
      <div className="w-full">
        <CtaSection3 />
      </div>
    </div>
  )
}

components/ui/cta-section-3.tsx
import { Button } from '@/components/ui/button'
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'
import { Badge } from '@/components/ui/badge'
import { Zap, BookOpen, Rocket, ArrowRight, PlayCircle } from 'lucide-react'

export function CtaSection3() {
  return (
    <section className='py-8 lg:py-16'>
      <div className='mx-auto w-full max-w-7xl px-4 sm:px-6 lg:px-8'>
        <div>
          <Card className='bg-card overflow-hidden border py-8 lg:py-12'>
            <CardContent className='gap-0 px-6 lg:px-12'>
              <div className='grid gap-12 lg:grid-cols-2'>
                {/* Left Content */}
                <div className='flex flex-col justify-center'>
                  <div className='flex flex-col gap-6'>
                    <Badge variant='default' className="h-auto px-2.5 py-0.5 font-semibold bg-primary rounded-md text-primary-foreground w-fit">
                      <Rocket className='me-2 size-3' />
                      Launch Your Success
                    </Badge>

                    <div className='flex flex-col gap-4'>
                      <h2 className='text-3xl font-bold tracking-tight text-balance lg:text-4xl'>
                        Accelerate your digital transformation journey
                      </h2>
                      <p className='text-muted-foreground lg:text-lg'>
                        Join thousands of innovative companies using our cutting-edge platform to automate workflows,
                        boost team productivity, and deliver exceptional results faster than ever before.
                      </p>
                    </div>

                    <div className='flex flex-col gap-3 sm:flex-row'>
                      <Button size='lg' className='h-10 cursor-pointer px-8'>
                        <Zap data-icon='inline-start' />
                        Start Free Trial
                      </Button>
                      <Button variant='outline' size='lg' className='h-10 cursor-pointer px-8'>
                        Watch Demo
                      </Button>
                    </div>
                  </div>
                </div>

                {/* Right Content */}
                <div className='flex flex-col gap-6'>
                  <Card className='group border-border cursor-pointer gap-2 py-6'>
                    <CardHeader className='px-6'>
                      <div className='flex items-center justify-between'>
                        <div className='flex items-center gap-3'>
                          <div className='bg-primary/10 flex size-10 items-center justify-center rounded-lg'>
                            <BookOpen className='text-primary size-5' />
                          </div>
                          <CardTitle className='text-base text-balance'>Documentation</CardTitle>
                        </div>
                        <ArrowRight className='text-muted-foreground size-4 transition-transform group-hover:translate-x-1' />
                      </div>
                    </CardHeader>
                    <CardContent className='px-6'>
                      <p className='text-muted-foreground text-sm'>
                        Complete guides, API references, and best practices to get you started.
                      </p>
                    </CardContent>
                  </Card>

                  <Card className='group border-border cursor-pointer gap-2 py-6'>
                    <CardHeader className='px-6'>
                      <div className='flex items-center justify-between'>
                        <div className='flex items-center gap-3'>
                          <div className='bg-primary/10 flex size-10 items-center justify-center rounded-lg'>
                            <PlayCircle className='text-primary size-5' />
                          </div>
                          <CardTitle className='text-base text-balance'>Video Tutorials</CardTitle>
                        </div>
                        <ArrowRight className='text-muted-foreground size-4 transition-transform group-hover:translate-x-1' />
                      </div>
                    </CardHeader>
                    <CardContent className='px-6'>
                      <p className='text-muted-foreground text-sm'>
                        Step-by-step video guides to help you master the platform quickly.
                      </p>
                    </CardContent>
                  </Card>
                </div>
              </div>
            </CardContent>
          </Card>
        </div>
      </div>
    </section>
  )
}

export default CtaSection3

demo.tsx
import CtaSection3 from '@/components/ui/cta-section-3'

export default function Default() {
  return <CtaSection3 />
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge button card
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
