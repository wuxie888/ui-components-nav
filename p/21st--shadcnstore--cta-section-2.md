<!-- CTA Section · @shadcnstore · https://21st.dev/@shadcnstore/components/cta-section-2
     license: MIT · category: cta
     A centered call-to-action section with a heading, email subscribe input, and a stack of member avatars with social proof. -->

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
import { CtaSection2 } from "@/components/blocks/marketing/cta/cta-section-2/components/cta-section-2"

export default function Page() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center">
      <div className="w-full">
        <CtaSection2 />
      </div>
    </div>
  )
}

components/ui/cta-section-2.tsx
import { Button } from '@/components/ui/button'
import { Input } from '@/components/ui/input'
import { Card, CardContent } from '@/components/ui/card'
import { Avatar, AvatarImage, AvatarFallback } from '@/components/ui/avatar'
import { AvatarGroup } from '@/components/motion/avatar-group'

export function CtaSection2() {
  return (
    <section className='w-full py-16 sm:py-24'>
      <div className='mx-auto max-w-4xl px-4 sm:px-6 lg:px-8'>
        <Card>
          <CardContent className='flex flex-col gap-6 py-6 sm:py-8'>
            <div className='flex flex-col gap-2 text-center'>
              <h2 className='text-4xl font-bold'>Subscribe to Our Community</h2>
              <p className='text-muted-foreground mx-auto max-w-2xl'>
                Get exclusive access to cutting-edge tech insights, industry trends, and expert advice delivered
                straight to your inbox. Join our growing community today!
              </p>
            </div>

            <div className='mx-auto flex max-sm:w-full flex-col gap-3 sm:flex-row'>
              <Input type='email' className='h-9 max-sm:w-full w-78' placeholder='Enter your email here' />
              <Button className="h-9 px-4 py-2 cursor-pointer">Join Now</Button>
            </div>

            <div className='flex flex-wrap items-center justify-center gap-2'>
              <AvatarGroup className='*:*:ring-background *:*:size-8 *:*:ring-2'>
                <Avatar className='bg-muted'>
                  <AvatarImage src='https://notion-avatars.netlify.app/api/avatar?preset=male-1' alt='User 1' />
                  <AvatarFallback>CN</AvatarFallback>
                </Avatar>
                <Avatar className='bg-muted'>
                  <AvatarImage src='https://notion-avatars.netlify.app/api/avatar?preset=female-5' alt='User 2' />
                  <AvatarFallback>LR</AvatarFallback>
                </Avatar>
                <Avatar className='bg-muted'>
                  <AvatarImage src='https://notion-avatars.netlify.app/api/avatar?preset=male-2' alt='User 3' />
                  <AvatarFallback>ER</AvatarFallback>
                </Avatar>
                <Avatar className='bg-muted'>
                  <AvatarImage src='https://notion-avatars.netlify.app/api/avatar?preset=female-3' alt='User 4' />
                  <AvatarFallback>DG</AvatarFallback>
                </Avatar>
              </AvatarGroup>
              <span className='text-sm'>5,000+ happy members</span>
            </div>
          </CardContent>
        </Card>
      </div>
    </section>
  )
}

export default CtaSection2

demo.tsx
import CtaSection2 from '@/components/ui/cta-section-2'

export default function Demo() {
  return <CtaSection2 />
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar button card input motion-avatar-group
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
