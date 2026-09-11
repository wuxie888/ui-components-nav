<!-- 3d Marquee · Aceternity UI · https://ui.aceternity.com/components/3d-marquee
     license: MIT · category: marquee
     A 3D Marquee effect with grid, good for showcasing testimonials and hero sections -->

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
components/ui/3d-marquee.tsx
"use client";

import { motion } from "motion/react";
import { cn } from "@/lib/utils";
export const ThreeDMarquee = ({
  images,
  className,
}: {
  images: string[];
  className?: string;
}) => {
  // Split the images array into 4 equal parts
  const chunkSize = Math.ceil(images.length / 4);
  const chunks = Array.from({ length: 4 }, (_, colIndex) => {
    const start = colIndex * chunkSize;
    return images.slice(start, start + chunkSize);
  });
  return (
    <div
      className={cn(
        "mx-auto block h-[600px] overflow-hidden rounded-2xl max-sm:h-100",
        className,
      )}
    >
      <div className="flex size-full items-center justify-center">
        <div className="size-[1720px] shrink-0 scale-50 sm:scale-75 lg:scale-100">
          <div
            style={{
              transform: "rotateX(55deg) rotateY(0deg) rotateZ(-45deg)",
            }}
            className="relative top-96 right-[50%] grid size-full origin-top-left grid-cols-4 gap-8 transform-3d"
          >
            {chunks.map((subarray, colIndex) => (
              <motion.div
                animate={{ y: colIndex % 2 === 0 ? 100 : -100 }}
                transition={{
                  duration: colIndex % 2 === 0 ? 10 : 15,
                  repeat: Infinity,
                  repeatType: "reverse",
                }}
                key={colIndex + "marquee"}
                className="flex flex-col items-start gap-8"
              >
                <GridLineVertical className="-left-4" offset="80px" />
                {subarray.map((image, imageIndex) => (
                  <div className="relative" key={imageIndex + image}>
                    <GridLineHorizontal className="-top-4" offset="20px" />
                    <motion.img
                      whileHover={{
                        y: -10,
                      }}
                      transition={{
                        duration: 0.3,
                        ease: "easeInOut",
                      }}
                      key={imageIndex + image}
                      src={image}
                      alt={`Image ${imageIndex + 1}`}
                      className="aspect-[970/700] rounded-lg object-cover ring ring-gray-950/5 hover:shadow-2xl"
                      width={970}
                      height={700}
                    />
                  </div>
                ))}
              </motion.div>
            ))}
          </div>
        </div>
      </div>
    </div>
  );
};

const GridLineHorizontal = ({
  className,
  offset,
}: {
  className?: string;
  offset?: string;
}) => {
  return (
    <div
      style={
        {
          "--background": "#ffffff",
          "--color": "rgba(0, 0, 0, 0.2)",
          "--height": "1px",
          "--width": "5px",
          "--fade-stop": "90%",
          "--offset": offset || "200px", //-100px if you want to keep the line inside
          "--color-dark": "rgba(255, 255, 255, 0.2)",
          maskComposite: "exclude",
        } as React.CSSProperties
      }
      className={cn(
        "absolute left-[calc(var(--offset)/2*-1)] h-[var(--height)] w-[calc(100%+var(--offset))]",
        "bg-[linear-gradient(to_right,var(--color),var(--color)_50%,transparent_0,transparent)]",
        "[background-size:var(--width)_var(--height)]",
        "[mask:linear-gradient(to_left,var(--background)_var(--fade-stop),transparent),_linear-gradient(to_right,var(--background)_var(--fade-stop),transparent),_linear-gradient(black,black)]",
        "[mask-composite:exclude]",
        "z-30",
        "dark:bg-[linear-gradient(to_right,var(--color-dark),var(--color-dark)_50%,transparent_0,transparent)]",
        className,
      )}
    ></div>
  );
};

const GridLineVertical = ({
  className,
  offset,
}: {
  className?: string;
  offset?: string;
}) => {
  return (
    <div
      style={
        {
          "--background": "#ffffff",
          "--color": "rgba(0, 0, 0, 0.2)",
          "--height": "5px",
          "--width": "1px",
          "--fade-stop": "90%",
          "--offset": offset || "150px", //-100px if you want to keep the line inside
          "--color-dark": "rgba(255, 255, 255, 0.2)",
          maskComposite: "exclude",
        } as React.CSSProperties
      }
      className={cn(
        "absolute top-[calc(var(--offset)/2*-1)] h-[calc(100%+var(--offset))] w-[var(--width)]",
        "bg-[linear-gradient(to_bottom,var(--color),var(--color)_50%,transparent_0,transparent)]",
        "[background-size:var(--width)_var(--height)]",
        "[mask:linear-gradient(to_top,var(--background)_var(--fade-stop),transparent),_linear-gradient(to_bottom,var(--background)_var(--fade-stop),transparent),_linear-gradient(black,black)]",
        "[mask-composite:exclude]",
        "z-30",
        "dark:bg-[linear-gradient(to_bottom,var(--color-dark),var(--color-dark)_50%,transparent_0,transparent)]",
        className,
      )}
    ></div>
  );
};

demo.tsx
"use client";
import { ThreeDMarquee } from "@/components/ui/3d-marquee";

