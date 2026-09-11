<!-- Bouncing Text · @pacekit · https://21st.dev/@pacekit/components/bouncing-text
     license: MIT · category: text
     Animated text that splits into individual characters and bounces them into place in sequence. -->

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
components/gsap/bouncing-text.tsx
"use client";

import { ComponentProps, useRef } from "react";

import { useGSAP } from "@gsap/react";
import gsap from "gsap";
import { SplitText } from "gsap/SplitText";

type BouncingTextProps = {
    repeat?: boolean | number;
} & ComponentProps<"p">;

export const BouncingText = ({ repeat = true, ...props }: BouncingTextProps) => {
    const textRef = useRef<HTMLParagraphElement | null>(null);

    useGSAP(
        () => {
            if (!textRef.current) return;
            let bounceCount = 0;
            const bounceChars = new SplitText(textRef.current, { type: "words,chars" }).chars;
            bounceChars.forEach((el) => {
                const tl = gsap.timeline({ repeat: repeat === true ? -1 : repeat === false ? 0 : repeat });
                tl.to(el, {
                    duration: 0,
                    y: -200,
                });
                tl.to(el, {
                    duration: 2,
                    y: 0,
                    rotate: -10,
                    ease: "bounce",
                });
                tl.to(el, {
                    duration: 1,
                    y: 0,
                    rotate: 0,
                    ease: "bounce",
                });
                tl.to(el, {
                    duration: 2,
                    y: -200,
                    rotate: 0,
                    delay: 1,
                    ease: "elastic",
                });
                tl.delay(bounceCount / 8);
                bounceCount++;
            });
        },
        { scope: textRef },
    );

    return <p {...props} ref={textRef} />;
};

demo.tsx
import { BouncingText } from "@/components/ui/bouncing-text";

export default function Demo() {
    return (
        <div className="flex min-h-[320px] w-full items-center justify-center">
            <BouncingText className="text-3xl font-semibold">Text Bounce</BouncingText>
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
