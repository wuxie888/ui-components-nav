<!-- Animated Text · @bundui · https://21st.dev/@bundui/components/animated-text
     license: unspecified · category: text
     Here is Animated Text component -->

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
components/ui/animated-text.tsx
"use client";

import { easeOut, motion } from "motion/react";

interface AnimatedTextProps {
  text: string;
  className?: string;
  animationType?: "letters" | "words";
  duration?: number;
  delay?: number;
  staggerDelay?: number;
  initialY?: number;
  initialOpacity?: number;
  animateY?: number;
  animateOpacity?: number;
}

export default function AnimatedText({
  text,
  className = "text-4xl font-bold",
  animationType = "letters",
  duration = 0.6,
  delay = 0,
  staggerDelay = 0.05,
  initialY = 10,
  initialOpacity = 0,
  animateY = 0,
  animateOpacity = 1
}: AnimatedTextProps) {
  const containerVariants = {
    hidden: { opacity: 0 },
    visible: {
      opacity: 1,
      transition: {
        staggerChildren: staggerDelay,
        delayChildren: delay
      }
    }
  };

  const itemVariants = {
    hidden: {
      y: initialY,
      opacity: initialOpacity
    },
    visible: {
      y: animateY,
      opacity: animateOpacity,
      transition: {
        duration: duration,
        ease: easeOut
      }
    }
  };

  const renderLetters = () => {
    return text.split("").map((char, index) => (
      <motion.span
        key={`letter-${index}`}
        variants={itemVariants}
        className="inline-block"
        style={{ whiteSpace: char === " " ? "pre" : "normal" }}>
        {char}
      </motion.span>
    ));
  };

  const renderWords = () => {
    return text.split(" ").map((word, index) => (
      <motion.span key={`word-${index}`} variants={itemVariants} className="mr-2 inline-block">
        {word}
      </motion.span>
    ));
  };

  return (
    <motion.div
      className={className}
      variants={containerVariants}
      initial="hidden"
      animate="visible">
      {animationType === "letters" ? renderLetters() : renderWords()}
    </motion.div>
  );
}

components/ui/page.tsx
import AnimatedText from "./animated-text";

export default function Example() {
  return (
    <AnimatedText
      text="Welcome to the Future"
      className="text-4xl font-bold"
      animationType="letters"
      staggerDelay={0.08}
      duration={0.6}
    />
  );
}

demo.tsx
import AnimatedGradientText from "@/components/ui/animated-text";

export default function AnimatedGradientTextExample() {
  return <AnimatedGradientText text="Thinking..." className="font-semibold lg:text-2xl" />;
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add animated-text.json
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
