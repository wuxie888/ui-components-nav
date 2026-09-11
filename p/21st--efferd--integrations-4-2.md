<!-- Integrations Grid · @efferd · https://21st.dev/@efferd/components/integrations-4-2
     license: no-license · category: features
     A split section with a heading and a masked grid of scattered rounded cards displaying integration logos. -->

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

type LogoType = {
	src: string;
	alt: string;
	isInvertable?: boolean;
};

type TileData = {
	row: number;
	col: number;
	logo?: LogoType;
};

export function Integrations() {
	return (
		<div className="mx-auto grid max-w-5xl grid-cols-1 gap-12 p-4 md:grid-cols-2 md:items-center">
			{/* Left Content */}
			<div className="max-w-xl space-y-5">
				<h2 className="font-medium text-3xl text-foreground tracking-tight sm:text-4xl md:text-5xl">
					Seamless Integration
				</h2>
				<p className="text-lg text-muted-foreground leading-8">
					Integrate with over 100+ tools and platforms to streamline your
					workflow and boost productivity.
				</p>
			</div>

			{/* Right Content - Visual */}
			<div className="place-items-end">
				<div className="mask-[radial-gradient(ellipse_at_center,black,black,transparent)] relative size-90">
					{tiles.map((tile) => (
						<IntegrationCard key={`${tile.row}_${tile.col}`} {...tile} />
					))}
				</div>
			</div>
		</div>
	);
}

function IntegrationCard({ row, col, logo }: TileData) {
	return (
		<div
			className={cn(
				"absolute flex size-18 items-center justify-center rounded-md border",
				logo
					? "bg-card shadow-xs dark:bg-card/60"
					: "bg-secondary/30 dark:bg-background" // Styling for empty tiles
			)}
			style={{
				left: col * 72, // 72px cell
				top: row * 72,
			}}
		>
			{logo && (
				<img
					alt={logo.alt}
					className={cn(
						"pointer-events-none size-8 select-none object-contain p-1",
						logo.isInvertable && "dark:invert"
					)}
					height={40}
					src={logo.src}
					width={40}
				/>
			)}
		</div>
	);
}

// Coordinate mapping to approximate the "scattered" look in the image.
// Grid 5x5.
const tiles: TileData[] = [
	// Row 0
	{
		row: 0,
		col: 1,
	},
	{
		row: 0,
		col: 3,
		logo: {
			src: "https://storage.efferd.com/logo/notion.svg",
			alt: "Notion Logo",
		},
	},

	// Row 1
	{ row: 1, col: 0 }, // Empty
	{
		row: 1,
		col: 2,
		logo: {
			src: "https://storage.efferd.com/logo/cursor.svg",
			alt: "Cursor Logo",
			isInvertable: true,
		},
	},
	{
		row: 1,
		col: 4,
		logo: {
			src: "https://storage.efferd.com/logo/vercel.svg",
			alt: "Vercel Logo",
			isInvertable: true,
		},
	},

	// Row 2
	{
		row: 2,
		col: 1,
		logo: {
			src: "https://storage.efferd.com/logo/planetscale.svg",
			alt: "Planetscale Logo",
			isInvertable: true,
		},
	},
	{
		row: 2,
		col: 3,
		logo: {
			src: "https://storage.efferd.com/logo/gmail.svg",
			alt: "Gmail Logo",
		},
	}, // Empty

	// Row 3

	{ row: 3, col: 0 }, // Empty
	{
		row: 3,
		col: 2,
		logo: {
			src: "https://storage.efferd.com/logo/supabase.svg",
			alt: "Supabase Logo",
		},
	},
	{
		row: 3,
		col: 4,
		logo: {
			src: "https://storage.efferd.com/logo/canva.svg",
			alt: "Canva Logo",
		},
	},

	// Row 4
	{
		row: 4,
		col: 1,
		logo: {
			src: "https://storage.efferd.com/logo/adobe.svg",
			alt: "Adobe Logo",
		},
	},
	{
		row: 4,
		col: 3,
		logo: {
			src: "https://storage.efferd.com/logo/polar.svg",
			alt: "Polar Logo",
		},
	},
];

demo.tsx
import Integrations from "@/components/ui/integrations-4";

export default function IntegrationsDemo() {
	return (
		<div className="flex min-h-svh w-full items-center justify-center p-6">
			<Integrations />
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
