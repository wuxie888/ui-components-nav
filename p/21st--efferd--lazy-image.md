<!-- Lazy Image · @efferd · https://21st.dev/@efferd/components/lazy-image
     license: unspecified · category: spinner
     A lightweight lazy-loading image with a skeleton shimmer and smooth fade-in.
Supports placeholders, in-view loading, and responsive aspect ratios out of the box. -->

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
components/lazy-image.tsx
"use client";

import { cn } from "@/lib/utils";
import { useInView } from "motion/react";
import React from "react";
import { AspectRatio } from "@/components/ui/aspect-ratio";

type LazyImageProps = {
	alt: string;
	src: string;
	className?: string;
	containerClassName?: string;
	/** URL of the fallback image. default: undefined */
	fallback?: string;
	/** The ratio of the image. */
	ratio: number;
	/** Whether the image should only load when it is in view. default: false */
	inView?: boolean;
};

export function LazyImage({
	alt,
	src,
	ratio,
	fallback,
	inView = false,
	className,
	containerClassName,
}: LazyImageProps) {
	const ref = React.useRef<HTMLDivElement | null>(null);
	const imgRef = React.useRef<HTMLImageElement | null>(null);
	const isInView = useInView(ref, { once: true });

	const [imgSrc, setImgSrc] = React.useState<string | undefined>(
		inView ? undefined : src
	);
	const [isLoading, setIsLoading] = React.useState(true);

	const handleError = () => {
		if (fallback) {
			setImgSrc(fallback);
		}
		setIsLoading(false);
	};

	const handleLoad = React.useCallback(() => {
		setIsLoading(false);
	}, []);

	// Load image only when inView
	React.useEffect(() => {
		if (inView && isInView && !imgSrc) {
			setImgSrc(src);
		}
	}, [inView, isInView, src, imgSrc]);

	// Handle cached images instantly
	React.useEffect(() => {
		if (imgRef.current?.complete) {
			handleLoad();
		}
	}, [handleLoad]);

	return (
		<AspectRatio
			className={cn(
				"relative size-full overflow-hidden border bg-accent/30",
				containerClassName
			)}
			ratio={ratio}
			ref={ref}
		>
			{imgSrc && (
				// biome-ignore lint/correctness/useImageSize: dynamic image size
				<img
					alt={alt}
					className={cn(
						"size-full object-cover transition-opacity duration-500",
						isLoading ? "opacity-0" : "opacity-100",
						className
					)}
					decoding="async"
					fetchPriority={inView ? "high" : "low"}
					loading="lazy"
					onError={handleError}
					onLoad={handleLoad}
					ref={imgRef}
					role="presentation" // Changed from "img" to "presentation" since it's decorative
					src={imgSrc}
				/>
			)}
		</AspectRatio>
	);
}

demo.tsx
'use client';
import React from 'react';
import { LazyImage } from '@/components/ui/lazy-image';
import { Button } from '@/components/ui/button';

export default function Demo() {
	const [instance, setInstance] = React.useState(0);
	return (
		<LazyImageDemo
			key={instance}
			onReload={() => setInstance((prev) => prev + 1)}
		/>
	);
}

function LazyImageDemo({ onReload }: { onReload: () => void }) {
	const [seed] = React.useState(() => Math.floor(Math.random() * 1000));

	const imageUrl = `https://picsum.photos/seed/${seed}/1280/720`;

	return (
		<div className="flex min-h-screen w-full flex-col items-center justify-center gap-4 p-4">
			<Button onClick={onReload}>Load Image</Button>
			<div className="mx-auto w-full max-w-4xl">
				<LazyImage
					alt="Random"
					src={imageUrl}
					ratio={16 / 9}
					fallback="https://cdn.21st.dev/assets/mirror/1b/1bf754ad4768408210bd1d41d50775b0bbc49c12618f08b13660e8df4b178091.svg"
				/>
			</div>
		</div>
	);
}
```

Install NPM dependencies:
```bash
npm install framer-motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add aspect-ratio button
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
