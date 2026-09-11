<!-- 3D Parallax Unfurling Gallery · @piyushxdev · https://21st.dev/@piyushxdev/components/3d-parallax-unfurling-gallery
     license: MIT · category: gallery
     A cinematic, scroll-driven image grid gallery with deep 3D matrix rotations, smooth multi-column parallax translation offsets, and responsive fluid boundaries for landing showcases -->

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
components/ui/3d-parallax-unfurling-gallery.tsx
"use client";

import React, { useCallback, useEffect, useMemo, useRef, useState } from "react";
import { motion, useScroll, useSpring, useTransform } from "framer-motion";

// Reconstructed from the public demo bundle of
// https://21st.dev/@piyushxdev/components/3d-parallax-unfurling-gallery (MIT, by Piyush)

const IMAGES = [
  "https://cdn.21st.dev/assets/mirror/a9/a9c2900d44fe6288b344f447cb12a05f7e64c439479a8ccb977d3b20eb371156.jpg",
  "https://cdn.21st.dev/assets/mirror/29/29cf6ad39eb198c05b8d915fca0becfd3d270d510d32eaec1b886c426c681c67.jpg",
  "https://cdn.21st.dev/assets/mirror/61/6154958e9df110914005256ff2319d43a2c2e0fc8bb54e9f8bce7b91fdce5df1.jpg",
  "https://cdn.21st.dev/assets/mirror/6d/6db92aff3c02cce69e2c672a6dd4e99cbf5c55d68fbf08c460527e6c7c5b64ba.jpg",
  "https://cdn.21st.dev/assets/mirror/42/42ad2d0680dba697d578434e5af5620c7ab1c7c55bc36cec3b55eec8b7a79cbf.jpg",
  "https://cdn.21st.dev/assets/mirror/cd/cd3dc09b1bbed97cfc879e2c5e62fdbc68dc4070b6105e476410d70e31d1e459.jpg",
  "https://cdn.21st.dev/assets/mirror/02/0232d63e3e0cb8d3599a77e29f87f8ec4b9fadfd031592296b3f19a730a5348c.jpg",
  "https://cdn.21st.dev/assets/mirror/56/562b212caa6ec06d8b0b313660dac6aa0bbfb729092cc4f16d04558a319af6b1.jpg",
  "https://cdn.21st.dev/assets/mirror/02/02cbcd62720734d469f2ea8e5ed7a212e18cb05e73457445b4d755ad0ae1fcd8.jpg",
  "https://images.unsplash.com/photo-1550614000-4b95d4ed798a?auto=format&fit=crop&w=600&q=80",
  "https://cdn.21st.dev/assets/mirror/c4/c42df7c9c444a1189dad0570c0d01986454cd6a10eaf253a9ab40eb921a5bae5.jpg",
  "https://cdn.21st.dev/assets/mirror/27/275fbf3f84c5258c7a8235a8a47022f847d0f408c950288c532aefa83d072a2c.jpg",
  "https://cdn.21st.dev/assets/mirror/7e/7e2fb073870b2f578a37a693b1e0c9402a98201149509b54da2f86a2ee6abf5e.jpg",
  "https://cdn.21st.dev/assets/mirror/3d/3d74651780292fb5a2ba23e525d9d09860bb83fbfafc7ede17b8e3662d7b1022.jpg",
];

const ImageCard = ({ src, onLoad }: { src: string; onLoad: () => void }) => (
  <div className="w-full h-[200px] sm:h-[300px] md:h-[400px] flex-shrink-0 bg-[#111] transition-transform duration-300 hover:scale-[1.02] cursor-pointer relative will-change-transform backface-hidden preserve-3d">
    <img
      src={src}
      alt="Gallery Asset"
      loading="lazy"
      onLoad={onLoad}
      className="w-full h-full object-cover opacity-80 hover:opacity-100 transition-opacity duration-300"
    />
  </div>
);

