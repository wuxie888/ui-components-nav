<!-- Scramble Text · @pacekit · https://21st.dev/@pacekit/components/scramble-text
     license: MIT · category: text
     A hover-triggered text effect that scrambles and restores its characters with smooth GSAP motion. -->

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
components/gsap/scramble-text.tsx
"use client";

import { ComponentProps, useEffect, useRef } from "react";

import { useGSAP } from "@gsap/react";
import gsap from "gsap";
import { ScrambleTextPlugin } from "gsap/ScrambleTextPlugin";

gsap.registerPlugin(ScrambleTextPlugin);

type ScrambleTextProps = {
    random?: boolean;
    scrambleOnLoad?: boolean;
} & ComponentProps<"div">;

const defaultChars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";

export const ScrambleText = ({ scrambleOnLoad = true, random = true, ...props }: ScrambleTextProps) => {
    const wrapperRef = useRef<HTMLDivElement | null>(null);

    const { contextSafe } = useGSAP();

    const scramble = contextSafe(() => {
        const target = wrapperRef.current;
        if (gsap.isTweening(target) || !target) return;
        gsap.to(target, {
            duration: 1,
            ease: "sine.in",
            scrambleText: {
                text: target.innerText,
                speed: 2,
                chars: random ? defaultChars : target.innerText.replace(/\s/g, ""),
            },
        });
    });

    useEffect(() => {
        if (scrambleOnLoad) scramble();
        const target = wrapperRef.current;
        target?.addEventListener("pointerenter", scramble);
        return () => target?.removeEventListener("pointerenter", scramble);
    }, [wrapperRef, scramble, scrambleOnLoad]);

    return <div {...props} ref={wrapperRef} />;
};

demo.tsx
import { ScrambleText } from "@/components/ui/scramble-text";

export default function Demo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center bg-background px-6 text-foreground">
      <ScrambleText className="font-mono text-lg font-medium sm:text-xl xl:text-3xl">
        <p>Hover to scramble what you see</p>
      </ScrambleText>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @gsap/react gsap
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
