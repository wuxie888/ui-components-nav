<!-- Orbiting Circles with Globe · @shadcnspace · https://21st.dev/@shadcnspace/components/orbiting-circles-02
     license: MIT · category: globe
     Full-display orbiting rings of integration icons circling a center particle globe rendered on canvas. -->

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
components/shadcn-space/orbiting-circles/orbiting-circles-02.tsx
"use client";

import React from "react";
import ParticleSphereAnimation from "@/components/shadcn-space/orbiting-circles/particalsphear";

const orbits = [
  {
    size: "w-110 h-110 md:w-180 md:h-180",
    duration: 18,
    icons: [
      { src: "https://images.shadcnspace.com/assets/svgs/supabase.svg", alt: "Supabase", angle: -60 },
      { src: "https://images.shadcnspace.com/assets/svgs/gemini.svg", alt: "gemini", angle: 0 },
      { src: "https://images.shadcnspace.com/assets/svgs/make.svg", alt: "Make", angle: 60 },
    ],
  },
  {
    size: "w-150 h-150 md:w-220 md:h-220",
    duration: 24,
    icons: [
      { src: "https://images.shadcnspace.com/assets/svgs/figma.svg", alt: "Figma", angle: 0 },
      { src: "https://images.shadcnspace.com/assets/svgs/slack.svg", alt: "Slack", angle: -90 },
    ],
  },
  {
    size: "w-180 h-180 md:w-265 md:h-265",
    duration: 30,
    icons: [
      { src: "https://images.shadcnspace.com/assets/svgs/clude.svg", alt: "Claude", angle: -60 },
      { src: "https://images.shadcnspace.com/assets/svgs/react.svg", alt: "react", angle: 0 },
      { src: "https://images.shadcnspace.com/assets/svgs/python.svg", alt: "python", angle: 60 },
    ],
  },
];

export default function OrbitingCirclesGlobeDemo() {
  return (
    <div className="relative w-full h-110 md:h-160 overflow-hidden flex justify-center">
      <style>{`
        @keyframes orbit-cw {
          from { transform: rotate(var(--start-angle)) }
          to   { transform: rotate(calc(var(--start-angle) + 360deg)) }
        }
        @keyframes orbit-ccw {
          from { transform: rotate(var(--start-angle)) }
          to   { transform: rotate(calc(var(--start-angle) - 360deg)) }
        }
        @keyframes counter-cw {
          from { transform: rotate(var(--counter-offset, 0deg)) }
          to   { transform: rotate(calc(var(--counter-offset, 0deg) - 360deg)) }
        }
        @keyframes counter-ccw {
          from { transform: rotate(var(--counter-offset, 0deg)) }
          to   { transform: rotate(calc(var(--counter-offset, 0deg) + 360deg)) }
        }
      `}</style>

      {/* Center particle globe */}
      <div className="absolute bottom-0 left-1/2 -translate-x-1/2 translate-y-1/2 aspect-square pointer-events-none w-75 md:w-145 z-10">
        <ParticleSphereAnimation />
      </div>

      {/* Orbiting rings */}
      {orbits.map((orbit, index) => {
        const isCW = index % 2 === 0;
        const orbitAnim = isCW ? "orbit-cw" : "orbit-ccw";
        const counterAnim = isCW ? "counter-cw" : "counter-ccw";

        const allIcons = [
          ...orbit.icons,
          ...orbit.icons.map((ic) => ({
            ...ic,
            angle: ic.angle + 180,
            alt: `${ic.alt}-mirror`,
          })),
        ];

        return (
          <div
            key={index}
            className={`absolute bottom-0 left-1/2 -translate-x-1/2 translate-y-1/2 rounded-full border border-border ${orbit.size}`}
          >
            {allIcons.map((iconData, iconIndex) => (
              <div
                key={iconIndex}
                className="absolute top-0 left-1/2 h-1/2 -ml-8 origin-bottom flex flex-col justify-start items-center"
                style={
                  {
                    "--start-angle": `${iconData.angle}deg`,
                    animation: `${orbitAnim} ${orbit.duration}s linear infinite`,
                  } as React.CSSProperties
                }
              >
                <div
                  className="p-3 sm:p-4 border border-border rounded-full bg-background -mt-8 relative z-10"
                  style={
                    {
                      "--counter-offset": `${-iconData.angle}deg`,
                      animation: `${counterAnim} ${orbit.duration}s linear infinite`,
                    } as React.CSSProperties
                  }
                >
                  <img
                    src={iconData.src}
                    alt={iconData.alt}
                    width={32}
                    height={32}
                    className="w-6 h-6 md:w-8 md:h-8"
                  />
                </div>
              </div>
            ))}
          </div>
        );
      })}
    </div>
  );
}

components/shadcn-space/orbiting-circles/particalsphear.tsx
"use client";

import { useEffect, useRef, useMemo } from "react";
import { cn } from "@/lib/utils";

// Total number of particles to render in the sphere
const PARTICLE_COUNT = 9000;
// Physical radius of the sphere
const RADIUS = 275;

// Color palette for the particles - contains blues, oranges, greens, and highlights
const COLORS = [
    "#ea580c",
    "#d97706",
    "#84cc16",
    "#f1f5f9",
    "#94a3b8",
    "#2563eb",
    "#3b82f6",
    "#60a5fa",
    "#f97316",
];

/**
 * Generates a set of 3D points distributed on the surface of a sphere.
 * Uses Archimedes' theorem for uniform distribution across the sphere's surface.
 */
