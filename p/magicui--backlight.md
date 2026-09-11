<!-- Backlight · Magic UI · https://magicui.design/docs/components/backlight
     license: MIT · category: image
     A backlight glow effect for videos, images, and SVGs. -->

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
components/ui/backlight.tsx
import { useId, type ReactElement } from "react"

type BacklightProps = {
  children?: ReactElement
  className?: string
  blur?: number
}

export function Backlight({ blur = 20, children, className }: BacklightProps) {
  const id = useId()

  return (
    <div className={className}>
      <svg width="0" height="0" aria-hidden="true">
        <filter id={id} y="-50%" x="-50%" width="200%" height="200%">
          <feGaussianBlur
            in="SourceGraphic"
            stdDeviation={blur}
            result="blurred"
          ></feGaussianBlur>
          <feColorMatrix
            type="saturate"
            in="blurred"
            values="4"
          ></feColorMatrix>
          <feComposite in="SourceGraphic" operator="over"></feComposite>
        </filter>
      </svg>

      <div style={{ filter: `url(#${id})` }}>{children}</div>
    </div>
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
