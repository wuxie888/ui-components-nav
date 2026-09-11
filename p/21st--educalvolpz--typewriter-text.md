<!-- Typewriter Text · @educalvolpz · https://21st.dev/@educalvolpz/components/typewriter-text
     license: MIT · category: text
     Animated text that reveals character by character with a typewriter effect, with configurable speed, optional looping, and reduced-motion support. -->

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
components/ui/index.tsx
import type React from "react";
import { useEffect, useRef, useState } from "react";

function useReducedMotion() {
  const [shouldReduceMotion, setShouldReduceMotion] = useState(false);

  useEffect(() => {
    const mediaQuery = window.matchMedia("(prefers-reduced-motion: reduce)");
    setShouldReduceMotion(mediaQuery.matches);

    const handleChange = (e: MediaQueryListEvent) => {
      setShouldReduceMotion(e.matches);
    };

    mediaQuery.addEventListener("change", handleChange);
    return () => mediaQuery.removeEventListener("change", handleChange);
  }, []);

  return shouldReduceMotion;
}

export interface TypewriterTextProps {
  children: string;
  className?: string;
  loop?: boolean;
  speed?: number;
}

const LOOP_RESTART_DELAY_MS = 1000;

const TypewriterText: React.FC<TypewriterTextProps> = ({
  children,
  speed = 50,
  loop = false,
  className = "",
}) => {
  const [displayed, setDisplayed] = useState("");
  const index = useRef(0);
  const timeout = useRef<NodeJS.Timeout | null>(null);
  const shouldReduceMotion = useReducedMotion();

  useEffect(() => {
    if (shouldReduceMotion) {
      // Show full text immediately when reduced motion is enabled
      setDisplayed(children);
      return;
    }

    setDisplayed("");
    index.current = 0;
    function type() {
      setDisplayed(children.slice(0, index.current + 1));
      if (index.current < children.length - 1) {
        index.current++;
        timeout.current = setTimeout(type, speed);
      } else if (loop) {
        timeout.current = setTimeout(() => {
          setDisplayed("");
          index.current = 0;
          type();
        }, LOOP_RESTART_DELAY_MS);
      }
    }
    type();
    return () => {
      if (timeout.current) {
        clearTimeout(timeout.current);
      }
    };
  }, [children, speed, loop, shouldReduceMotion]);

  return <span className={className}>{displayed}</span>;
};

export default TypewriterText;

demo.tsx
import TypewriterText from "@/components/ui/typewriter-text";

export default function TypewriterTextDemo() {
  return (
    <div className="flex w-full flex-col items-center justify-center gap-4 px-8 py-16 text-center">
      <TypewriterText
        className="text-3xl font-semibold tracking-tight text-foreground"
        speed={80}
        loop
      >
        Welcome to SmoothUI
      </TypewriterText>
      <TypewriterText
        className="max-w-md text-lg text-muted-foreground"
        speed={40}
        loop
      >
        This text loops so you can preview the typewriter effect.
      </TypewriterText>
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
