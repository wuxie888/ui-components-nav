<!-- Mouse Wave Text · @pacekit · https://21st.dev/@pacekit/components/mouse-wave-text
     license: MIT · category: text
     Interactive text that ripples with a wave-like bounce effect as the mouse moves across the screen. -->

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
components/gsap/mouse-wave-text.tsx
"use client";

import { ComponentProps, ReactNode, useRef } from "react";

import { useGSAP } from "@gsap/react";
import gsap from "gsap";

import { cn } from "@/lib/utils";

type MouseWaveTextProps = {
    children: ReactNode;
    className?: string;
    shadowClassName?: string;
    textClassName?: string;
    wrapperTweenVars?: gsap.TweenVars;
} & ComponentProps<"div">;

export const MouseWaveText = ({
    children,
    textClassName,
    className,
    shadowClassName,
    wrapperTweenVars,
    ...props
}: MouseWaveTextProps) => {
    const wrapperRef = useRef<HTMLDivElement | null>(null);
    const textRef = useRef<HTMLParagraphElement | null>(null);
    const shadowRef = useRef<HTMLParagraphElement | null>(null);

    useGSAP(
        () => {
            if (!wrapperRef.current || !shadowRef.current || !textRef.current) return;
            const textContent = shadowRef.current.textContent;
            const bb = shadowRef.current.getBoundingClientRect();

            for (let i = 0; i <= bb.width * 0.55; i++) {
                const div = document.createElement("div");
                textRef.current.append(div);
                gsap.set(div, {
                    position: "absolute",
                    width: 4,
                    height: bb.height,
                    x: i * 2,
                    y: -bb.height,
                    textIndent: -i * 2,
                    overflow: "hidden",
                    textContent: textContent,
                });
            }

            if (wrapperTweenVars) gsap.set(wrapperRef.current, wrapperTweenVars);

            const tl = gsap
                .timeline({
                    paused: true,
                    defaults: { duration: 0.25, ease: "power3.inOut", yoyoEase: "sine.inOut" },
                })
                .to(textRef.current.children, {
                    y: "-=30",
                    stagger: {
                        amount: 1,
                        yoyo: true,
                        repeat: 1,
                        ease: "none",
                    },
                });

            gsap.timeline()
                .fromTo(tl, { progress: 0.9 }, { duration: 1.5, progress: 0.1, ease: "power2.inOut" })
                .to(tl, { duration: 4, progress: 0.4, ease: "elastic.out(0.8)" });

            const onMove = (e: MouseEvent) => {
                const xp = e.x / window.innerWidth;
                gsap.to(tl, { progress: xp, overwrite: true });
                gsap.to(wrapperRef.current, {
                    x: gsap.utils.mapRange(0, 1, 15, -15, xp),
                    y: gsap.utils.mapRange(0, 1, -15, 15, xp),
                });
            };

            const onMouseDown = () => {
                gsap.timeline({ defaults: { duration: 0.2, overwrite: "auto" } })
                    .to(textRef.current, {
                        y: -25,
                    })
                    .to(
                        shadowRef.current,
                        {
                            filter: "blur(2px)",
                            opacity: 0.85,
                            scale: 0.96,
                            transformOrigin: "45px 99px",
                        },
                        0,
                    );
            };

            const onMouseUp = () => {
                gsap.timeline({ defaults: { ease: "bounce" } })
                    .to(textRef.current, { y: 0 })
                    .to(
                        shadowRef.current,
                        {
                            filter: "blur(0px)",
                            opacity: 1,
                            scale: 1,
                        },
                        0,
                    );
            };

            window.addEventListener("pointermove", onMove);
            window.addEventListener("mousedown", onMouseDown);
            window.addEventListener("mouseup", onMouseUp);

            return () => {
                window.removeEventListener("pointermove", onMove);
                window.removeEventListener("mousedown", onMouseDown);
                window.removeEventListener("mouseup", onMouseUp);
            };
        },
        { scope: wrapperRef },
    );

    return (
        <div {...props} ref={wrapperRef} className={cn("whitespace-nowrap", className)}>
            <p ref={shadowRef} className={shadowClassName}>
                {children}
            </p>
            <p ref={textRef} aria-disabled="true" className={textClassName} />
        </div>
    );
};

demo.tsx
import { MouseWaveText } from "@/components/ui/mouse-wave-text";

export default function Demo() {
    return (
        <div className="flex min-h-[320px] w-full items-center justify-center">
            <MouseWaveText
                className="text-3xl font-semibold"
                textClassName="text-blue-500"
                shadowClassName="text-blue-500/20">
                Mouse Wave Text
            </MouseWaveText>
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
