<!-- Gif Text · @chamaac · https://21st.dev/@chamaac/components/gif-text
     license: no-license · category: text
     Animated text that uses a GIF as the fill color via background-clip, with a loading state. -->

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
components/ui/gif-text.tsx
"use client";

import { cn } from "@/lib/utils";
import { useEffect, useState } from "react";

interface GifTextProps {
  /**
   * The text to display
   */
  text?: string;
  /**
   * The source URL for the background image/gif
   */
  gif?: string;
  /**
   * Class for the text element
   */
  className?: string;
  /**
   * Class for the container (e.g. height, background)
   */
  containerClassName?: string;
}

const GifText = ({
  text = "CHAMAAC",
  gif = "https://assets.amarn.me/gif-text.gif",
  className,
  containerClassName,
}: GifTextProps) => {
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    if (!gif) return;

    setLoading(true);
    const img = new Image();
    img.src = gif;
    img.onload = () => setLoading(false);

    return () => {
      img.onload = null;
    };
  }, [gif]);

  return (
    <div
      className={cn(
        "flex flex-col items-center justify-center p-4 bg-white dark:bg-black",
        containerClassName
      )}
    >
      <h2
        className={cn(
          "text-[clamp(80px,12vw,150px)] font-extrabold select-none text-center leading-tight uppercase transition-colors duration-300 ",
          loading
            ? "text-neutral-400 animate-pulse duration-100"
            : "text-transparent bg-clip-text bg-cover bg-center bg-no-repeat",
          className
        )}
        style={{
          backgroundImage: loading ? "none" : `url(${gif})`,
          WebkitBackgroundClip: loading ? "none" : "text",
          backgroundClip: loading ? "none" : "text",
        }}
      >
        {text}
      </h2>
    </div>
  );
};

export default GifText;

demo.tsx
import GifText from "@/components/ui/gif-text";

export default function GifTextDemo() {
  return (
    <GifText
      text="CHAMAAC"
      gif="https://cdn.21st.dev/assets/mirror/3b/3b4510e4cd062ea14630d3d6abcc80b867d4c4ba0dc648d408648c9b15592c52.gif"
      containerClassName="h-[400px] w-full"
    />
  );
}
```

Install NPM dependencies:
```bash
npm install clsx tailwind-merge
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
