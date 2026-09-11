<!-- Animated Verification OTP Input · @shadcnspace · https://21st.dev/@shadcnspace/components/input-otp-10
     license: MIT · category: input
     A 6-digit one-time-passcode input with an animated rotating particle-sphere avatar, glowing typing feedback per slot, and a success state shown once the code is complete. -->

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
components/shadcn-space/input-otp/input-otp-10.tsx
"use client";

import { useState, useEffect, useRef, useContext } from "react";
import ParticleSphereAnimation from "@/components/shadcn-space/input-otp/particalsphear";
import { REGEXP_ONLY_DIGITS } from "input-otp";
import { OTPInput, OTPInputContext } from "input-otp";
import { cn } from "@/lib/utils";
import { motion, AnimatePresence } from "motion/react";
import { ShieldCheck } from "lucide-react";

const SPRING_TRANSITION = {
  type: "spring",
  stiffness: 450,
  damping: 28,
} as const;
const CustomOTPSlot = ({ index, isSuccess }: { index: number; isSuccess: boolean }) => {
  const inputOTPContext = useContext(OTPInputContext);
  const { char, hasFakeCaret, isActive } = inputOTPContext?.slots[index] ?? {};

  const [pulseKey, setPulseKey] = useState(0);
  const prevCharRef = useRef(char);

  useEffect(() => {
    if (char && char !== prevCharRef.current) {
      setPulseKey((prev) => prev + 1);
    }
    prevCharRef.current = char;
  }, [char]);

  return (
    <div
      className={cn(
        "relative flex h-14 w-11 sm:w-12 items-center justify-center rounded-xl border transition-all duration-300 font-mono text-xl font-bold select-none",
        isSuccess
          ? "border-emerald-500 bg-emerald-500/10 text-emerald-400"
          : isActive
          ? "border-primary bg-primary/5 text-foreground"
          : "border-border/60 bg-muted/10 text-muted-foreground hover:border-muted-foreground/30 hover:bg-muted/20"
      )}
    >

      <AnimatePresence mode="popLayout">
        {char ? (
          <motion.span
            key={`char-${char}`}
            initial={{ opacity: 0, scale: 0.5, y: 6 }}
            animate={{ opacity: 1, scale: 1, y: 0 }}
            exit={{ opacity: 0, scale: 0.7, y: -6 }}
            transition={SPRING_TRANSITION}
            className={cn("absolute font-mono text-xl", isSuccess ? "text-emerald-400" : "text-foreground")}
          >
            {char}
          </motion.span>
        ) : null}
      </AnimatePresence>
      <AnimatePresence>
        {pulseKey > 0 && (
          <motion.div
            key={pulseKey}
            className="absolute inset-0 rounded-xl border border-primary pointer-events-none"
            initial={{ opacity: 0.8, scale: 0.9, filter: "blur(0px)" }}
            animate={{ opacity: 0, scale: 1.5, filter: "blur(2px)" }}
            exit={{ opacity: 0 }}
            transition={{ duration: 0.4, ease: "easeOut" }}
          />
        )}
      </AnimatePresence>
      {hasFakeCaret && !isSuccess && (
        <div className="pointer-events-none absolute inset-0 flex items-center justify-center">
          <motion.div
            className="bg-primary h-6 w-0.5"
            animate={{ opacity: [1, 0, 1] }}
            transition={{
              repeat: Infinity,
              duration: 1,
              ease: "easeInOut",
            }}
          />
        </div>
      )}
    </div>
  );
};

