<!-- Nebula · @chamaac · https://21st.dev/@chamaac/components/nebula
     license: MIT · category: hero
     An animated deep-space nebula background using a WebGL fractal noise shader with customizable colors and speed. -->

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
components/ui/nebula.tsx
"use client";

import React, { useRef, useMemo } from "react";
import { Canvas, useFrame, useThree } from "@react-three/fiber";
import * as THREE from "three";
import { cn } from "@/lib/utils";

const vertexShader = `
  varying vec2 vUv;
  void main() {
    vUv = uv;
    gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0);
  }
`;

const fragmentShader = `
uniform float uTime;
uniform vec3 uColor1;
uniform vec3 uColor2;
uniform vec3 uColor3;
uniform float uSpeed;

varying vec2 vUv;

// 2D Random
float random(in vec2 st) {
    return fract(sin(dot(st.xy, vec2(12.9898, 78.233))) * 43758.5453123);
}

// 2D Noise based on Morgan McGuire @morgan3d
// https://www.shadertoy.com/view/4dS3Wd
float noise(in vec2 st) {
    vec2 i = floor(st);
    vec2 f = fract(st);

    // Four corners in 2D of a tile
    float a = random(i);
    float b = random(i + vec2(1.0, 0.0));
    float c = random(i + vec2(0.0, 1.0));
    float d = random(i + vec2(1.0, 1.0));

    // Smooth Interpolation

    // Cubic Hermine Curve.  Same as SmoothStep()
    vec2 u = f * f * (3.0 - 2.0 * f);
    // u = smoothstep(0.,1.,f);

    // Mix 4 coorners percentages
    return mix(a, b, u.x) +
        (c - a) * u.y * (1.0 - u.x) +
        (d - b) * u.x * u.y;
}

#define OCTAVES 6
float fbm(in vec2 st) {
    // Initial values
    float value = 0.0;
    float amplitude = .5;
    float frequency = 0.;
    //
    // Loop of octaves
    for (int i = 0; i < OCTAVES; i++) {
        value += amplitude * noise(st);
        st *= 2.;
        amplitude *= .5;
    }
    return value;
}

void main() {
    vec2 st = vUv * 3.0;
    float time = uTime * uSpeed;
    
    // Domain Warping
    vec2 q = vec2(0.);
    q.x = fbm(st + 0.00 * time); 
    q.y = fbm(st + vec2(1.0));

    vec2 r = vec2(0.);
    r.x = fbm(st + 1.0 * q + vec2(1.7, 9.2) + 0.15 * time); 
    r.y = fbm(st + 1.0 * q + vec2(8.3, 2.8) + 0.126 * time);

    float f = fbm(st + r);

    // Color Mixing
    vec3 color = mix(uColor3, uColor2, clamp((f * f) * 4.0, 0.0, 1.0));
    color = mix(color, uColor1, clamp(length(q), 0.0, 1.0));
    color = mix(color, vec3(1.0), clamp(length(r.x), 0.0, 1.0));

    // Darken edges
    float vignette = 1.0 - smoothstep(0.5, 1.5, length(vUv - 0.5));
    color *= vignette;


    gl_FragColor = vec4((f * f * f + .6 * f * f + .5 * f) * color, 1.0);
}
`;

const NebulaMaterial = ({
  speed,
  color1,
  color2,
  color3,
}: {
  speed: number;
  color1: string;
  color2: string;
  color3: string;
}) => {
  const materialRef = useRef<THREE.ShaderMaterial>(null);
  const lastTimeRef = useRef(0);

  const uniforms = useMemo(
    () => ({
      uTime: { value: 0 },
      uSpeed: { value: speed },
      uColor1: { value: new THREE.Color(color1) },
      uColor2: { value: new THREE.Color(color2) },
      uColor3: { value: new THREE.Color(color3) },
    }),

    [] // Initialize once
  );

  // Update uniforms when props change
  React.useEffect(() => {
    uniforms.uSpeed.value = speed;
    uniforms.uColor1.value.set(color1);
    uniforms.uColor2.value.set(color2);
    uniforms.uColor3.value.set(color3);
  }, [speed, color1, color2, color3, uniforms]);

  useFrame((state) => {
    if (!materialRef.current) return;
    // Cap render to ~30 fps — nebula moves slowly so this is imperceptible
    const elapsed = state.clock.getElapsedTime();
    if (elapsed - lastTimeRef.current < 1 / 30) return;
    lastTimeRef.current = elapsed;
    materialRef.current.uniforms.uTime.value = elapsed;
  });

  const { viewport } = useThree();

  return (
    <mesh>
      <planeGeometry args={[viewport.width, viewport.height]} />
      <shaderMaterial
        ref={materialRef}
        vertexShader={vertexShader}
        fragmentShader={fragmentShader}
        uniforms={uniforms}
      />
    </mesh>
  );
};

interface NebulaProps {
  className?: string;
  speed?: number;
  color1?: string; // Highlights/Fracture
  color2?: string; // Nebula main
  color3?: string; // Deep space
}

export default function Nebula({
  className,
  speed = 2.0,
  color1 = "#5efff4", // Cyan (Highlight)
  color2 = "#763b65", // Magenta-ish (Nebula)
  color3 = "#1a0b2e", // Deep purple (Deep Space)
}: NebulaProps) {
  return (
    <div
      className={cn(
        "absolute inset-0 w-full h-full pointer-events-none",
        className
      )}
    >
      <Canvas
        camera={{ position: [0, 0, 1] }}
        dpr={[1, 2]}
        gl={{ antialias: false, powerPreference: "high-performance" }}
      >
        <NebulaMaterial
          speed={speed}
          color3={color3}
          color2={color2}
          color1={color1}
        />
      </Canvas>
    </div>
  );
}

demo.tsx
import Nebula from "@/components/ui/nebula";

export default function NebulaDemo() {
  return (
    <div className="relative h-[500px] w-full overflow-hidden rounded-xl bg-black">
      <Nebula />
      <div className="relative z-10 flex h-full flex-col items-center justify-center gap-4 text-center">
        <h1 className="bg-gradient-to-b from-white to-white/60 bg-clip-text text-5xl font-semibold tracking-tight text-transparent">
          Explore the Nebula
        </h1>
        <p className="max-w-md text-balance text-white/70">
          A deep space nebula effect rendered with fractional distortion shaders.
        </p>
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @react-three/fiber clsx tailwind-merge three
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
