<!-- 3d Wrapper · @badtzx0 · https://21st.dev/@badtzx0/components/3d-wrapper
     license: unspecified · category: form
     Here is 3d Wrapper component -->

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
components/ui/3d-wrapper.tsx
"use client"

import React, { ReactNode, useRef } from "react"
import {
  motion,
  useMotionTemplate,
  useMotionValue,
  useSpring,
} from "motion/react"

import { cn } from "@/lib/utils"

interface Wrapper3DProps {
  children: ReactNode
  damping?: number
  swiftness?: number
  mass?: number
  maxRotation?: number
  translateZ?: number
  perspective?: boolean
  className?: string
}

export function Wrapper3D({
  children,
  damping = 20,
  swiftness = 80,
  mass = 1.5,
  maxRotation = 100,
  translateZ = 75,
  perspective = true,
  className,
}: Wrapper3DProps) {
  const halfMaxRotation = maxRotation / 2

  const refMotionDiv = useRef<HTMLDivElement | null>(null)

  const x = useMotionValue(0)
  const y = useMotionValue(0)

  const xSpring = useSpring(x, {
    damping: damping,
    stiffness: swiftness,
    mass: mass,
  })

  const ySpring = useSpring(y, {
    damping: damping,
    stiffness: swiftness,
    mass: mass,
  })

  const transform = useMotionTemplate`rotateX(${xSpring}deg) rotateY(${ySpring}deg)`

  const handleMouseMove = (e: React.MouseEvent<HTMLDivElement>) => {
    if (!refMotionDiv.current) return

    const rect = refMotionDiv.current.getBoundingClientRect()
    const mouseX = e.clientX - rect.left
    const mouseY = e.clientY - rect.top

    const rX = ((mouseY / rect.height) * maxRotation - halfMaxRotation) * -1
    const rY = (mouseX / rect.width) * maxRotation - halfMaxRotation

    x.set(rX)
    y.set(rY)
  }

  const handleMouseLeave = () => {
    x.set(0)
    y.set(0)
  }

  return (
    <motion.div
      ref={refMotionDiv}
      onMouseMove={handleMouseMove}
      onMouseLeave={handleMouseLeave}
      initial={{ opacity: 0, scale: 0.8 }}
      animate={{ opacity: 1, scale: 1 }}
      transition={{ duration: 1, ease: "easeOut" }}
      style={{
        transformStyle: "preserve-3d",
        transform,
        ...(perspective && { perspective: "1000px" }),
      }}
      className={cn(className)}
    >
      <div
        style={{
          transform: `translateZ(${translateZ}px)`,
          transformStyle: "preserve-3d",
        }}
      >
        {children}
      </div>
    </motion.div>
  )
}

demo.tsx
import { Button } from "@/components/ui/button"
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from "@/components/ui/card"
import { Input } from "@/components/ui/input"
import { Label } from "@/components/ui/label"
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select"
import { Wrapper3D } from "@/components/ui/3d-wrapper"

export default function Wrapper3DDemo() {
  return (
    <div>
      <Wrapper3D maxRotation={20} translateZ={60} perspective={false}>
        <Card className="dark:bg-secondary w-auto bg-white shadow-lg sm:w-[350px]">
          <CardHeader>
            <CardTitle>Create project</CardTitle>
            <CardDescription>
              Deploy your new project in one-click.
            </CardDescription>
          </CardHeader>
          <CardContent>
            <form>
              <div className="grid w-full items-center gap-4">
                <div className="flex flex-col space-y-1.5">
                  <Label htmlFor="name">Name</Label>
                  <Input id="name" placeholder="Name of your project" />
                </div>
                <div className="flex flex-col space-y-1.5">
                  <Label htmlFor="framework">Framework</Label>
                  <Select>
                    <SelectTrigger id="framework">
                      <SelectValue placeholder="Select" />
                    </SelectTrigger>
                    <SelectContent position="popper">
                      <SelectItem value="next">Next.js</SelectItem>
                      <SelectItem value="sveltekit">SvelteKit</SelectItem>
                      <SelectItem value="astro">Astro</SelectItem>
                      <SelectItem value="nuxt">Nuxt.js</SelectItem>
                    </SelectContent>
                  </Select>
                </div>
              </div>
            </form>
          </CardContent>
          <CardFooter className="flex justify-between">
            <Button variant="outline">Cancel</Button>
            <Button>Deploy</Button>
          </CardFooter>
        </Card>
      </Wrapper3D>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install clsx motion tailwind-merge
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button card input label select
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
