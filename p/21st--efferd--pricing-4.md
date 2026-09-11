<!-- Pricing Section with Frequency Toggle · @efferd · https://21st.dev/@efferd/components/pricing-4
     license: no-license · category: pricing-section
     Interactive three-tier pricing section with a monthly/yearly toggle, animated price numbers, and a highlighted popular plan. -->

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
components/ui/pricing-section.tsx
"use client";
import { cn } from "@/lib/utils";
import NumberFlow from "@number-flow/react";
import { AnimatePresence, motion } from "motion/react";
import Link from "next/link";
import React from "react";
import { Button } from "@/components/ui/button";
import { type FREQUENCY, FrequencyToggle } from "@/components/frequency-toggle";

type Plan = {
	name: string;
	info: string;
	price: {
		monthly: number;
		yearly: number; // yearly per month
	};
	features: string[];
	btn: {
		text: string;
		href: string;
	};
	highlighted?: boolean;
};

const plans: Plan[] = [
	{
		name: "Basic",
		info: "For most individuals",
		price: {
			monthly: 7,
			yearly: 6,
		},
		features: [
			"Up to 3 Blog posts",
			"Up to 3 Transcriptions",
			"Up to 3 Posts stored",
			"Markdown support",
			"Community support",
			"AI powered suggestions",
		],
		btn: {
			text: "Start Your Free Trial",
			href: "#",
		},
	},
	{
		highlighted: true,
		name: "Pro",
		info: "For small businesses",
		price: {
			monthly: 17,
			yearly: 14,
		},
		features: [
			"Up to 500 Blog Posts",
			"Up to 500 Transcriptions",
			"Up to 500 Posts stored",
			"Unlimited Markdown support",
			"SEO optimization tools",
			"Priority support",
			"AI powered suggestions",
		],
		btn: {
			text: "Get started",
			href: "#",
		},
	},
	{
		name: "Business",
		info: "For large organizations",
		price: {
			monthly: 49,
			yearly: 40,
		},
		features: [
			"Unlimited Blog Posts",
			"Unlimited Transcriptions",
			"Unlimited Posts stored",
			"Unlimited Markdown support",
			"SEO optimization tools",
			"Priority support",
			"AI powered suggestions",
		],
		btn: {
			text: "Contact team",
			href: "#",
		},
	},
];

export function PricingSection() {
	const [frequency, setFrequency] = React.useState<"monthly" | "yearly">(
		"monthly"
	);

	return (
		<div className="flex w-full flex-col items-center justify-center space-y-7 p-4">
			<div className="mx-auto max-w-xl space-y-2">
				<h2 className="text-center font-bold text-2xl tracking-tight md:text-3xl lg:font-extrabold lg:text-4xl">
					Plans that Scale with You
				</h2>
				<p className="text-center text-muted-foreground text-sm md:text-base">
					Whether you're just starting out or growing fast, our flexible pricing
					has you covered — with no hidden costs.
				</p>
			</div>

			<FrequencyToggle frequency={frequency} setFrequency={setFrequency} />
			<div className="mx-auto grid w-full max-w-4xl grid-cols-1 gap-6 md:grid-cols-3">
				{plans.map((plan) => (
					<PricingCard frequency={frequency} key={plan.name} plan={plan} />
				))}
			</div>
		</div>
	);
}

type PricingCardProps = React.ComponentProps<"div"> & {
	plan: Plan;
	frequency?: FREQUENCY;
};

