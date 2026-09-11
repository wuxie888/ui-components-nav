<!-- Pricing · @efferd · https://21st.dev/@efferd/components/pricing
     license: unspecified · category: cta
     a dynamic, responsive pricing table with animated monthly/yearly toggle, tooltips for feature descriptions, and an optional highlight effect for popular plans. Each card displays plan info, price, and a CTA button, making it perfect for showcasing product tiers in a modern SaaS landing page. -->

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
import { Badge } from "@/components/ui/badge";
import { Button } from "@/components/ui/button";
import { DecorIcon } from "@/components/decor-icon";

export function PricingSection() {
	return (
		<section className="w-full space-y-5">
			<div className="mx-auto max-w-lg">
				<div className="flex justify-center">
					<div className="rounded-md border px-4 py-1 text-sm">Pricing</div>
				</div>
				<h2 className="mt-4 text-center font-bold text-2xl tracking-tight md:text-3xl">
					Pricing Based on Your Success
				</h2>
				<p className="mt-2 text-center text-muted-foreground text-sm md:text-base">
					We offer a single price for all our services. We believe that pricing
					is a critical component of any successful business.
				</p>
			</div>

			<div className="mx-auto w-full max-w-2xl space-y-2">
				<div className="relative grid border bg-background p-4 shadow-xs md:grid-cols-2">
					<DecorIcon className="size-3" position="top-left" />
					<DecorIcon className="size-3" position="top-right" />
					<DecorIcon className="size-3" position="bottom-left" />
					<DecorIcon className="size-3" position="bottom-right" />

					<div className="w-full px-4 pt-5 pb-4">
						<div className="space-y-1">
							<div className="flex items-center justify-between">
								<h3 className="font-semibold leading-none">Monthly</h3>
								<div className="flex items-center gap-x-1">
									<span className="text-muted-foreground text-sm line-through">
										$8.99
									</span>
									<Badge variant="secondary">11% off</Badge>
								</div>
							</div>
							<p className="text-muted-foreground text-sm">
								Best value for growing businesses!
							</p>
						</div>
						<div className="mt-10 space-y-4">
							<div className="flex items-end gap-0.5 text-muted-foreground text-xl">
								<span>$</span>
								<span className="-mb-0.5 font-extrabold text-4xl text-foreground tracking-tighter md:text-4xl">
									7.99
								</span>
								<span>/month</span>
							</div>
							<Button asChild className="w-full" variant="outline">
								<a href="#">Start Your Journey</a>
							</Button>
						</div>
					</div>
					<div className="relative w-full rounded-md border bg-card p-4 shadow dark:bg-card/80">
						<div className="space-y-1">
							<div className="flex items-center justify-between">
								<h3 className="font-semibold leading-none">Yearly</h3>
								<div className="flex items-center gap-x-1">
									<span className="text-muted-foreground text-sm line-through">
										$8.99
									</span>
									<Badge>22% off</Badge>
								</div>
							</div>
							<p className="text-muted-foreground text-sm">
								Unlock savings with an annual commitment!
							</p>
						</div>
						<div className="mt-10 space-y-4">
							<div className="flex items-end text-muted-foreground text-xl">
								<span>$</span>
								<span className="-mb-0.5 font-extrabold text-4xl text-foreground tracking-tighter md:text-4xl">
									6.99
								</span>
								<span>/month</span>
							</div>
							<Button asChild className="w-full">
								<a href="#">Get Started Now</a>
							</Button>
						</div>
					</div>
				</div>

				<div className="flex items-center justify-center gap-x-2 text-muted-foreground text-sm">
					<IconPlaceholder
						className="size-4"
						hugeicons="SecurityCheckIcon"
						lucide="ShieldCheckIcon"
						phosphor="ShieldCheckIcon"
						remixicon="RiShieldCheckLine"
						tabler="IconShieldCheck"
					/>
					<span>Access to all features with no hidden fees</span>
				</div>
			</div>
		</section>
	);
}

demo.tsx
import React from 'react';
import { PricingSection } from '@/components/ui/pricing';

export default function Demo() {
	return (
		<div className="flex min-h-screen items-center justify-center py-12">
			<PricingSection
				plans={PLANS}
				heading="Plans that Scale with You"
				description="Whether you're just starting out or growing fast, our flexible pricing has you covered — with no hidden costs."
			/>
		</div>
	);
}

const PLANS = [
	{
		id: 'basic',
		name: 'Basic',
		info: 'For most individuals',
		price: {
			monthly: 7,
			yearly: Math.round(7 * 12 * (1 - 0.12)),
		},
		features: [
			{ text: 'Up to 3 Blog posts', limit: '100 tags' },
			{ text: 'Up to 3 Transcriptions' },
			{ text: 'Up to 3 Posts stored' },
			{
				text: 'Markdown support',
				tooltip: 'Export content in Markdown format',
			},
			{
				text: 'Community support',
				tooltip: 'Get answers your questions on discord',
			},
			{
				text: 'AI powered suggestions',
				tooltip: 'Get up to 100 AI powered suggestions',
			},
		],
		btn: {
			text: 'Start Your Free Trial',
			href: '#',
		},
	},
	{
		highlighted: true,
		id: 'pro',
		name: 'Pro',
		info: 'For small businesses',
		price: {
			monthly: 17.99,
			yearly: Math.round(17.99 * 12 * (1 - 0.12)),
		},
		features: [
			{ text: 'Up to 500 Blog Posts', limit: '500 tags' },
			{ text: 'Up to 500 Transcriptions' },
			{ text: 'Up to 500 Posts stored' },
			{
				text: 'Unlimited Markdown support',
				tooltip: 'Export content in Markdown format',
			},
			{ text: 'SEO optimization tools' },
			{ text: 'Priority support', tooltip: 'Get 24/7 chat support' },
			{
				text: 'AI powered suggestions',
				tooltip: 'Get up to 500 AI powered suggestions',
			},
		],
		btn: {
			text: 'Get started',
			href: '#',
		},
	},
	{
		name: 'Business',
		info: 'For large organizations',
		price: {
			monthly: 69.99,
			yearly: Math.round(49.99 * 12 * (1 - 0.12)),
		},
		features: [
			{ text: 'Unlimited Blog Posts' },
			{ text: 'Unlimited Transcriptions' },
			{ text: 'Unlimited Posts stored' },
			{ text: 'Unlimited Markdown support' },
			{
				text: 'SEO optimization tools',
				tooltip: 'Advanced SEO optimization tools',
			},
			{ text: 'Priority support', tooltip: 'Get 24/7 chat support' },
			{
				text: 'AI powered suggestions',
				tooltip: 'Get up to 500 AI powered suggestions',
			},
		],
		btn: {
			text: 'Contact team',
			href: '#',
		},
	},
];
```

Install NPM dependencies:
```bash
npm install framer-motion lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge button decor-icon tooltip
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
