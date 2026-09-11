<!-- Spotlight Cursor · @uilayout.contact · https://21st.dev/@uilayout.contact/components/spotlight-cursor
     license: unspecified · category: cursor
     Here is spotlight cursor effect -->

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
components/ui/spotlight.tsx
// @ts-nocheck
'use client';
import { cn } from '@/lib/utils';
import React, { createContext, type MouseEvent, useContext, useRef, useState } from 'react';

interface MousePosition {
  x: number;
  y: number;
}

interface SpotlightProps {
  children: React.ReactNode;
  className?: string;
  ProximitySpotlight?: boolean;
  HoverFocusSpotlight?: boolean;
  CursorFlowGradient?: boolean;
}
interface SpotlightItemProps {
  children: React.ReactNode;
  className?: string;
}

interface SpotLightContextType {
  ProximitySpotlight: boolean;
  HoverFocusSpotlight: boolean;
  CursorFlowGradient: boolean;
}

const SpotLightContext = createContext<SpotLightContextType | undefined>(undefined);
export const useSpotlight = () => {
  const context = useContext(SpotLightContext);
  if (!context) {
    throw new Error('useSpotlight must be used within a SpotlightProvider');
  }
  return context;
};
export const Spotlight = ({
  children,
  className,
  ProximitySpotlight = true,
  HoverFocusSpotlight = false,
  CursorFlowGradient = true,
}: SpotlightProps) => {
  return (
    <SpotLightContext.Provider
      value={{
        ProximitySpotlight,
        HoverFocusSpotlight,
        CursorFlowGradient,
      }}
    >
      <div className={cn('group relative z-10 rounded-md    ', className)}>{children}</div>
    </SpotLightContext.Provider>
  );
};
export function SpotLightItem({ children, className }: SpotlightItemProps) {
  const { HoverFocusSpotlight, ProximitySpotlight, CursorFlowGradient } = useSpotlight();
  const boxWrapper = useRef(null);
  const [isHovered, setIsHovered] = useState(false);
  const [mousePosition, setMousePosition] = React.useState({
    x: null,
    y: null,
  });
  React.useEffect(() => {
    const updateMousePosition = (ev: { clientX: any; clientY: any }) => {
      setMousePosition({ x: ev.clientX, y: ev.clientY });
    };
    window.addEventListener('mousemove', updateMousePosition);
    return () => {
      window.removeEventListener('mousemove', updateMousePosition);
    };
  }, []);

  const [overlayColor, setOverlayColor] = useState({ x: 0, y: 0 });
  const handleMouemove = ({ currentTarget, clientX, clientY }): MouseEvent => {
    const { left, top } = currentTarget.getBoundingClientRect();

    const x = clientX - left;
    const y = clientY - top;

    setOverlayColor({ x, y });
  };
  // console.log(overlayColor)

  return (
    <div
      onMouseMove={handleMouemove}
      onMouseEnter={() => CursorFlowGradient && setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
      ref={boxWrapper}
      className={cn(className, ' relative  rounded-lg p-[2px] bg-[#ffffff15] overflow-hidden')}
    >
      {isHovered && (
        <div
          className='pointer-events-none absolute opacity-0 z-50 rounded-xl w-full h-full group-hover:opacity-100  transition duration-300 '
          style={{
            background: `
            radial-gradient(
              250px circle at ${overlayColor.x}px ${overlayColor.y}px,
              rgba(255, 255, 255, 0.137),
              transparent 80%
            )
          `,
          }}
        />
      )}
      {HoverFocusSpotlight && (
        <div
          className='absolute opacity-0 group-hover:opacity-100 z-10 inset-0 bg-fixed rounded-lg'
          style={{
            background: `radial-gradient(circle at ${mousePosition.x}px ${mousePosition.y}px, #ffffff76 0%,transparent 20%,transparent) fixed `,
          }}
        ></div>
      )}
      {ProximitySpotlight && (
        <div
          className='absolute inset-0 z-0  bg-fixed rounded-lg'
          style={{
            background: `radial-gradient(circle at ${mousePosition.x}px ${mousePosition.y}px, #ffffff6e 0%,transparent 20%,transparent) fixed`,
          }}
        ></div>
      )}
      {children}
    </div>
  );
}

type SpotlightCardProps = {
  children: React.ReactNode;
  className?: string;
};

export function SpotlightCard({ children, className = '' }: SpotlightCardProps) {
  return (
    <div
      className={`relative h-full bg-slate-800 rounded-3xl p-px before:absolute before:w-80 before:h-80 before:-left-40 before:-top-40 before:bg-slate-400 before:rounded-full before:opacity-0 before:pointer-events-none before:transition-opacity before:duration-500 before:translate-x-(--mouse-x) before:translate-y-(--mouse-y) group-hover:before:opacity-100 before:z-10 before:blur-[100px] after:absolute after:w-96 after:h-96 after:-left-48 after:-top-48 after:bg-indigo-500 after:rounded-full after:opacity-0 after:pointer-events-none after:transition-opacity after:duration-500 after:translate-x-(--mouse-x) after:translate-y-(--mouse-y) hover:after:opacity-10 after:z-30 after:blur-[100px] overflow-hidden ${className}`}
    >
      {children}
    </div>
  );
}

demo.tsx
import { Component } from "@/components/ui/spotlight-cursor";

const DemoOne = () => {
  return (
    <div className="flex w-full h-screen justify-center items-center bg-black overflow-hidden">
      <h1 className="text-white text-4xl font-bold z-10 pointer-events-none">Move your mouse!</h1>
      <Component />
    </div>
  );
};

export { DemoOne };
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
