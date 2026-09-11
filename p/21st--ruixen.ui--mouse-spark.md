<!-- Mouse Spark · @ruixen.ui · https://21st.dev/@ruixen.ui/components/mouse-spark
     license: unspecified · category: cursor
     The MouseSpark component is a visually appealing, interactive cursor particle effect for web applications. It automatically adjusts to light and dark themes, seamlessly blending with white or black backgrounds. When the user moves their mouse, colorful particles are spawned at the cursor position, leaving smooth, fading trails that create a dynamic, sparkling effect. The component is fully responsive, covering the entire viewport and adapting to window resizing, while remaining non-intrusive with pointer-events disabled so it doesn’t interfere with user interactions. Designed for elegance and performance, it continuously removes old particles to ensure smooth animation, providing a modern and engaging visual enhancement for websites or applications without requiring any props or configuration. -->

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
components/ui/mouse-spark.tsx
"use client";

import React, { useEffect, useRef } from "react";

interface MouseSparkProps {
  width?: number;
  height?: number;
  theme?: "light" | "dark";
}

const MouseSpark: React.FC<MouseSparkProps> = ({
  width = typeof window !== "undefined" ? window.innerWidth : 800,
  height = typeof window !== "undefined" ? window.innerHeight : 600,
  theme = "light",
}) => {
  const canvasRef = useRef<HTMLCanvasElement | null>(null);

  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;

    canvas.width = width;
    canvas.height = height;
    const ctx = canvas.getContext("2d");
    if (!ctx) return;

    // Theme colors
    const colors =
      theme === "dark"
        ? ["#ff6b6b", "#feca57", "#48dbfb", "#1dd1a1", "#5f27cd"]
        : ["#ff7f50", "#ffb347", "#00d2ff", "#76e4f7", "#ff85a2"];

    // Particles
    let particles: {
      x: number;
      y: number;
      dx: number;
      dy: number;
      color: string;
    }[] = [];

    const spawnParticles = (x: number, y: number) => {
      for (let i = 0; i < 3; i++) {
        const angle = Math.random() * Math.PI * 2;
        const speed = Math.random() * 3 + 0.5;
        const color = colors[Math.floor(Math.random() * colors.length)];
        particles.push({
          x,
          y,
          dx: Math.cos(angle) * speed,
          dy: Math.sin(angle) * speed,
          color,
        });
      }
    };

    // Mouse move event
    const handleMouseMove = (e: MouseEvent) => {
      spawnParticles(e.clientX, e.clientY);
    };

    const animate = () => {
      if (!ctx) return;

      ctx.clearRect(0, 0, canvas.width, canvas.height);

      particles.forEach((p, i) => {
        p.x += p.dx;
        p.y += p.dy;
        p.dx *= 0.92;
        p.dy *= 0.92;

        ctx.fillStyle = p.color;
        ctx.beginPath();
        ctx.arc(p.x, p.y, 4, 0, Math.PI * 2);
        ctx.fill();

        // Remove slow particles
        if (Math.abs(p.dx) < 0.05 && Math.abs(p.dy) < 0.05) {
          particles.splice(i, 1);
        }
      });

      requestAnimationFrame(animate);
    };

    canvas.addEventListener("mousemove", handleMouseMove);
    animate();

    return () => {
      canvas.removeEventListener("mousemove", handleMouseMove);
    };
  }, [width, height, theme]);

  return <canvas ref={canvasRef} style={{ display: "block" }} />;
};

export default MouseSpark;

demo.tsx
"use client";

import React, { useState } from "react";
import MouseSpark from "@/components/ui/mouse-spark";

export default function HomePage() {
  const [theme, setTheme] = useState<"light" | "dark">("dark");

  return (
    <div style={{ width: "100vw", height: "100vh", overflow: "hidden" }}>
      <MouseSpark />
      <div
        style={{
          position: "absolute",
          top: "50%",
          left: "50%",
          transform: "translate(-50%, -50%)",
          color: theme === "dark" ? "#000" : "#fff",
          fontSize: "1rem",
          textAlign: "center",
        }}
      >
        Move your mouse to see the splash effect!
      </div>
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
