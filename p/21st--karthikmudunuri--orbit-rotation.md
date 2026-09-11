<!-- Orbit Rotation · @karthikmudunuri · https://21st.dev/@karthikmudunuri/components/orbit-rotation
     license: MIT · category: icon
     An animated component that rotates customizable tech/brand icons in concentric dotted orbits around a center icon. -->

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
components/ui/orbit-rotation.tsx
"use client"

import type React from "react"
import {
  FaApple,
  FaAws,
  FaDocker,
  FaGithub,
  FaGoogle,
  FaInstagram,
  FaLinkedin,
  FaNodeJs,
  FaReact,
  FaTwitter,
} from "react-icons/fa"
import {
  SiFacebook,
  SiNextdotjs,
  SiRedux,
  SiTypescript,
  SiVercel,
} from "react-icons/si"

import { cn } from "@/lib/utils"

interface OrbitIcon {
  Icon: React.ComponentType<{ className?: string }>
  name?: string
}

interface OrbitRotationProps {
  icons?: OrbitIcon[]
  orbitCount?: number
  orbitGap?: number
  centerIcon?: OrbitIcon
  className?: string
  size?: "sm" | "md" | "lg"
}

const defaultIcons: OrbitIcon[] = [
  { Icon: FaReact, name: "React" },
  { Icon: FaAws, name: "AWS" },
  { Icon: FaDocker, name: "Docker" },
  { Icon: FaNodeJs, name: "Node.js" },
  { Icon: SiNextdotjs, name: "Next.js" },
  { Icon: SiVercel, name: "Vercel" },
  { Icon: SiRedux, name: "Redux" },
  { Icon: SiTypescript, name: "TypeScript" },
  { Icon: FaGithub, name: "GitHub" },
  { Icon: FaTwitter, name: "Twitter" },
  { Icon: FaLinkedin, name: "LinkedIn" },
  { Icon: FaInstagram, name: "Instagram" },
  { Icon: FaGoogle, name: "Google" },
  { Icon: FaApple, name: "Apple" },
  { Icon: SiFacebook, name: "Facebook" },
]

const defaultCenterIcon: OrbitIcon = {
  Icon: FaReact,
  name: "React",
}

export function OrbitRotation({
  icons = defaultIcons,
  orbitCount = 3,
  orbitGap = 6,
  centerIcon = defaultCenterIcon,
  className,
  size = "md",
  ...props
}: OrbitRotationProps) {
  const iconsPerOrbit = Math.ceil(icons.length / orbitCount)

  const sizeClasses = {
    sm: "w-16 h-16",
    md: "w-24 h-24",
    lg: "w-32 h-32",
  }

  const iconSizeClasses = {
    sm: "w-6 h-6",
    md: "w-8 h-8",
    lg: "w-10 h-10",
  }

  return (
    <div
      className={cn("relative flex items-center justify-center", className)}
      {...props}
    >
      <div className="relative flex items-center justify-center">
        {/* Center Icon */}
        <div
          className={cn(
            "bg-background/90 border-border/50 flex items-center justify-center rounded-full border shadow-xl backdrop-blur-sm",
            sizeClasses[size]
          )}
        >
          <centerIcon.Icon className={cn(iconSizeClasses[size])} />
        </div>

        {/* Generate Orbits */}
        {[...Array(orbitCount)].map((_, orbitIdx) => {
          const orbitSize = `${8 + orbitGap * (orbitIdx + 1)}rem`
          const angleStep = (2 * Math.PI) / iconsPerOrbit
          const animationDuration = `${12 + orbitIdx * 6}s`

          return (
            <div
              key={orbitIdx}
              className="border-muted-foreground/30 absolute rounded-full border-2 border-dotted"
              style={{
                width: orbitSize,
                height: orbitSize,
                animation: `orbit-spin ${animationDuration} linear infinite`,
              }}
            >
              {icons
                .slice(
                  orbitIdx * iconsPerOrbit,
                  orbitIdx * iconsPerOrbit + iconsPerOrbit
                )
                .map((iconConfig, iconIdx) => {
                  const angle = iconIdx * angleStep
                  const x = 50 + 50 * Math.cos(angle)
                  const y = 50 + 50 * Math.sin(angle)

                  return (
                    <div
                      key={iconIdx}
                      className="bg-background/80 border-border/50 absolute rounded-full border p-1 shadow-lg backdrop-blur-sm"
                      style={{
                        left: `${x}%`,
                        top: `${y}%`,
                        transform: "translate(-50%, -50%)",
                      }}
                    >
                      <iconConfig.Icon className={cn(iconSizeClasses[size])} />
                    </div>
                  )
                })}
            </div>
          )
        })}
      </div>
    </div>
  )
}

demo.tsx
"use client"

import {
  FaApple,
  FaAws,
  FaDocker,
  FaGithub,
  FaGoogle,
  FaInstagram,
  FaLinkedin,
  FaNodeJs,
  FaReact,
  FaTwitter,
} from "react-icons/fa"
import {
  SiFacebook,
  SiNextdotjs,
  SiRedux,
  SiTypescript,
  SiVercel,
} from "react-icons/si"

import { OrbitRotation } from "@/components/ui/orbit-rotation"

export function OrbitRotationDemo() {
  const techIcons = [
    { Icon: FaReact, name: "React" },
    { Icon: FaAws, name: "AWS" },
    { Icon: FaDocker, name: "Docker" },
    { Icon: FaNodeJs, name: "Node.js" },
    { Icon: SiNextdotjs, name: "Next.js" },
    { Icon: SiVercel, name: "Vercel" },
    { Icon: SiRedux, name: "Redux" },
    { Icon: SiTypescript, name: "TypeScript" },
    { Icon: FaGithub, name: "GitHub" },
    { Icon: FaTwitter, name: "Twitter" },
    { Icon: FaLinkedin, name: "LinkedIn" },
    { Icon: FaInstagram, name: "Instagram" },
    { Icon: FaGoogle, name: "Google" },
    { Icon: FaApple, name: "Apple" },
    { Icon: SiFacebook, name: "Facebook" },
  ]

  const centerIcon = {
    Icon: SiNextdotjs,
    name: "Next.js",
  }

  return (
    <div className="bg-background relative h-[500px] w-full overflow-hidden rounded-lg border p-8">
      <div className="flex h-full items-center justify-center">
        <OrbitRotation
          icons={techIcons}
          orbitCount={3}
          orbitGap={6}
          centerIcon={centerIcon}
          size="md"
        />
      </div>
    </div>
  )
}

export default OrbitRotationDemo
```

Install NPM dependencies:
```bash
npm install react-icons
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add utils
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
