<!-- Zoomable Image · @inference-sh · https://21st.dev/@inference-sh/components/zoomable-image
     license: MIT · category: gallery
     Click-to-zoom image component with a medium.com-style lightbox that expands on click and closes on scroll. -->

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
components/infsh/zoomable-image.tsx
"use client"

import Zoom from "react-medium-image-zoom"
import "react-medium-image-zoom/dist/styles.css"
import "./zoomable-image.css"
import { ImageOff } from "lucide-react"
import { DetailedHTMLProps, ImgHTMLAttributes, useState } from "react"

export interface ZoomableImageProps
  extends DetailedHTMLProps<ImgHTMLAttributes<HTMLImageElement>, HTMLImageElement> {
  /** Disable zoom behavior */
  disabled?: boolean
  /** Higher quality image src to load when zoomed */
  zoomSrc?: string
  /** Offset in pixels from window boundaries when zoomed */
  zoomMargin?: number
}

export default function ZoomableImage({
  src,
  alt,
  className,
  onLoad,
  onError,
  disabled = false,
  zoomSrc,
  zoomMargin = 16,
  ...rest
}: ZoomableImageProps) {
  const [hasError, setHasError] = useState(false)

  if (!src) return null

  const handleError = (e: React.SyntheticEvent<HTMLImageElement, Event>) => {
    setHasError(true)
    onError?.(e)
  }

  if (hasError) {
    return (
      <div className="aspect-square w-full h-24 flex items-center justify-center rounded-md">
        <div className="flex flex-col items-center gap-2 text-muted-foreground">
          <ImageOff strokeWidth={1} className="w-8 h-8" />
        </div>
      </div>
    )
  }

  const image = (
    <img
      src={src}
      alt={alt || ""}
      className={className}
      onLoad={onLoad}
      onError={handleError}
      {...rest}
    />
  )

  if (disabled) {
    return image
  }

  return (
    <Zoom
      classDialog="zoomable-image-dialog"
      zoomMargin={zoomMargin}
      zoomImg={zoomSrc ? { src: zoomSrc } : undefined}
    >
      {image}
    </Zoom>
  )
}

components/infsh/zoomable-image.css
.zoomable-image-dialog [data-rmiz-modal-overlay="hidden"] {
  background-color: transparent;
}

.zoomable-image-dialog [data-rmiz-modal-overlay="visible"] {
  background-color: var(--background);
}

.zoomable-image-dialog [data-rmiz-btn-unzoom] {
  background-color: var(--muted);
  color: var(--muted-foreground);
}

.zoomable-image-dialog [data-rmiz-btn-unzoom]:focus-visible {
  outline-offset: 0.25rem;
  outline: 0.125rem solid var(--ring);
}

demo.tsx
import ZoomableImage from "@/components/ui/zoomable-image";

export default function Default() {
  return (
    <div className="flex flex-col items-center gap-3 p-8">
      <p className="text-sm text-muted-foreground">
        click the image to zoom in, click again or scroll to close.
      </p>
      <ZoomableImage
        src="https://cdn.21st.dev/assets/mirror/ca/ca905300194e1d0ffc3223cfd3ba226477f36706c5461ffcd97c6f9e15344b8e.jpg"
        alt="A beautiful landscape"
        className="rounded-lg w-full max-w-md"
      />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react react-medium-image-zoom
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
