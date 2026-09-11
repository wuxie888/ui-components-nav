<!-- Tilt Effect · @bundui · https://21st.dev/@bundui/components/tilt-effect
     license: no-license · category: image
     Here is Tilt Effect component -->

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
components/ui/page.tsx
import Image from "next/image";

import { TiltEffect } from "./tilt-effect";
import { Card, CardContent } from "@/components/ui/card";
import { Badge } from "@/components/ui/badge";

export default function TiltEffectExample() {
  return (
    <TiltEffect>
      <Card className="overflow-hidden rounded-md pt-0 shadow-none md:w-[280px]">
        <Image
          src="/images/products/list1.png"
          alt=""
          width={200}
          height={200}
          className="aspect-square size-full w-full object-cover transition-all duration-200 ease-linear"
          unoptimized
        />
        <CardContent className="space-y-1">
          <div className="text-lg font-semibold">T-shirt</div>
          <Badge variant="outline">Best Sellers</Badge>
        </CardContent>
      </Card>
    </TiltEffect>
  );
}

components/ui/tilt-effect.tsx
"use client";

import React, { useState, useRef, useEffect } from "react";
import { motion, useMotionValue, useSpring, useTransform } from "motion/react";

export type TiltEffectProps = {
  children: React.ReactNode;
  tiltFactor?: number;
  perspective?: number;
  transitionDuration?: number;
};

export const TiltEffect: React.FC<TiltEffectProps> = ({
  children,
  tiltFactor = 12,
  perspective = 1000,
  transitionDuration = 0.5
}) => {
  const ref = useRef<HTMLDivElement>(null);
  const [elementSize, setElementSize] = useState({ width: 0, height: 0 });

  const x = useMotionValue(0);
  const y = useMotionValue(0);

  const springConfig = { damping: 30, stiffness: 400, mass: 0.5 };
  const xSpring = useSpring(x, springConfig);
  const ySpring = useSpring(y, springConfig);

  const rotateX = useTransform(
    ySpring,
    [-elementSize.height / 2, elementSize.height / 2],
    [tiltFactor, -tiltFactor]
  );
  const rotateY = useTransform(
    xSpring,
    [-elementSize.width / 2, elementSize.width / 2],
    [-tiltFactor, tiltFactor]
  );

  useEffect(() => {
    const updateSize = () => {
      if (ref.current) {
        setElementSize({
          width: ref.current.offsetWidth,
          height: ref.current.offsetHeight
        });
      }
    };

    updateSize();
    window.addEventListener("resize", updateSize);
    return () => window.removeEventListener("resize", updateSize);
  }, []);

  const handleMouseMove = (event: React.MouseEvent<HTMLDivElement>) => {
    if (!ref.current) return;
    const rect = ref.current.getBoundingClientRect();
    const mouseX = event.clientX - rect.left;
    const mouseY = event.clientY - rect.top;
    const centerX = elementSize.width / 2;
    const centerY = elementSize.height / 2;
    x.set(mouseX - centerX);
    y.set(mouseY - centerY);
  };

  const handleMouseLeave = () => {
    x.set(0);
    y.set(0);
  };

  return (
    <motion.div
      ref={ref}
      style={{
        perspective,
        transformStyle: "preserve-3d"
      }}
      onMouseMove={handleMouseMove}
      onMouseLeave={handleMouseLeave}>
      <motion.div
        style={{
          rotateX,
          rotateY
        }}
        transition={{
          duration: transitionDuration,
          type: "spring",
          ...springConfig
        }}>
        {children}
      </motion.div>
    </motion.div>
  );
};

demo.tsx
import Image from "next/image";

import { TiltEffect } from "@/components/ui/tilt-effect";
import { Card, CardContent } from "@/components/ui/card";

export default function TiltEffectExample() {
  return (
    <TiltEffect>
      <Card className="overflow-hidden pt-0 md:w-[280px]">
        <Image
          src="https://cdn.21st.dev/assets/localized/5aba418e7e3bfa36a2d1f5808dff35fad665a91c6f8c37b9c6863d4c09b215e6.jpg"
          alt=""
          width={200}
          height={200}
          className="aspect-square size-full w-full object-cover transition-all duration-200 ease-linear"
          unoptimized
        />
        <CardContent>
          <div className="text-xl">Red Hat</div>
          <div className="text-muted-foreground text-sm">Best Sellers</div>
        </CardContent>
      </Card>
    </TiltEffect>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge card tilt-effect.json
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