export default function ThreeDMarqueeDemo() {
  const images = [
    "https://cdn.21st.dev/assets/mirror/0c/0c1b9daa8958761bd680a7472a0fca30695372b04b3768afb6f925c80ad51ac2.png",
    "https://cdn.21st.dev/assets/mirror/cc/cc54cab20e500bfc666a3d006fcbea92ee57407b552d7a40702ffb13e375d145.png",
    "https://cdn.21st.dev/assets/mirror/92/92c8376dba8be3292150d2d9c71e88658ed8f1706c70edf559124e8c46a6a1ad.webp",
    "https://cdn.21st.dev/assets/mirror/cc/cc2dfc8dfedb1750610c30282ddf350fc19c0be5cfe9425bb15fd2829e0851fc.png",
    "https://cdn.21st.dev/assets/mirror/13/138b5c6c771c7ee2e669ed5c41fc10f258fb2e62ebebaa9257ca2fa44c207f71.png",
    "https://cdn.21st.dev/assets/mirror/8e/8e2c324b6f6f74f376ab36778066ddddd3c6c8c516536506989c3b48673193ee.png",
    "https://cdn.21st.dev/assets/mirror/33/33c3b297f01fefa675d7a209206836824e9c27f803dcb6627831e8c37f1ba28d.png",
    "https://cdn.21st.dev/assets/mirror/96/9659640f579d0b0cb0ce5fb0ab075bb670f7eec1308e6dcf21af4c68b5363e7b.png",
    "https://cdn.21st.dev/assets/mirror/27/27954035d8ec50e6708cef496cdf1673ac724e534fcaba367582b3bb2d5a09d5.png",
    "https://cdn.21st.dev/assets/mirror/c8/c811c67928c08fcf3c1cb56a0c3ee24121983e7ce4ec03d857f2155afd319ec9.webp",
    "https://cdn.21st.dev/assets/mirror/4f/4f392821d1bac633efcd1bd5ad82afa7f28b72871fc02a3ce9e968f72d2643d2.png",
    "https://cdn.21st.dev/assets/mirror/fb/fbedb15cda77cdccdda1f3593ce4f7d1b44ff6ee7092a0f37416a16471894a18.png",
    "https://cdn.21st.dev/assets/mirror/0f/0f57e305728baa3e2690572d1ec87857985891e33cf17d8da562b7c0fa2b61aa.png",
    "https://cdn.21st.dev/assets/mirror/80/80b00b6a3290eddc107450ff35cc8e5f202122cb4c140d4f93b4df86138b846c.png",
    "https://cdn.21st.dev/assets/mirror/94/94b4a94744fbed20bdf0d5a391fcaeb7712659b881ad7f25ed5e171c6b0bedef.webp",
    "https://cdn.21st.dev/assets/mirror/4d/4d991b9d5c68e83913c516dcba1ef2fab1e7d6afc28da8db328cd6df880f5829.png",
    "https://cdn.21st.dev/assets/mirror/ea/ea462a1a1c0345dbc39a45ef6ec9136759e22e09d8a47ce6258d0129485fc544.png",
    "https://cdn.21st.dev/assets/mirror/45/458e84cb32abb12e1925f65fe4a7fd1ba691145faa7c348961dc539a824f142d.png",
    "https://cdn.21st.dev/assets/mirror/80/804ba514e4b77e1ddf56595bac13a5e233746eb736a113c084b0ce4213a34572.png",
    "https://cdn.21st.dev/assets/mirror/84/84111d091fd2cea221562ea64c052739702183067894c707c9cc100264cdd68b.png",
    "https://cdn.21st.dev/assets/mirror/e8/e8848272d3b319d168f6a98ba059b9da7a9db84a416a099ba2540a94dd4da83d.webp",
    "https://cdn.21st.dev/assets/mirror/f2/f2ff6364c66b4ffa58bd614f9fe014170fb508510449794e852b4d53fc3c9ff4.png",
    "https://cdn.21st.dev/assets/mirror/ef/ef25579d8e901076fd49227b766c6a57b0597e59cd7a24a81f7dccdd893a7b26.png",
    "https://cdn.21st.dev/assets/mirror/22/221c9fa186791f84a658e91e86763dd62409f020e4708dc089b25db43228f84a.png",
    "https://cdn.21st.dev/assets/mirror/14/14c3f7a38fbf518a920f48f17602641a9b7cd80d6e0946da024a2745a5148f52.png",
    "https://cdn.21st.dev/assets/mirror/44/44c22d45c939c0e3f7f7ee6e9cdbe2a94ed15a4231ebee810e7fec73387ff0a6.png",
    "https://cdn.21st.dev/assets/mirror/7d/7d436ba5e940394d4ddf9a13e27d779b0786d0adee7dfc0ab3bc5287bb198f19.png",
    "https://cdn.21st.dev/assets/mirror/32/324044404c1e35fdb445316531598f0f27b93cb3045a09ca8efaa977c3e29c04.png",
    "https://cdn.21st.dev/assets/mirror/bd/bdc8d01e5c55a54ad5069f88318453e4941e03ca484ddf2f1a41ea1e4d1db74b.png",
    "https://cdn.21st.dev/assets/mirror/7f/7f264ef88d660aed1cbefa5b7c8022daf638ac420022ce7d1783851f6d18f93d.png",
    "https://cdn.21st.dev/assets/mirror/fe/fe7b30b12e4781380531c4058fc894e989e6614b14d096cd2fa3b6c98d02a75e.webp",
  ];
  return (
    <div className="mx-auto my-10 max-w-7xl rounded-3xl bg-gray-950/5 p-2 ring-1 ring-neutral-700/10 dark:bg-neutral-800">
      <ThreeDMarquee images={images} />
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