export default function Component() {
  const containerRef = useRef<HTMLDivElement>(null);
  const sectionRef = useRef<HTMLElement>(null);
  const [, setReady] = useState(false);
  const loadedCount = useRef(0);

  const handleLoad = useCallback(() => {
    loadedCount.current += 1;
    if (loadedCount.current >= 1) setReady(true);
  }, []);

  useEffect(() => {
    const t = setTimeout(() => setReady(true), 1200);
    return () => clearTimeout(t);
  }, []);

  const columns = useMemo(() => {
    const c1 = IMAGES.filter((_, i) => i % 4 === 0);
    const c2 = IMAGES.filter((_, i) => i % 4 === 1);
    const c3 = IMAGES.filter((_, i) => i % 4 === 2);
    const c4 = IMAGES.filter((_, i) => i % 4 === 3);
    return {
      col1: [...c1, ...c1],
      col2: [...c2, ...c2],
      col3: [...c3, ...c3],
      col4: [...c4, ...c4],
    };
  }, []);

  const { scrollYProgress } = useScroll({
    target: sectionRef,
    container: containerRef,
    offset: ["start start", "end end"],
  });
  const progress = useSpring(scrollYProgress, { stiffness: 100, damping: 20, mass: 0.5 });

  // Phase 1 (0 → 15%): the framed card unfurls to full viewport
  const width = useTransform(progress, [0, 0.15], ["90vw", "100vw"]);
  const height = useTransform(progress, [0, 0.15], ["80vh", "100vh"]);
  const borderRadius = useTransform(progress, [0, 0.15], ["48px", "0px"]);
  const borderWidth = useTransform(progress, [0, 0.15], ["4px", "0px"]);

  // Phase 2 (15% → 100%): the tilted grid settles toward flat while columns parallax
  const rotateY = useTransform(progress, [0.15, 1], [-45, -8]);
  const rotateX = useTransform(progress, [0.15, 1], [25, 4]);
  const rotateZ = useTransform(progress, [0.15, 1], [15, 2]);
  const z = useTransform(progress, [0.15, 1], [-800, 0]);
  const y1 = useTransform(progress, [0.15, 1], ["0%", "-40%"]);
  const y2 = useTransform(progress, [0.15, 1], ["-40%", "10%"]);
  const y3 = useTransform(progress, [0.15, 1], ["0%", "-40%"]);
  const y4 = useTransform(progress, [0.15, 1], ["-30%", "20%"]);

  const colClass = "flex flex-col gap-4 md:gap-6 w-[22vw] min-w-[200px] pointer-events-auto";

  return (
    <div ref={containerRef} className="w-full h-screen overflow-y-auto overflow-x-hidden bg-[#050505]">
      <section
        ref={sectionRef}
        className="relative w-full h-[600vh] bg-[#050505] text-white font-sans selection:bg-white selection:text-black"
      >
        <div className="sticky top-0 h-screen w-full flex justify-center items-center overflow-hidden">
          <motion.div
            style={{ width, height, borderRadius, borderWidth, borderColor: "#2c2738" }}
            className="relative bg-black overflow-hidden flex items-center justify-center max-w-[1920px] mx-auto will-change-transform backface-hidden preserve-3d"
          >
            <div
              className="absolute inset-0 flex justify-center items-center pointer-events-none"
              style={{ perspective: "1000px" }}
            >
              <div className="absolute inset-0 z-20 shadow-[inset_0_100px_150px_-50px_rgba(0,0,0,1),inset_0_-100px_150px_-50px_rgba(0,0,0,1)]" />
              <div className="absolute inset-0 z-20 shadow-[inset_150px_0_150px_-50px_rgba(0,0,0,1),inset_-150px_0_150px_-50px_rgba(0,0,0,1)]" />
              <motion.div
                style={{ rotateX, rotateY, rotateZ, z, transformStyle: "preserve-3d" }}
                className="flex gap-4 md:gap-6 justify-center items-center w-[120vw] h-[150vh] origin-center opacity-100 will-change-transform backface-hidden"
              >
                <motion.div style={{ y: y1 }} className={colClass}>
                  {columns.col1.map((src, i) => <ImageCard key={`col1-${i}`} src={src} onLoad={handleLoad} />)}
                </motion.div>
                <motion.div style={{ y: y2 }} className={colClass}>
                  {columns.col2.map((src, i) => <ImageCard key={`col2-${i}`} src={src} onLoad={handleLoad} />)}
                </motion.div>
                <motion.div style={{ y: y3 }} className={colClass}>
                  {columns.col3.map((src, i) => <ImageCard key={`col3-${i}`} src={src} onLoad={handleLoad} />)}
                </motion.div>
                <motion.div style={{ y: y4 }} className={colClass}>
                  {columns.col4.map((src, i) => <ImageCard key={`col4-${i}`} src={src} onLoad={handleLoad} />)}
                </motion.div>
              </motion.div>
            </div>
          </motion.div>
        </div>
      </section>
    </div>
  );
}

demo.tsx
// This is a file with a demo for your component
// That's what users will see in the preview
// Create new files in this directory to add more demos

import Component  from "@/components/ui/3d-parallax-unfurling-gallery";

// ONLY DEFAULT EXPORT WILL BE TREATED AS A DEMO
export default function DemoOne() {
  return <Component />;
}
```

Install NPM dependencies:
```bash
npm install framer-motion
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
