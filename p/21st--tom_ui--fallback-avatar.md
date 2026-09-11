<!-- Fallback Avatar · @tom_ui · https://21st.dev/@tom_ui/components/fallback-avatar
     license: MIT · category: team
     A unique gradient avatar generated from a name string. -->

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
components/ui/fallback-avatar.tsx
"use client";

import { useCallback, useEffect, useRef, useState } from "react";
import { cn } from "@/lib/utils";

// --- Hash utilities ---

function hashString(str: string): [number, number] {
  let h1 = 0xdeadbeef;
  let h2 = 0x41c6ce57;
  for (let i = 0; i < str.length; i++) {
    const ch = str.charCodeAt(i);
    h1 = Math.imul(h1 ^ ch, 2654435761);
    h2 = Math.imul(h2 ^ ch, 1597334677);
  }
  h1 = Math.imul(h1 ^ (h1 >>> 16), 2246822507);
  h1 ^= Math.imul(h2 ^ (h2 >>> 13), 3266489909);
  h2 = Math.imul(h2 ^ (h2 >>> 16), 2246822507);
  h2 ^= Math.imul(h1 ^ (h1 >>> 13), 3266489909);
  return [h1 >>> 0, h2 >>> 0];
}

function mulberry32(seed: number) {
  return function () {
    seed |= 0;
    seed = (seed + 0x6d2b79f5) | 0;
    let t = Math.imul(seed ^ (seed >>> 15), 1 | seed);
    t = (t + Math.imul(t ^ (t >>> 7), 61 | t)) ^ t;
    return ((t ^ (t >>> 14)) >>> 0) / 4294967296;
  };
}

function deriveHue(hash: [number, number]): number {
  const bytes: number[] = [];
  for (let i = 0; i < 4; i++) {
    bytes.push((hash[0] >> (i * 8)) & 0xff);
    bytes.push((hash[1] >> (i * 8)) & 0xff);
  }
  return bytes.reduce((a, b) => a + b, 0) % 360;
}

function oklchToRgb(
  L: number,
  C: number,
  H: number
): [number, number, number] {
  const hRad = (H * Math.PI) / 180;
  const a = C * Math.cos(hRad);
  const b = C * Math.sin(hRad);
  const l_ = L + 0.3963377774 * a + 0.2158037573 * b;
  const m_ = L - 0.1055613458 * a - 0.0638541728 * b;
  const s_ = L - 0.0894841775 * a - 1.291485548 * b;
  const l = l_ * l_ * l_;
  const m = m_ * m_ * m_;
  const s = s_ * s_ * s_;
  let R = +4.0767416621 * l - 3.3077115913 * m + 0.2309699292 * s;
  let G = -1.2684380046 * l + 2.6097574011 * m - 0.3413193965 * s;
  let B = -0.0041960863 * l - 0.7034186147 * m + 1.707614701 * s;
  const gamma = (v: number) =>
    v <= 0.0031308 ? 12.92 * v : 1.055 * Math.pow(v, 1 / 2.4) - 0.055;
  return [
    Math.round(Math.max(0, Math.min(1, gamma(R))) * 255),
    Math.round(Math.max(0, Math.min(1, gamma(G))) * 255),
    Math.round(Math.max(0, Math.min(1, gamma(B))) * 255),
  ];
}

function getColors(
  hash: [number, number]
): [[number, number, number], [number, number, number]] {
  const hue = deriveHue(hash);
  return [oklchToRgb(0.8, 0.18, hue), oklchToRgb(0.45, 0.18, hue)];
}

// --- Uniforms ---

interface Uniforms {
  S: number;
  H: number;
  P: [number, number, number, number];
  Q: [number, number, number, number];
  C1: [number, number, number];
  C2: [number, number, number];
}

function computeUniforms(name: string): Uniforms {
  const hash = hashString(name);
  const rng = mulberry32(hash[0]);
  const [c1, c2] = getColors(hash);
  const p: number[] = [];
  for (let i = 0; i < 8; i++) p.push(rng());
  return {
    S: hash[0] / 4294967296,
    H: deriveHue(hash) / 360,
    P: [p[0], p[1], p[2], p[3]],
    Q: [p[4], p[5], p[6], p[7]],
    C1: [c1[0] / 255, c1[1] / 255, c1[2] / 255],
    C2: [c2[0] / 255, c2[1] / 255, c2[2] / 255],
  };
}

// --- WebGL ---

