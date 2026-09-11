<!-- Hero 1 · @efferd · https://21st.dev/@efferd/components/hero-1
     license: unspecified · category: hero
     Centered hero with radial gradient background and announcement badge -->

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
components/ui/hero.tsx
import { cn } from "@/lib/utils";
import { Button } from "@/components/ui/button";

export function HeroSection() {
	return (
		<section className="mx-auto w-full max-w-5xl">
			{/* Top Shades */}
			<div
				aria-hidden="true"
				className="absolute inset-0 isolate hidden overflow-hidden contain-strict lg:block"
			>
				<div className="absolute inset-0 -top-14 isolate -z-10 bg-[radial-gradient(35%_80%_at_49%_0%,--theme(--color-foreground/.08),transparent)] contain-strict" />
			</div>

			{/* X Bold Faded Borders */}
			<div
				aria-hidden="true"
				className="absolute inset-0 mx-auto hidden min-h-screen w-full max-w-5xl lg:block"
			>
				<div className="mask-y-from-80% mask-y-to-100% absolute inset-y-0 left-0 z-10 h-full w-px bg-foreground/15" />
				<div className="mask-y-from-80% mask-y-to-100% absolute inset-y-0 right-0 z-10 h-full w-px bg-foreground/15" />
			</div>

			{/* main content */}

			<div className="relative flex flex-col items-center justify-center gap-5 pt-32 pb-30">
				{/* X Content Faded Borders */}
				<div
					aria-hidden="true"
					className="absolute inset-0 -z-1 size-full overflow-hidden"
				>
					<div className="absolute inset-y-0 left-4 w-px bg-linear-to-b from-transparent via-border to-border md:left-8" />
					<div className="absolute inset-y-0 right-4 w-px bg-linear-to-b from-transparent via-border to-border md:right-8" />
					<div className="absolute inset-y-0 left-8 w-px bg-linear-to-b from-transparent via-border/50 to-border/50 md:left-12" />
					<div className="absolute inset-y-0 right-8 w-px bg-linear-to-b from-transparent via-border/50 to-border/50 md:right-12" />
				</div>

				<a
					className={cn(
						"group mx-auto flex w-fit items-center gap-3 rounded-full border bg-card px-3 py-1 shadow",
						"fade-in slide-in-from-bottom-10 animate-in fill-mode-backwards transition-all delay-500 duration-500 ease-out"
					)}
					href="#link"
				>
					<IconPlaceholder
						className="size-3 text-muted-foreground"
						hugeicons="Rocket01Icon"
						lucide="RocketIcon"
						phosphor="RocketIcon"
						remixicon="RiRocketLine"
						tabler="IconRocket"
					/>
					<span className="text-xs">shipped new features!</span>
					<span className="block h-5 border-l" />

					<IconPlaceholder
						className="size-3 duration-150 ease-out group-hover:translate-x-1"
						hugeicons="ArrowRight02Icon"
						lucide="ArrowRightIcon"
						phosphor="ArrowRightIcon"
						remixicon="RiArrowRightLine"
						tabler="IconArrowRight"
					/>
				</a>

				<h1
					className={cn(
						"fade-in slide-in-from-bottom-10 animate-in text-balance fill-mode-backwards text-center text-4xl tracking-tight delay-100 duration-500 ease-out md:text-5xl lg:text-6xl",
						"text-shadow-[0_0px_50px_theme(--color-foreground/.2)]"
					)}
				>
					Building Teams Help <br /> You Scale and Lead
				</h1>

				<p className="fade-in slide-in-from-bottom-10 mx-auto max-w-md animate-in fill-mode-backwards text-center text-base text-foreground/80 tracking-wider delay-200 duration-500 ease-out sm:text-lg md:text-xl">
					Conecting you with world-class talent <br /> to scale, innovate and
					lead
				</p>

				<div className="fade-in slide-in-from-bottom-10 flex animate-in flex-row flex-wrap items-center justify-center gap-3 fill-mode-backwards pt-2 delay-300 duration-500 ease-out">
					<Button className="rounded-full" size="lg" variant="secondary">
						<IconPlaceholder
							data-icon="inline-start"
							hugeicons="Call02Icon"
							lucide="PhoneCallIcon"
							phosphor="PhoneCallIcon"
							remixicon="RiPhoneLine"
							tabler="IconPhone"
						/>{" "}
						Book a Call
					</Button>
					<Button className="rounded-full" size="lg">
						Get started{" "}
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
		</section>
	);
}

components/ui/logos-section.tsx
import { LogoCloud } from "@/components/logo-cloud"; // @efferd/logo-cloud-3

export function LogosSection() {
	return (
		<section className="relative space-y-4 border-t pt-6 pb-10">
			<h2 className="text-center font-medium text-lg text-muted-foreground tracking-tight md:text-xl">
				Trusted by <span className="text-foreground">experts</span>
			</h2>
			<div className="relative z-10 mx-auto max-w-4xl">
				<LogoCloud />
			</div>
		</section>
	);
}

demo.tsx
import { HeroSection, LogosSection } from "@/components/ui/hero-1";
import { Header } from "@/components/ui/header-1";

export default function DemoOne() {
	return (
		<div className= "flex w-full flex-col" >
		<Header />
		< main className = "grow" >
			<HeroSection />
			< LogosSection />
			</main>
			< /div>
		);
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button header-1 logo-cloud-3
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
