<!-- Logo Cloud 4 · @efferd · https://21st.dev/@efferd/components/logo-cloud-4
     license: unspecified · category: marquee
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
components/ui/logo-cloud.tsx
import { InfiniteSlider } from "@/components/ui/infinite-slider";
// https://motion-primitives.com/docs/progressive-blur
import { ProgressiveBlur } from "@/components/ui/progressive-blur";

export function LogoCloud() {
	return (
		<div className="relative border-x border-y bg-linear-to-r from-secondary via-transparent to-secondary py-6">
			<InfiniteSlider gap={42} reverse speed={60} speedOnHover={20}>
				{logos.map((logo) => (
					<img
						alt={logo.alt}
						className="pointer-events-none h-4 select-none md:h-5 dark:brightness-0 dark:invert"
						height="auto"
						key={`logo-${logo.alt}`}
						loading="lazy"
						src={logo.src}
						width="auto"
					/>
				))}
			</InfiniteSlider>

			<ProgressiveBlur
				blurIntensity={1}
				className="pointer-events-none absolute top-0 left-0 h-full w-[100px] md:w-[160px]"
				direction="left"
			/>
			<ProgressiveBlur
				blurIntensity={1}
				className="pointer-events-none absolute top-0 right-0 h-full w-[100px] md:w-[160px]"
				direction="right"
			/>
		</div>
	);
}

const logos = [
	{
		src: "https://storage.efferd.com/logo/nvidia-wordmark.svg",
		alt: "Nvidia Logo",
	},
	{
		src: "https://storage.efferd.com/logo/supabase-wordmark.svg",
		alt: "Supabase Logo",
	},
	{
		src: "https://storage.efferd.com/logo/openai-wordmark.svg",
		alt: "OpenAI Logo",
	},
	{
		src: "https://storage.efferd.com/logo/turso-wordmark.svg",
		alt: "Turso Logo",
	},
	{
		src: "https://storage.efferd.com/logo/vercel-wordmark.svg",
		alt: "Vercel Logo",
	},
	{
		src: "https://storage.efferd.com/logo/github-wordmark.svg",
		alt: "GitHub Logo",
	},
	{
		src: "https://storage.efferd.com/logo/claude-wordmark.svg",
		alt: "Claude AI Logo",
	},
	{
		src: "https://storage.efferd.com/logo/clerk-wordmark.svg",
		alt: "Clerk Logo",
	},
];

demo.tsx
import { cn } from "@/lib/utils";
import { LogoCloud } from "@/components/ui/logo-cloud-4";

export default function DemoOne() {
  return (
    <div className="min-h-screen w-full place-content-center px-4">
     <div
        aria-hidden="true"
        className={cn(
          "-top-1/2 -translate-x-1/2 pointer-events-none absolute left-1/2 h-[120vmin] w-[120vmin] rounded-b-full",
          "bg-[radial-gradient(ellipse_at_center,--theme(--color-foreground/.1),transparent_50%)]",
          "blur-[30px]"
        )}
      />
      <div className="w-full">
        <h2 className="mb-5 text-center">
          <span className="block font-medium text-2xl text-muted-foreground">
            Already used by
          </span>
          <span className="font-black text-2xl text-primary tracking-tight md:text-3xl">
            Best in the Game
          </span>
        </h2>

        <LogoCloud logos={logos} />
      </div>
    </div>
  );
}

const logos = [
  {
    src: "https://cdn.21st.dev/assets/mirror/bd/bdf5f3ae72bcfda892a686c03b7932985c694e9a9828643c980601bbc9e53cb4.svg",
    alt: "Nvidia Logo",
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/31/319eeae853dd1af99d442b6c16b6c38dc52a66a719f8e502c65f85d26255cbd3.svg",
    alt: "Supabase Logo",
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/2b/2bcdd4124223e3bf8e66bc08ce0ac32a6cc42ffe3584bbecfd377847176a188d.svg",
    alt: "OpenAI Logo",
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/fc/fc7b090ebcfc468d24a1dc482b2db1fcbfd99ca14568552a30ce553d6dda7fcb.svg",
    alt: "Turso Logo",
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/56/5624b7c243ac8d60e848fb5ea222ec932c1600df54a2762238b37498372fb0c8.svg",
    alt: "Vercel Logo",
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/90/90f01a9537335666282ae5acc80bd4305f86d085a92d60904c3aa3ccc4414570.svg",
    alt: "GitHub Logo",
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/e8/e8514b1206f79e1abdafcc1d2632393cc7cfbcbbe25426ac5143b17b184b56b8.svg",
    alt: "Claude AI Logo",
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/96/96517bce3574d648280ff639d01d9889f354b488b3f826db5df746d730232a0c.svg",
    alt: "Clerk Logo",
  },
];
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add infinite-slider progressive-blur
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