export function PricingCard({
	plan,
	className,
	frequency = "monthly",
	...props
}: PricingCardProps) {
	return (
		<div
			className={cn(
				"relative flex w-full flex-col overflow-hidden rounded-lg border shadow-xs",
				plan.highlighted && "scale-105",
				className
			)}
			key={plan.name}
			{...props}
		>
			<div
				className={cn(
					"border-b p-4",
					plan.highlighted && "bg-card dark:bg-card/80"
				)}
			>
				<AnimatePresence mode="wait">
					<div className="absolute top-2 right-2 z-10 flex items-center gap-2">
						{plan.highlighted && (
							<motion.div
								className="flex items-center gap-1 rounded-md border bg-background px-2 py-0.5 text-xs"
								key="popular-badge"
								layout
								transition={{ duration: 0.1 }}
							>
								<IconPlaceholder
									className="size-3 fill-current"
									hugeicons="StarIcon"
									lucide="StarIcon"
									phosphor="StarIcon"
									remixicon="RiStarLine"
									tabler="IconStar"
								/>
								Popular
							</motion.div>
						)}

						{frequency === "yearly" &&
							plan.price.monthly > plan.price.yearly && (
								<motion.div
									animate={{ opacity: 1 }}
									className="flex items-center gap-1 rounded-md border bg-primary px-2 py-0.5 text-primary-foreground text-xs"
									exit={{ opacity: 0 }}
									initial={{ opacity: 0 }}
									key="discount-badge"
									layout
									transition={{ duration: 0.15 }}
								>
									{/* Calculate the actual discount percentage of the plan */}
									{Math.round(
										((plan.price.monthly - plan.price.yearly) /
											plan.price.monthly) *
											100
									)}
									% off
								</motion.div>
							)}
					</div>
				</AnimatePresence>

				<div className="font-medium text-lg">{plan.name}</div>
				<p className="font-normal text-muted-foreground text-sm">{plan.info}</p>
				<h3 className="mt-6 mb-1 flex w-max items-end gap-1">
					<NumberFlow
						className="font-extrabold text-3xl [&::part(suffix)]:font-normal [&::part(suffix)]:text-base [&::part(suffix)]:text-muted-foreground"
						format={{
							style: "currency",
							currency: "USD",
							notation: "compact",
						}}
						suffix="/month"
						value={plan.price[frequency]}
					/>
				</h3>
				<p className="mb-2 font-normal text-muted-foreground text-xs">
					billed {frequency}
				</p>
			</div>
			<div
				className={cn(
					"space-y-3 px-4 pt-6 pb-8 text-muted-foreground text-sm",
					plan.highlighted && "bg-muted/10"
				)}
			>
				{plan.features.map((feature) => (
					<div className="flex items-center gap-2" key={feature}>
						<IconPlaceholder
							className="size-3.5 text-foreground"
							hugeicons="CheckmarkCircle04Icon"
							lucide="CheckCircleIcon"
							phosphor="CheckCircleIcon"
							remixicon="RiCheckboxCircleLine"
							tabler="IconCircleCheck"
						/>
						<p>{feature}</p>
					</div>
				))}
			</div>
			<div
				className={cn(
					"mt-auto w-full border-t p-3",
					plan.highlighted && "bg-card dark:bg-card/80"
				)}
			>
				<Button
					asChild
					className="w-full"
					variant={plan.highlighted ? "default" : "outline"}
				>
					<Link href={plan.btn.href}>{plan.btn.text}</Link>
				</Button>
			</div>
		</div>
	);
}

components/ui/frequency-toggle.tsx
"use client";
import { cn } from "@/lib/utils";
import { motion } from "motion/react";
import type React from "react";

export type FREQUENCY = "monthly" | "yearly";

type FrequencyToggleProps = React.ComponentProps<"div"> & {
	frequency: FREQUENCY;
	setFrequency: React.Dispatch<React.SetStateAction<FREQUENCY>>;
	frequencies?: FREQUENCY[];
};

export function FrequencyToggle({
	frequency,
	setFrequency,
	frequencies = ["monthly", "yearly"],
	...props
}: FrequencyToggleProps) {
	return (
		<div
			className={cn(
				"mx-auto flex w-fit rounded-xl border bg-card p-1 shadow-xs",
				props.className
			)}
			{...props}
		>
			{frequencies.map((freq) => (
				<button
					className="relative px-4 py-1 text-sm capitalize"
					key={freq}
					onClick={() => setFrequency(freq)}
					type="button"
				>
					<span className="relative z-10">{freq}</span>
					{frequency === freq && (
						<motion.span
							className="absolute inset-0 z-10 rounded-xl bg-background mix-blend-difference dark:bg-foreground"
							layoutId="frequency"
							transition={{ type: "spring", duration: 0.4 }}
						/>
					)}
				</button>
			))}
		</div>
	);
}

demo.tsx
import PricingSection from "@/components/ui/pricing-4";

export default function PricingSectionDemo() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center bg-background py-16 text-foreground">
      <PricingSection />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @number-flow/react lucide-react motion
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
