<!-- Image Gallery · @efferd · https://21st.dev/@efferd/components/image-gallery
     license: unspecified · category: gallery
     A responsive image gallery with lazy loading and smooth in-view fade-in effects.
Automatically handles portrait/landscape ratios and fallback placeholders. -->

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
components/ui/index.tsx
import { LazyImage } from "@/components/lazy-image";

export function ImageGallery() {
	return (
		<div className="relative flex min-h-screen w-full flex-col items-center justify-center px-4 py-10">
			<div className="mx-auto grid w-full max-w-5xl grid-cols-1 gap-4 sm:grid-cols-2 md:grid-cols-4 md:gap-6">
				{Array.from({ length: 4 }).map((_, col) => (
					<div className="grid gap-4" key={col}>
						{Array.from({ length: 8 }).map((_, index) => {
							const isPortrait = Math.random() > 0.5;
							const width = isPortrait ? 1080 : 1920;
							const height = isPortrait ? 1920 : 1080;
							const ratio = isPortrait ? 9 / 16 : 16 / 9;

							return (
								<LazyImage
									alt={`Image ${col}-${index}`}
									containerClassName="cn-rounded"
									fallback={`https://placehold.co/${width}x${height}/`}
									inView={true}
									key={`${col}-${index}`}
									ratio={ratio}
									src={`https://picsum.photos/seed/${col}-${index}/${width}/${height}`}
								/>
							);
						})}
					</div>
				))}
			</div>
		</div>
	);
}

demo.tsx
import { ImageGallery } from "@/components/ui/image-gallery";

export default function DemoOne() {
  return <ImageGallery />;
}
```

Install NPM dependencies:
```bash
npm install framer-motion motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add aspect-ratio lazy-image
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