export default function InputOtp10() {
  const [value, setValue] = useState("");
  const [timer, setTimer] = useState(30);
  const [isSuccess, setIsSuccess] = useState(false);

  useEffect(() => {
    if (value.length === 6) {
      setIsSuccess(true);
    } else {
      setIsSuccess(false);
    }
  }, [value]);

  useEffect(() => {
    if (timer === 0) return;
    const interval = setInterval(() => {
      setTimer((t) => t - 1);
    }, 1000);
    return () => clearInterval(interval);
  }, [timer]);

  return (
    <div className="relative w-full max-w-sm sm:max-w-md mx-auto rounded-2xl border border-border bg-linear-to-b from-card to-card/60 p-6 sm:p-8 backdrop-blur-xl overflow-hidden group select-none">
      <div className="absolute -top-12 -right-12 w-32 h-32 bg-primary/10 rounded-full blur-2xl pointer-events-none group-hover:bg-primary/15 transition-colors duration-500" />
      <div className="absolute -bottom-12 -left-12 w-32 h-32 bg-primary/5 rounded-full blur-2xl pointer-events-none" />
      <div className="flex flex-col items-center gap-6 sm:gap-7">
        <div className="relative w-40 h-40 flex items-center justify-center">
          <div 
            className="absolute inset-0 rounded-full border border-dashed border-primary/30 animate-spin pointer-events-none" 
            style={{ animationDuration: "60s" }}
          />
          <div 
            className="absolute inset-2 rounded-full border border-primary/15 animate-spin pointer-events-none" 
            style={{ animationDuration: "30s", animationDirection: "reverse" }}
          />
          <div className="absolute inset-4 rounded-full border border-dashed border-primary/5 pointer-events-none" />
          <div 
            className={cn(
              "absolute -inset-1.5 rounded-full transition-all duration-500",
              isSuccess 
                ? "bg-linear-to-t from-emerald-500/0 via-emerald-500/10 to-emerald-500/0 animate-pulse"
                : "bg-linear-to-t from-primary/0 via-primary/5 to-primary/0 animate-pulse"
            )}
            style={!isSuccess ? { animationDuration: "3s" } : undefined}
          />
          <div className={cn(
            "w-32 h-32 rounded-full overflow-hidden flex items-center justify-center transition-all duration-500",
            isSuccess ? "scale-105" : "scale-95 group-hover:scale-100"
          )}>
            <ParticleSphereAnimation className="w-full h-full scale-135 opacity-90 group-hover:opacity-100 transition-opacity duration-300" />
          </div>
          <AnimatePresence>
            {isSuccess && (
              <motion.div
                initial={{ opacity: 0, scale: 0.6 }}
                animate={{ opacity: 1, scale: 1 }}
                exit={{ opacity: 0, scale: 0.6 }}
                className="absolute inset-0 flex items-center justify-center bg-background/40 backdrop-blur-xs rounded-full"
              >
                <div className="p-3 rounded-full bg-emerald-500/20 border border-emerald-500/40 text-emerald-400">
                  <ShieldCheck className="w-8 h-8" />
                </div>
              </motion.div>
            )}
          </AnimatePresence>
        </div>
        <div className="text-center flex flex-col gap-1.5">
          <h3 className="text-sm font-bold tracking-tight text-foreground uppercase">
            {isSuccess ? "System Key Decrypted" : "Enter Credentials"}
          </h3>
          <p className="text-xs text-muted-foreground max-w-2xs leading-relaxed">
            {isSuccess 
              ? "Session authorization successfully validated by Security Hub."
              : "Please input the 6-digit verification code sent to your dynamic authenticator node."}
          </p>
        </div>
        <div className="w-full flex justify-center">
          <OTPInput
            maxLength={6}
            value={value}
            onChange={setValue}
            pattern={REGEXP_ONLY_DIGITS}
            containerClassName="group flex items-center justify-center gap-2 sm:gap-2.5"
          >
            <div className="flex items-center gap-2 sm:gap-2.5">
              {Array.from({ length: 6 }).map((_, idx) => (
                <CustomOTPSlot key={idx} index={idx} isSuccess={isSuccess} />
              ))}
            </div>
          </OTPInput>
        </div>
        <div className="h-4 flex items-center justify-center">
          <span className="text-xs font-mono text-muted-foreground/70 uppercase tracking-widest">
            {value.length === 0 
              ? "Awaiting key signature..."
              : value.length < 6 
              ? `Entering signature: ${value.length} / 6`
              : "Signature matching complete"}
          </span>
        </div>
      </div>
    </div>
  );
}

components/shadcn-space/input-otp/particalsphear.tsx
"use client";

import { useEffect, useRef, useMemo } from "react";
import { cn } from "@/lib/utils";

// Total number of particles to render in the sphere
const PARTICLE_COUNT = 5000;
// Physical radius of the sphere
const RADIUS = 275;

// Color palette for the particles - contains blues, oranges, greens, and highlights
const COLORS = [
    "#2563eb",
    "#3b82f6",
    "#60a5fa",
    "#f97316",
    "#ea580c",
    "#d97706",
    "#84cc16",
    "#f1f5f9",
    "#94a3b8",
];

/**
 * Generates a set of 3D points distributed on the surface of a sphere.
 * Uses Lambert's Cylindrical Projection for uniform distribution.
 */
function generateSpherePoints(count: number) {
    const points = [];

    for (let i = 0; i < count; i++) {
        const z = Math.random() * 2 - 1; // Range: -1 to 1
        const theta = Math.random() * 2 * Math.PI; // Full rotation in radians
        const r_at_z = Math.sqrt(1 - z * z); // Radius of the circle at this Z coordinate

        // Add 6% thickness/volume to the shell
        const r = RADIUS * (0.97 + Math.random() * 0.06);

        // Convert spherical orientation to Cartesian coordinates
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

            // Map and project points
            const rotatedPoints = points.map((p) => {
                // Standard 3D Rotation Math around the Y-axis
                const cos = Math.cos(rotationRef.current);
                const sin = Math.sin(rotationRef.current);

                // Calculate new X and Z coordinates after rotation
                const x = p.x * cos - p.z * sin;
                const z = p.x * sin + p.z * cos;

                // Perspective/Depth calculations (0 to 1 scale)
                const scale = (z + RADIUS) / (2 * RADIUS);

                // Rim effect: Calculate 2D distance from the center to fade out the middle
                const distFromCenter = Math.sqrt(x * x + p.y * p.y);
                const rimFactor = Math.min(distFromCenter / RADIUS, 1);

                // Hollow center fading effect (denser at edges, slightly faded in middle)
                const opacity = Math.max(0.22, Math.pow(rimFactor, 2) * 0.8) * (0.4 + 0.6 * scale);
                const size = (0.5 + 0.8 * scale) * 2.6;

                return { x, y: p.y, z, color: p.color, opacity, size };
            });

            // Sort by z-depth (back to front) for proper 3D rendering layered layout
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
    }, [points]);

    return (
        <div
            className={cn(
                "mx-auto w-full",
                className
            )}
        >
            <canvas
                ref={canvasRef}
                className="rounded-full select-none pointer-events-none w-full h-auto max-w-[575px]"
                width={575}
                height={575}
            />
        </div>
    );
}

demo.tsx
import InputOtp10 from "@/components/ui/input-otp-10";

export default function InputOtp10Demo() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center p-6">
      <InputOtp10 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install input-otp lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add input-otp
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
