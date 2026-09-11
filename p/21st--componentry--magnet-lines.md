<!-- Magnet Lines · @componentry · https://21st.dev/@componentry/components/magnet-lines
     license: MIT · category: cursor
     An interactive grid of lines that rotate to point toward the cursor, creating a magnetic field effect. -->

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
components/ui/magnet-lines.tsx
"use client";

import React, { useRef, useState, useEffect } from "react";
import { motion } from "framer-motion";

interface MagnetLinesProps {
    rows?: number;
    columns?: number;
    containerSize?: string;
    lineColor?: string;
    lineWidth?: string;
    lineHeight?: string;
    baseAngle?: number;
    className?: string;
    style?: React.CSSProperties;
}

export function MagnetLines({
    rows = 9,
    columns = 9,
    containerSize = "80vmin",
    lineColor = "#efefef",
    lineWidth = "1vmin",
    lineHeight = "6vmin",
    baseAngle = 0,
    className = "",
    style = {},
}: MagnetLinesProps) {
    const containerRef = useRef<HTMLDivElement>(null);

    // Spans the grid
    const total = rows * columns;
    const spans = Array.from({ length: total }, (_, i) => i);

    return (
        <div
            ref={containerRef}
            className={`relative grid place-items-center ${className}`}
            style={{
                gridTemplateColumns: `repeat(${columns}, 1fr)`,
                gridTemplateRows: `repeat(${rows}, 1fr)`,
                width: containerSize,
                height: containerSize,
                ...style,
            }}
        >
            {spans.map((i) => (
                <Line
                    key={i}
                    containerRef={containerRef}
                    lineColor={lineColor}
                    lineWidth={lineWidth}
                    lineHeight={lineHeight}
                    baseAngle={baseAngle}
                />
            ))}
        </div>
    );
}

function Line({
    containerRef,
    lineColor,
    lineWidth,
    lineHeight,
    baseAngle,
}: {
    containerRef: React.RefObject<HTMLDivElement | null>;
    lineColor: string;
    lineWidth: string;
    lineHeight: string;
    baseAngle: number;
}) {
    const lineRef = useRef<HTMLDivElement>(null);
    const [rotate, setRotate] = useState(baseAngle);

    useEffect(() => {
        const updateRotation = (e: MouseEvent) => {
            if (!lineRef.current || !containerRef.current) return;

            const rect = lineRef.current.getBoundingClientRect();
            const centerX = rect.left + rect.width / 2;
            const centerY = rect.top + rect.height / 2;

            const mouseX = e.clientX;
            const mouseY = e.clientY;

            const distanceX = mouseX - centerX;
            const distanceY = mouseY - centerY;

            const angle = (Math.atan2(distanceY, distanceX) * 180) / Math.PI;

            setRotate(angle + baseAngle);
        };

        window.addEventListener("mousemove", updateRotation);
        return () => window.removeEventListener("mousemove", updateRotation);
    }, [baseAngle, containerRef]);

    return (
        <motion.div
            ref={lineRef}
            animate={{ rotate }}
            transition={{ type: "spring", damping: 20, stiffness: 300 }}
            style={{
                width: lineWidth,
                height: lineHeight,
                backgroundColor: lineColor,
            }}
        />
    );
}

demo.tsx
import { MagnetLines } from "@/components/ui/magnet-lines";

export default function Default() {
    return (
        <div className="flex min-h-[500px] w-full items-center justify-center bg-black">
            <MagnetLines
                rows={9}
                columns={9}
                containerSize="60vmin"
                lineColor="#efefef"
                lineWidth="0.8vmin"
                lineHeight="5vmin"
                baseAngle={0}
            />
        </div>
    );
}
```

Install NPM dependencies:
```bash
npm install framer-motion
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
