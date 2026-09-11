<!-- Logo Cloud 2 · @efferd · https://21st.dev/@efferd/components/logo-cloud-2
     license: unspecified · category: clients
     Stylish logo grid featuring top tech brands with decorative plus icons and dynamic borders for a modern visual layout. -->

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
import { cn } from "@/lib/utils";
import { DecorIcon } from "@/components/decor-icon";

type Logo = {
	src: string;
	alt: string;
};

export function LogoCloud() {
	return (
		<div className="grid grid-cols-2 border md:grid-cols-4">
			<LogoCard
				className="relative border-r border-b bg-secondary dark:bg-secondary/30"
				logo={{
					src: "https://storage.efferd.com/logo/nvidia-wordmark.svg",
					alt: "Nvidia Logo",
				}}
			>
				<DecorIcon className="z-10" position="bottom-right" />
			</LogoCard>

			<LogoCard
				className="border-b md:border-r"
				logo={{
					src: "https://storage.efferd.com/logo/supabase-wordmark.svg",
					alt: "Supabase Logo",
				}}
			/>

			<LogoCard
				className="relative border-r border-b md:bg-secondary dark:md:bg-secondary/30"
				logo={{
					src: "https://storage.efferd.com/logo/github-wordmark.svg",
					alt: "GitHub Logo",
				}}
			>
				<DecorIcon className="z-10" position="bottom-right" />
				<DecorIcon className="z-10 hidden md:block" position="bottom-left" />
			</LogoCard>

			<LogoCard
				className="relative border-b bg-secondary md:bg-background dark:bg-secondary/30 md:dark:bg-background"
				logo={{
					src: "https://storage.efferd.com/logo/openai-wordmark.svg",
					alt: "OpenAI Logo",
				}}
			/>

			<LogoCard
				className="relative border-r border-b bg-secondary md:border-b-0 md:bg-background dark:bg-secondary/30 md:dark:bg-background"
				logo={{
					src: "https://storage.efferd.com/logo/turso-wordmark.svg",
					alt: "Turso Logo",
				}}
			>
				<DecorIcon className="z-10 md:hidden" position="bottom-right" />
			</LogoCard>

			<LogoCard
				className="border-b bg-background md:border-r md:border-b-0 md:bg-secondary dark:md:bg-secondary/30"
				logo={{
					src: "https://storage.efferd.com/logo/clerk-wordmark.svg",
					alt: "Clerk Logo",
				}}
			/>

			<LogoCard
				className="border-r"
				logo={{
					src: "https://storage.efferd.com/logo/claude-wordmark.svg",
					alt: "Claude AI Logo",
				}}
			/>

			<LogoCard
				className="bg-secondary dark:bg-secondary/30"
				logo={{
					src: "https://storage.efferd.com/logo/vercel-wordmark.svg",
					alt: "Vercel Logo",
				}}
			/>
		</div>
	);
}

type LogoCardProps = React.ComponentProps<"div"> & {
	logo: Logo;
};

function LogoCard({ logo, className, children, ...props }: LogoCardProps) {
	return (
		<div
			className={cn(
				"flex items-center justify-center bg-background px-4 py-8 md:p-8",
				className
			)}
			{...props}
		>
			<img
				alt={logo.alt}
				className="pointer-events-none h-4 select-none md:h-5 dark:brightness-0 dark:invert"
				height="auto"
				src={logo.src}
				width="auto"
			/>
			{children}
		</div>
	);
}

demo.tsx
import { LogoCloud } from "@/components/ui/logo-cloud-2";

export default function DemoOne() {
 return (
    <div className="min-h-screen w-full place-content-center px-4">
      <section className="relative mx-auto grid max-w-3xl">
        <h2 className="mb-6 text-center font-medium text-lg text-muted-foreground tracking-tight md:text-2xl">
          Companies we{" "}
          <span className="font-semibold text-primary">collaborate</span> with.
        </h2>

        <LogoCloud />
      </section>
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
npx shadcn@latest add decor-icon
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