const VERT_SRC = "attribute vec2 a;void main(){gl_Position=vec4(a,0,1);}";

const FRAG_SRC = `precision mediump float;
uniform vec2 R;
uniform float S,H;
uniform vec4 P,Q;
uniform vec3 C1,C2;
uniform float T;
#define UV (gl_FragCoord.xy/R)

void main(){
  vec2 uv=UV;
  vec2 b1=vec2(P.x,P.y);
  vec2 b2=vec2(P.z,P.w);
  vec2 b3=vec2(Q.x,Q.y);
  vec2 c1=b1+vec2(sin(T*.7+b1.x*6.)*.08, cos(T*.9+b1.y*6.)*.08);
  vec2 c2=b2+vec2(sin(T*.6+2.1)*.1, cos(T*.8+1.3)*.07);
  vec2 c3=b3+vec2(sin(T*.5+4.2)*.07, cos(T*1.1+3.7)*.09);
  float breath=1.+sin(T*1.3)*.06;
  float d1=(1.-length(uv-c1)*1.5)*breath;
  float d2=(1.-length(uv-c2)*1.5)*(1.+sin(T*1.7+1.)*.05);
  float d3=(1.-length(uv-c3)*1.5)*(1.+sin(T*1.1+2.)*.05);
  vec3 col=vec3(0);
  col=1.-(1.-col)*(1.-C1*max(d1,0.));
  col=1.-(1.-col)*(1.-C2*max(d2,0.));
  vec3 c3col=mix(C1,C2,.5+sin(T*.4)*.15);
  col=1.-(1.-col)*(1.-c3col*max(d3,0.));
  col=clamp(col,0.,1.);
  gl_FragColor=vec4(col,1);
}`;

let _glCanvas: HTMLCanvasElement | null = null;
let _gl: WebGLRenderingContext | null = null;
let _vShader: WebGLShader | null = null;
let _qBuf: WebGLBuffer | null = null;
let _prog: WebGLProgram | null = null;

function ensureGL() {
  if (_gl) return;
  _glCanvas = document.createElement("canvas");
  _glCanvas.width = 64;
  _glCanvas.height = 64;
  _gl = _glCanvas.getContext("webgl", {
    antialias: false,
    depth: false,
    preserveDrawingBuffer: true,
  });
  if (!_gl) return;

  _vShader = _gl.createShader(_gl.VERTEX_SHADER)!;
  _gl.shaderSource(_vShader, VERT_SRC);
  _gl.compileShader(_vShader);

  _qBuf = _gl.createBuffer();
  _gl.bindBuffer(_gl.ARRAY_BUFFER, _qBuf);
  _gl.bufferData(
    _gl.ARRAY_BUFFER,
    new Float32Array([-1, -1, 1, -1, -1, 1, -1, 1, 1, -1, 1, 1]),
    _gl.STATIC_DRAW
  );

  const fs = _gl.createShader(_gl.FRAGMENT_SHADER)!;
  _gl.shaderSource(fs, FRAG_SRC);
  _gl.compileShader(fs);

  _prog = _gl.createProgram()!;
  _gl.attachShader(_prog, _vShader);
  _gl.attachShader(_prog, fs);
  _gl.linkProgram(_prog);
}

function renderToCanvas(
  targetCanvas: HTMLCanvasElement,
  uniforms: Uniforms,
  time: number
) {
  ensureGL();
  if (!_gl || !_glCanvas || !_prog || !_qBuf) return;

  const pxW = targetCanvas.width;
  const pxH = targetCanvas.height;
  if (_glCanvas.width !== pxW || _glCanvas.height !== pxH) {
    _glCanvas.width = pxW;
    _glCanvas.height = pxH;
  }

  _gl.viewport(0, 0, pxW, pxH);
  _gl.useProgram(_prog);
  _gl.bindBuffer(_gl.ARRAY_BUFFER, _qBuf);

  const pos = _gl.getAttribLocation(_prog, "a");
  _gl.enableVertexAttribArray(pos);
  _gl.vertexAttribPointer(pos, 2, _gl.FLOAT, false, 0, 0);

  _gl.uniform2f(_gl.getUniformLocation(_prog, "R"), pxW, pxH);
  _gl.uniform1f(_gl.getUniformLocation(_prog, "S"), uniforms.S);
  _gl.uniform1f(_gl.getUniformLocation(_prog, "H"), uniforms.H);
  _gl.uniform4fv(_gl.getUniformLocation(_prog, "P"), uniforms.P);
  _gl.uniform4fv(_gl.getUniformLocation(_prog, "Q"), uniforms.Q);
  _gl.uniform3fv(_gl.getUniformLocation(_prog, "C1"), uniforms.C1);
  _gl.uniform3fv(_gl.getUniformLocation(_prog, "C2"), uniforms.C2);
  _gl.uniform1f(_gl.getUniformLocation(_prog, "T"), time);

  _gl.drawArrays(_gl.TRIANGLES, 0, 6);

  const ctx2d = targetCanvas.getContext("2d");
  if (ctx2d) {
    ctx2d.clearRect(0, 0, pxW, pxH);
    ctx2d.drawImage(_glCanvas, 0, 0);
  }
}

