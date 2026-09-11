<!-- Squash Text · @pacekit · https://21st.dev/@pacekit/components/squash-text
     license: MIT · category: text
     Animated heading whose characters bounce and squash into place with a playful, dynamic GSAP effect. -->

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
components/gsap/squash-text.tsx
"use client";

import { ComponentProps, useId, useRef } from "react";

import { useGSAP } from "@gsap/react";
import gsap from "gsap";
import { CustomBounce } from "gsap/CustomBounce";
import { CustomEase } from "gsap/CustomEase";
import { SplitText } from "gsap/SplitText";

// Ensure plugins are registered globally
if (typeof window !== "undefined") {
    gsap.registerPlugin(CustomEase, CustomBounce, SplitText);
}

type SquashTextProps = {
    repeat?: boolean | number;
} & ComponentProps<"div">;

export const SquashText = ({ repeat = true, children, ...props }: SquashTextProps) => {
    const wrapperRef = useRef<HTMLDivElement | null>(null);
    const bounceId = useId();

    // We sanitize the IDs for GSAP ease names
    const bounceEase = `bounce${bounceId.replace(/:/g, "")}`;
    const squashEase = `squash${bounceId.replace(/:/g, "")}`;

    useGSAP(
        () => {
            if (!wrapperRef.current) return;

            // 1. Create the Custom Bounce
            CustomBounce.create(bounceEase, {
                strength: 0.6,
                squash: 1,
                squashID: squashEase,
            });

            // 2. Initialize SplitText
            const split = new SplitText(wrapperRef.current, { type: "chars" });
            const chars = split.chars;

            // 3. Create Timeline
            const tl = gsap.timeline({
                defaults: { duration: 1.5, stagger: { amount: 0.1, ease: "sine.in" } },
                repeat: repeat === true ? -1 : repeat === false ? 0 : repeat,
            });

            tl.from(
                chars,
                {
                    duration: 0.6,
                    opacity: 0,
                    ease: "power1.inOut",
                },
                0,
            )
                .from(
                    chars,
                    {
                        y: -100,
                        ease: bounceEase,
                    },
                    0,
                )
                .to(
                    chars,
                    {
                        scaleX: 1.5,
                        scaleY: 0.8,
                        rotate: () => 15 - 30 * Math.random(),
                        ease: squashEase,
                        transformOrigin: "50% 100%",
                    },
                    0,
                );

            // Cleanup: Revert the split text when component unmounts
            return () => {
                split.revert();
            };
        },
        { scope: wrapperRef, dependencies: [repeat] },
    );

    return (
        <div {...props} ref={wrapperRef}>
            {children}
        </div>
    );
};

demo.tsx
import { SquashText } from "@/components/ui/squash-text";

export default function SquashTextDemo() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center bg-white p-8">
      <SquashText className="text-5xl font-bold tracking-tight text-neutral-900 md:text-7xl">
        Squash Text
      </SquashText>
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
