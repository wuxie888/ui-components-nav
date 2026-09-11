<!-- Integrations Logo Row · @efferd · https://21st.dev/@efferd/components/integrations-5
     license: no-license · category: cta
     A horizontal row of overlapping circular integration logos with edge fade masks, a heading, and a call-to-action button. -->

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
components/ui/integrations.tsx
import { cn } from "@/lib/utils";
import { Button } from "@/components/ui/button";

type Integration = {
	src?: string;
	name: string;
	isInvertable?: boolean;
};

const data: Integration[] = [
	{
		name: "Empty 1",
	},
	{
		name: "Vercel",
		src: "https://storage.efferd.com/logo/vercel.svg",
		isInvertable: true,
	},
	{
		name: "OpenAI",
		src: "https://storage.efferd.com/logo/openai.svg",
		isInvertable: true,
	},
	{
		src: "https://storage.efferd.com/logo/supabase.svg",
		name: "Supabase",
	},
	{
		name: "GitHub",
		src: "https://storage.efferd.com/logo/github.svg",
		isInvertable: true,
	},
	{
		name: "Notion",
		src: "https://storage.efferd.com/logo/notion.svg",
	},
	{
		name: "Gmail",
		src: "https://storage.efferd.com/logo/gmail.svg",
	},
	{
		name: "Google Maps",
		src: "https://storage.efferd.com/logo/google-maps.svg",
	},
	{
		name: "Neon",
		src: "https://storage.efferd.com/logo/neon.svg",
	},
	{
		name: "Empty 2",
	},
];

export function Integrations() {
	return (
		<div className="flex flex-col items-center justify-center gap-6 py-24">
			<div className="max-w-xl space-y-2 px-4 text-center">
				<h2 className="font-semibold text-4xl tracking-tight">
					All types of integration
				</h2>
				<p className="text-base text-muted-foreground md:text-lg">
					Connect your favourite apps and services easily
				</p>
			</div>

			<div className="flex flex-col justify-center rounded-full border bg-secondary dark:bg-secondary/10">
				<div className="mask-l-from-90 mask-r-from-90 flex items-center justify-center -space-x-4 p-1">
					{data.map((item) => (
						<div
							className={cn(
								"relative z-0 transition-transform",
								item.src ? "hover:z-10 hover:scale-110" : ""
							)}
							key={item.name}
						>
							<div className="flex size-12 items-center justify-center overflow-hidden rounded-full border bg-card shadow-sm md:size-16">
								{item.src && (
									<img
										alt={item.name}
										className={cn(
											"pointer-events-auto size-5 select-none object-contain md:size-6",
											item.isInvertable && "dark:invert"
										)}
										height="auto"
										src={item.src}
										width="auto"
									/>
								)}
							</div>
						</div>
					))}
				</div>
			</div>
			<Button className="rounded-full px-5!">
				See all integration{" "}
				<IconPlaceholder
					data-icon="inline-end"
					hugeicons="ArrowUpRight01Icon"
					lucide="ArrowUpRightIcon"
					phosphor="ArrowUpRightIcon"
					remixicon="RiArrowRightUpLine"
					tabler="IconArrowUpRight"
				/>
			</Button>
		</div>
	);
}

demo.tsx
import { Integrations } from "@/components/ui/integrations-5";

export default function Default() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center bg-background text-foreground">
      <Integrations />
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
npx shadcn@latest add button
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