// --- Component ---

interface FallbackAvatarProps {
  name: string;
  size?: number;
  animated?: boolean;
  className?: string;
}

export default function FallbackAvatar({
  name,
  size = 32,
  animated = true,
  className,
}: FallbackAvatarProps) {
  const canvasRef = useRef<HTMLCanvasElement>(null);
  const animRef = useRef<number>(0);
  const isHovering = useRef(false);
  const timeRef = useRef(0);
  const uniformsRef = useRef<Uniforms | null>(null);

  const [isMounted, setIsMounted] = useState(false);

  useEffect(() => {
    setIsMounted(true);
    return () => setIsMounted(false);
  }, []);

  const getUniforms = useCallback(() => {
    if (!uniformsRef.current) {
      uniformsRef.current = computeUniforms(name);
    }
    return uniformsRef.current;
  }, [name]);

  useEffect(() => {
    uniformsRef.current = null;
  }, [name]);

  // Initial static render
  useEffect(() => {
    if (!isMounted) return;
    const canvas = canvasRef.current;
    if (!canvas) return;
    renderToCanvas(canvas, getUniforms(), 0);
  }, [name, size, isMounted, getUniforms]);

  // Hover animation
  useEffect(() => {
    if (!animated || !isMounted) return;
    const canvas = canvasRef.current;
    if (!canvas) return;

    let startTime: number | null = null;

    const animate = (timestamp: number) => {
      if (!isHovering.current) return;
      if (startTime === null) startTime = timestamp - timeRef.current * 1000;
      const elapsed = (timestamp - startTime) / 1000;
      timeRef.current = elapsed;
      renderToCanvas(canvas, getUniforms(), elapsed);
      animRef.current = requestAnimationFrame(animate);
    };

    const onEnter = () => {
      isHovering.current = true;
      startTime = null;
      animRef.current = requestAnimationFrame(animate);
    };

    const onLeave = () => {
      isHovering.current = false;
      cancelAnimationFrame(animRef.current);
      renderToCanvas(canvas, getUniforms(), timeRef.current);
    };

    canvas.addEventListener("mouseenter", onEnter);
    canvas.addEventListener("mouseleave", onLeave);

    return () => {
      canvas.removeEventListener("mouseenter", onEnter);
      canvas.removeEventListener("mouseleave", onLeave);
      cancelAnimationFrame(animRef.current);
    };
  }, [animated, isMounted, getUniforms]);

  const dpr = isMounted ? window.devicePixelRatio || 1 : 1;

  return (
    <canvas
      ref={canvasRef}
      width={size * dpr}
      height={size * dpr}
      className={cn("rounded-full", className)}
      style={{ width: size, height: size }}
    />
  );
}

demo.tsx
"use client";

import FallbackAvatar from "@/components/ui/fallback-avatar";

const people = [
  "Maya Chen",
  "Noah Kim",
  "Ava Patel",
  "Leo Garcia",
  "Zoe Smith",
  "Eli Brown",
  "Nina Rossi",
  "Owen Lee",
];

function Demo() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center overflow-hidden bg-background p-8">
      <div className="rounded-3xl border bg-card/70 p-8 shadow-sm backdrop-blur">
        <div className="mb-6">
          <p className="text-sm text-muted-foreground">Generated team avatars</p>
          <h3 className="text-2xl font-semibold tracking-tight">Fallback Avatar</h3>
        </div>
        <div className="grid grid-cols-4 gap-5">
          {people.map((name) => (
            <div key={name} className="flex flex-col items-center gap-2">
              <FallbackAvatar className="border shadow-sm" name={name} size={72} />
              <span className="max-w-24 truncate text-xs text-muted-foreground">
                {name}
              </span>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
}


export default { Demo };
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
