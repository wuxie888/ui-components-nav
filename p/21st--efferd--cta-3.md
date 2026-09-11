<!-- CTA 3 · @efferd · https://21st.dev/@efferd/components/cta-3
     license: unspecified · category: cta
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
components/ui/cta.tsx
import { Button } from "@/components/ui/button";
import { DecorIcon } from "@/components/decor-icon";

export function CallToAction() {
	return (
		<div className="relative mx-auto flex w-full max-w-3xl flex-col justify-between gap-y-4 border-y px-4 py-8 dark:bg-[radial-gradient(35%_80%_at_25%_0%,--theme(--color-foreground/.08),transparent)]">
			<DecorIcon className="size-4" position="top-left" />
			<DecorIcon className="size-4" position="top-right" />
			<DecorIcon className="size-4" position="bottom-left" />
			<DecorIcon className="size-4" position="bottom-right" />

			<div className="pointer-events-none absolute -inset-y-6 -left-px w-px border-l" />
			<div className="pointer-events-none absolute -inset-y-6 -right-px w-px border-r" />

			<div className="absolute top-0 left-1/2 -z-10 h-full border-l border-dashed" />

			<h2 className="text-center font-semibold text-xl md:text-3xl">
				Start for Free Today!
			</h2>
			<p className="text-balance text-center font-medium text-muted-foreground text-sm md:text-base">
				Begin your 6-day free trial today to fully explore and experience all
				the features and benefits we offer.
			</p>

			<div className="flex items-center justify-center gap-2">
				<Button variant="outline">Contact Sales</Button>
				<Button>
					Get Started{" "}
					<IconPlaceholder
						data-icon="inline-end"
						hugeicons="ArrowRight02Icon"
						lucide="ArrowRightIcon"
						phosphor="ArrowRightIcon"
						remixicon="RiArrowRightLine"
						tabler="IconArrowRight"
					/>
				</Button>
			</div>
		</div>
	);
}

demo.tsx
import { CallToAction } from "@/components/ui/cta-3";

export default function DemoOne() {
return (
    <div className="w-full flex min-h-screen items-center justify-center p-4">
      <CallToAction />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button decor-icon
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