function generateSpherePoints(count: number) {
    const points = [];

    for (let i = 0; i < count; i++) {
        // Archimedes' Theorem / Lambert's Cylindrical Projection:
        // Distributing points uniformly on the Z axis and then finding the corresponding circle radius at that height.
        const z = Math.random() * 2 - 1; // Range: -1 to 1
        const theta = Math.random() * 2 * Math.PI; // Full rotation in radians
        const r_at_z = Math.sqrt(1 - z * z); // Radius of the circle at this Z coordinate (Pythagorean)

        // Add 6% thickness/volume to the "shell" so it looks natural and not like a perfect math skin
        const r = RADIUS * (0.97 + Math.random() * 0.06);

        // Convert spherical/cylindrical orientation to Cartesian (X, Y, Z) coordinates
        const x = r * r_at_z * Math.cos(theta);
        const y = r * r_at_z * Math.sin(theta);
        const point_z = r * z;

        // Assign a color group based on the Y-position (Vertical distribution)
        let colorIndex;
        const yFactor = (y + RADIUS) / (2 * RADIUS); // Normalize Y position to 0.0 - 1.0 range

        if (Math.random() > 0.9) {
            // 10% chance to be a bright white "star" highlight
            colorIndex = 7;
        } else if (yFactor > 0.6) {
            // Top section of the sphere tends towards Blues
            colorIndex = Math.floor(Math.random() * 3);
        } else if (yFactor < 0.4) {
            // Bottom section tends towards Oranges/Warm tones
            colorIndex = 3 + Math.floor(Math.random() * 3);
        } else {
            // Middle section is a mix of all colors
            colorIndex = Math.floor(Math.random() * COLORS.length);
        }

        points.push({ x, y, z: point_z, color: COLORS[colorIndex] });
    }

    return points;
}

export default function ParticleSphereAnimation({
    className,
}: {
    className?: string;
}) {
    const canvasRef = useRef<HTMLCanvasElement>(null);
    const points = useMemo(() => generateSpherePoints(PARTICLE_COUNT), []);
    const rotationRef = useRef(0);
    const animationFrameRef = useRef<number | undefined>(undefined);

    useEffect(() => {
        const canvas = canvasRef.current;
        if (!canvas) return;

        const ctx = canvas.getContext("2d", { 
            alpha: true,
            willReadFrequently: false
        });
        if (!ctx) return;

        // Set canvas size
        const size = 575;
        canvas.width = size;
        canvas.height = size;

        // Enable image smoothing for better anti-aliasing
        ctx.imageSmoothingEnabled = true;
        ctx.imageSmoothingQuality = "high";

        // Animation loop
        const animate = () => {
            // Clear the canvas for transparent background
            ctx.clearRect(0, 0, size, size);

            // Update rotation
            rotationRef.current += 0.003;

            // Translate to center
            ctx.save();
            ctx.translate(size / 2, size / 2);

            // Sort points by depth (z-order) for proper rendering
            const rotatedPoints = points.map((p) => {
                // Standard 3D Rotation Math around the Y-axis
                const cos = Math.cos(rotationRef.current);
                const sin = Math.sin(rotationRef.current);

                // Calculate new X and Z coordinates after rotation
                const x = p.x * cos - p.z * sin;
                const z = p.x * sin + p.z * cos;

                // Perspective/Depth calculations:
                // scale factor is 0 (back of sphere) to 1 (front of sphere)
                const scale = (z + RADIUS) / (2 * RADIUS);

                // Rim effect: Calculate 2D distance from the center to fade out the middle
                const distFromCenter = Math.sqrt(x * x + p.y * p.y);
                const rimFactor = Math.min(distFromCenter / RADIUS, 1);

                /**
                 * Rendering Rules:
                 * 1. Opacity: Particles near the edge (rim) are denser/more opaque. Particles in front are clearer.
                 * 2. Size: Particles in front (higher Z) appear larger to simulate perspective.
                 */
                const opacity = Math.max(0.1, Math.pow(rimFactor, 3) * 0.8) * (0.4 + 0.6 * scale);
                const size = (0.4 + 0.8 * scale) * 1.5;

                return { x, y: p.y, z, color: p.color, opacity, size, scale };
            });

            // Sort by z-depth (back to front)
            rotatedPoints.sort((a, b) => a.z - b.z);

            // Draw particles
            rotatedPoints.forEach((p) => {
                ctx.beginPath();
                ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
                ctx.fillStyle = p.color;
                ctx.globalAlpha = p.opacity;
                ctx.fill();
            });

            // Reset globalAlpha to prevent state leakage
            ctx.globalAlpha = 1.0;

            ctx.restore();

            animationFrameRef.current = requestAnimationFrame(animate);
        };

        animate();

        return () => {
            if (animationFrameRef.current) {
                cancelAnimationFrame(animationFrameRef.current);
            }
        };
    }, []);

    return (
        <div
            className={cn(
                "mx-auto w-full",
                className
            )}
        >
            <canvas
                ref={canvasRef}
                className="rounded-full select-none pointer-events-none w-full h-auto mx-auto max-w-[575px]"
                width={575}
                height={575}
            />
        </div>
    );
}

demo.tsx
import OrbitingCirclesGlobe from "@/components/ui/orbiting-circles-02";

export default function Demo() {
  return (
    <div className="flex min-h-[500px] w-full items-end justify-center bg-background">
      <OrbitingCirclesGlobe />
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
