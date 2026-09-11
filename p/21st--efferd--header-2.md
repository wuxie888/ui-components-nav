<!-- Header 2 · @efferd · https://21st.dev/@efferd/components/header-2
     license: unspecified · category: navigation-menu
     Elegant responsive header with scroll-based styling, mobile drawer menu, and smooth transitions. -->

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
components/ui/header.tsx
"use client";
import { cn } from "@/lib/utils";
import { Logo } from "@/components/logo";
import { useScroll } from "@/hooks/use-scroll";
import { Button } from "@/components/ui/button";
import { MobileNav } from "@/components/1/mobile-nav";

export const navLinks = [
	{
		label: "Features",
		href: "#",
	},
	{
		label: "Pricing",
		href: "#",
	},
	{
		label: "About",
		href: "#",
	},
];

export function Header() {
	const scrolled = useScroll(10);

	return (
		<header
			className={cn(
				"sticky top-0 z-50 mx-auto w-full max-w-4xl border-transparent border-b md:rounded-md md:border md:transition-all md:ease-out",
				{
					"border-border bg-background/95 backdrop-blur-sm supports-backdrop-filter:bg-background/50 md:top-2 md:max-w-3xl md:shadow":
						scrolled,
				}
			)}
		>
			<nav
				className={cn(
					"flex h-14 w-full items-center justify-between px-4 md:h-12 md:transition-all md:ease-out",
					{
						"md:px-2": scrolled,
					}
				)}
			>
				<a
					className="rounded-md p-2 hover:bg-muted dark:hover:bg-muted/50"
					href="#"
				>
					<Logo className="h-4" />
				</a>
				<div className="hidden items-center gap-2 md:flex">
					<div>
						{navLinks.map((link) => (
							<Button asChild key={link.label} size="sm" variant="ghost">
								<a href={link.href}>{link.label}</a>
							</Button>
						))}
					</div>
					<Button size="sm" variant="outline">
						Sign In
					</Button>
					<Button size="sm">Get Started</Button>
				</div>
				<MobileNav />
			</nav>
		</header>
	);
}

components/ui/mobile-nav.tsx
import { cn } from "@/lib/utils";
import React from "react";
import { Button } from "@/components/ui/button";
import { Portal, PortalBackdrop } from "@/components/portal";
import { navLinks } from "@/components/header";

export function MobileNav() {
	const [open, setOpen] = React.useState(false);

	return (
		<div className="md:hidden">
			<Button
				aria-controls="mobile-menu"
				aria-expanded={open}
				aria-label="Toggle menu"
				className="md:hidden"
				onClick={() => setOpen(!open)}
				size="icon"
				variant="outline"
			>
				{open ? (
					<IconPlaceholder
						className="size-4.5"
						hugeicons="Cancel01Icon"
						lucide="XIcon"
						phosphor="XIcon"
						remixicon="RiCloseLine"
						tabler="IconX"
					/>
				) : (
					<IconPlaceholder
						className="size-4.5"
						hugeicons="Menu01Icon"
						lucide="MenuIcon"
						phosphor="ListIcon"
						remixicon="RiMenuLine"
						tabler="IconMenu2"
					/>
				)}
			</Button>
			{open && (
				<Portal className="top-14" id="mobile-menu">
					<PortalBackdrop />
					<div
						className={cn(
							"data-[slot=open]:zoom-in-97 ease-out data-[slot=open]:animate-in",
							"size-full p-4"
						)}
						data-slot={open ? "open" : "closed"}
					>
						<div className="grid gap-y-2">
							{navLinks.map((link) => (
								<Button
									asChild
									className="justify-start"
									key={link.label}
									variant="ghost"
								>
									<a href={link.href}>{link.label}</a>
								</Button>
							))}
						</div>
						<div className="mt-12 flex flex-col gap-2">
							<Button className="w-full" variant="outline">
								Sign In
							</Button>
							<Button className="w-full">Get Started</Button>
						</div>
					</div>
				</Portal>
			)}
		</div>
	);
}

demo.tsx
import React from 'react';
import { Header } from "@/components/ui/header-2";

export default function Demo() {
	return (
		<div className="w-full">
			<Header />

			<main className="mx-auto min-h-screen w-full max-w-3xl px-4 py-12">
				<div  className="space-y-2 mb-4">
					<div className="bg-accent h-6 w-4/6 rounded-md border" />
					<div className="bg-accent h-6 w-1/2 rounded-md border" />
				</div>
				<div  className="flex gap-2 mb-8">
					<div className="bg-accent h-3 w-14 rounded-md border" />
					<div className="bg-accent h-3 w-12 rounded-md border" />
				</div>

				{Array.from({ length: 7 }).map((_, i) => (
					<div key={i} className="space-y-2 mb-8">
						<div className="bg-accent h-4 w-full rounded-md border" />
						<div className="bg-accent h-4 w-full rounded-md border" />
						<div className="bg-accent h-4 w-full rounded-md border" />
						<div className="bg-accent h-4 w-1/2 rounded-md border" />
					</div>
				))}
			</main>
		</div>
	);
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button logo menu-toggle-icon portal use-scroll
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
