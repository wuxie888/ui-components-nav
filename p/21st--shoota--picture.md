<!-- Picture · @shoota · https://21st.dev/@shoota/components/picture
     license: no-license · category: image
     A figure primitive that frames an image with a grayscale-to-color hover reveal and an overlaid caption, for blog and editorial reading layouts. -->

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
components/blog/picture.tsx
import * as React from "react"

import { cn } from "@/lib/utils"

type PictureProps = React.HTMLAttributes<HTMLElement> & {
  children?: React.ReactNode
}

function PictureRoot({ className, children, ...props }: PictureProps) {
  return (
    <figure
      className={cn(
        "relative overflow-hidden rounded-md bg-muted [&_img]:rounded-md",
        className
      )}
      {...props}
    >
      {children}
    </figure>
  )
}

type PictureImageProps = React.ImgHTMLAttributes<HTMLImageElement> & {
  objectPosition?: React.CSSProperties["objectPosition"]
  transition?: boolean
}

function PictureImage({
  className,
  objectPosition,
  transition = false,
  style,
  ...props
}: PictureImageProps) {
  return (
    <img
      {...props}
      style={{ objectPosition, ...style }}
      className={cn(
        "block w-full object-cover opacity-60 grayscale transition-opacity duration-700",
        transition &&
          "hover:opacity-100 hover:grayscale-[60%] hover:duration-1000 focus:opacity-100 focus:grayscale-[60%] focus:duration-1000",
        className
      )}
    />
  )
}

type PictureCaptionProps = React.HTMLAttributes<HTMLElement>

function PictureCaption({
  className,
  children,
  ...props
}: PictureCaptionProps) {
  return (
    <figcaption
      className={cn(
        "absolute bottom-0 right-0 h-6 px-1.5 text-xs font-bold leading-6 text-accent bg-[rgba(4,37,43,0.6)]",
        className
      )}
      {...props}
    >
      {children}
    </figcaption>
  )
}

export const Picture = Object.assign(PictureRoot, {
  Image: PictureImage,
  Caption: PictureCaption,
})

export { PictureImage, PictureCaption }
export type { PictureProps, PictureImageProps, PictureCaptionProps }

demo.tsx
import { Picture } from "@/components/ui/picture";

export default function PictureDemo() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center bg-background p-8 text-foreground">
      <Picture className="w-full max-w-md">
        <Picture.Image
          src="https://cdn.21st.dev/assets/mirror/9f/9f9d953869e8b1b03f94862aa569ea2b120b47821487e0f5ba356bbde8a3d8a4.jpg"
          alt="Fog drifting over a pine forest at dawn"
          transition
        />
        <Picture.Caption>Gymnopédie No.1</Picture.Caption>
      </Picture>
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add theme.json utils.json
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
