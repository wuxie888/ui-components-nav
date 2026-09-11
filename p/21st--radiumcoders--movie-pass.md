<!-- Movie Pass Button · @radiumcoders · https://21st.dev/@radiumcoders/components/movie-pass
     license: MIT · category: button
     A ticket-style button with rounded corner cut-outs punched into each corner, like a torn cinema pass, with a quick press-scale tactile effect. Theme-aware via primary and background tokens. -->

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
components/evil-buttons/movie-pass.tsx
"use client";

import { CSSProperties, ReactNode } from "react";
import { cn } from "@/lib/utils";

/**
 * Notch radius for the four corner cutouts. The notches are punched out with a
 * CSS mask instead of background-colored overlays, so the ticket shape works on
 * any surface (no need to match the page background).
 */
const NOTCH = 8;

const cornerNotchMask = [
  `radial-gradient(circle ${NOTCH}px at 0 0, transparent ${NOTCH}px, black ${NOTCH + 0.5}px)`,
  `radial-gradient(circle ${NOTCH}px at 100% 0, transparent ${NOTCH}px, black ${NOTCH + 0.5}px)`,
  `radial-gradient(circle ${NOTCH}px at 0 100%, transparent ${NOTCH}px, black ${NOTCH + 0.5}px)`,
  `radial-gradient(circle ${NOTCH}px at 100% 100%, transparent ${NOTCH}px, black ${NOTCH + 0.5}px)`,
].join(", ");

const notchStyle: CSSProperties = {
  WebkitMaskImage: cornerNotchMask,
  maskImage: cornerNotchMask,
  WebkitMaskRepeat: "no-repeat",
  maskRepeat: "no-repeat",
  // Intersect the four masks so only the corner circles are cut out.
  WebkitMaskComposite: "source-in",
  maskComposite: "intersect",
};

function MoviePassButton({
  children,
  className,
  style,
}: {
  children: ReactNode;
  className?: string;
  style?: CSSProperties;
}) {
  return (
    <button
      style={{ ...notchStyle, ...style }}
      className={cn(
        className,
        "px-6 py-3 bg-primary text-background",
        "relative",
        "active:scale-95 transition-all duration-75",
      )}
    >
      {children}
    </button>
  );
}
export default MoviePassButton;

demo.tsx
"use client";

import MoviePassButton from "@/components/ui/movie-pass";

export default function Default() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center bg-background p-12">
      <MoviePassButton>Deploy Doom</MoviePassButton>
    </div>
  );
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
