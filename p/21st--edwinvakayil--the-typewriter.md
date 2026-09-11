<!-- The Typewriter · @edwinvakayil · https://21st.dev/@edwinvakayil/components/the-typewriter
     license: unspecified · category: text
      -->

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
components/ui/typewriter.tsx
"use client";

import { motion } from "motion/react";
import { useEffect, useState } from "react";

import { cn } from "@/lib/utils";

export interface TextTypewriterProps {
  children: string;
  className?: string;
  duration?: number;
}

const WRONG_CHARS = "!@#$%^&*()QWERTY";

function randomWrongChar() {
  return WRONG_CHARS[Math.floor(Math.random() * WRONG_CHARS.length)];
}

export default function TextTypewriter({
  children,
  className,
  duration = 3,
}: TextTypewriterProps) {
  const [text, setText] = useState("");
  const [showCursor, setShowCursor] = useState(false);

  useEffect(() => {
    const timeouts = new Set<ReturnType<typeof setTimeout>>();
    const speed = duration / 3;

    const schedule = (callback: () => void, ms: number) => {
      const id = setTimeout(callback, ms * speed);
      timeouts.add(id);
    };

    const runAnimation = () => {
      let currentText = "";
      let targetIndex = 0;
      const finalText = children;

      const typeChar = () => {
        if (targetIndex >= finalText.length) {
          setText(finalText);
          setShowCursor(false);
          schedule(() => {
            setShowCursor(true);
            runAnimation();
          }, 1000);
          return;
        }

        const targetChar = finalText[targetIndex];
        const shouldGlitch = Math.random() > 0.6 && targetChar !== " ";

        if (shouldGlitch) {
          currentText += randomWrongChar();
          setText(currentText);

          schedule(
            () => {
              currentText = currentText.slice(0, -1);
              setText(currentText);

              schedule(() => {
                if (Math.random() > 0.5) {
                  currentText += randomWrongChar();
                  setText(currentText);

                  schedule(() => {
                    currentText = currentText.slice(0, -1);
                    setText(currentText);

                    schedule(() => {
                      currentText += targetChar;
                      setText(currentText);
                      targetIndex++;
                      schedule(typeChar, 50 + Math.random() * 100);
                    }, 80);
                  }, 120);
                } else {
                  currentText += targetChar;
                  setText(currentText);
                  targetIndex++;
                  schedule(typeChar, 50 + Math.random() * 100);
                }
              }, 80);
            },
            100 + Math.random() * 150
          );
        } else {
          currentText += targetChar;
          setText(currentText);
          targetIndex++;
          schedule(typeChar, 40 + Math.random() * 80);
        }
      };

      setText("");
      setShowCursor(true);
      schedule(typeChar, 500);
    };

    runAnimation();

    return () => {
      for (const id of timeouts) {
        clearTimeout(id);
      }
      timeouts.clear();
    };
  }, [children, duration]);

  return (
    <div className={cn(className)}>
      <span aria-live="polite">
        {text}
        {showCursor ? (
          <motion.span
            animate={{ opacity: [1, 0, 1] }}
            aria-hidden
            transition={{
              duration: 0.8,
              ease: "linear",
              repeat: Number.POSITIVE_INFINITY,
            }}
          >
            |
          </motion.span>
        ) : null}
      </span>
    </div>
  );
}

demo.tsx
"use client";

import TextTypewriter from "@/components/ui/the-typewriter";

export function TerminalHeadline() {
  return (
    <TextTypewriter
      className="font-mono text-2xl text-foreground sm:text-4xl"
      duration={2.6}
    >
      Deploying interface motion
    </TextTypewriter>
  );
}

export default TerminalHeadline
```

Install NPM dependencies:
```bash
npm install motion
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
