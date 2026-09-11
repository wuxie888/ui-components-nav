<!-- Footer 2 · @efferd · https://21st.dev/@efferd/components/footer-2
     license: unspecified · category: footer
     a modern website footer featuring multi-column navigation, social media icons, and App Store & Google Play buttons. Perfect for SaaS, eCommerce, or blogs, it adds credibility, easy navigation, and mobile app links in one elegant layout. -->

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
components/ui/footer.tsx
import { cn } from "@/lib/utils";
import { GithubIcon } from "@/components/icons/github-icon";
import { InstagramIcon } from "@/components/icons/instagram-icon";
import { XIcon } from "@/components/icons/x-icon";
import { Logo } from "@/components/logo";
import { Button } from "@/components/ui/button";
import { FullWidthDivider } from "@/components/full-width-divider";

export function Footer() {
	return (
		<footer
			className={cn(
				"relative mx-auto max-w-5xl lg:border-x",
				"dark:bg-[radial-gradient(35%_80%_at_15%_0%,--theme(--color-foreground/.1),transparent)]"
			)}
		>
			<FullWidthDivider position="top" />
			<div className="grid max-w-5xl grid-cols-6 gap-6 p-4">
				<div className="col-span-6 flex flex-col gap-4 pt-5 md:col-span-4">
					<a className="w-max" href="#">
						<Logo className="h-5" />
					</a>
					<p className="max-w-sm text-balance text-muted-foreground text-sm">
						Beautify your app with efferd.
					</p>
					<div className="flex gap-2">
						{socialLinks.map((item, index) => (
							<Button
								asChild
								key={`social-${item.link}-${index}`}
								size="icon"
								variant="outline"
							>
								<a href={item.link} target="_blank">
									{item.icon}
								</a>
							</Button>
						))}
					</div>
				</div>
				<div className="col-span-3 w-full md:col-span-1">
					<span className="text-muted-foreground text-xs">Resources</span>
					<div className="mt-2 flex flex-col gap-2">
						{resources.map(({ href, title }) => (
							<a
								className="w-max text-sm hover:underline"
								href={href}
								key={title}
							>
								{title}
							</a>
						))}
					</div>
				</div>
				<div className="col-span-3 w-full md:col-span-1">
					<span className="text-muted-foreground text-xs">Company</span>
					<div className="mt-2 flex flex-col gap-2">
						{company.map(({ href, title }) => (
							<a
								className="w-max text-sm hover:underline"
								href={href}
								key={title}
							>
								{title}
							</a>
						))}
					</div>
				</div>
			</div>
			<FullWidthDivider />
			<div className="flex items-center justify-center gap-2 py-4">
				<p className="text-center font-light text-muted-foreground text-sm">
					&copy; {new Date().getFullYear()} efferd, All rights reserved
				</p>
			</div>
		</footer>
	);
}

const company = [
	{
		title: "About Us",
		href: "#",
	},
	{
		title: "Careers",
		href: "#",
	},
	{
		title: "Brand assets",
		href: "#",
	},
	{
		title: "Privacy Policy",
		href: "#",
	},
	{
		title: "Terms of Service",
		href: "#",
	},
];

const resources = [
	{
		title: "Blog",
		href: "#",
	},
	{
		title: "Help Center",
		href: "#",
	},
	{
		title: "Contact Support",
		href: "#",
	},
	{
		title: "Community",
		href: "#",
	},
	{
		title: "Security",
		href: "#",
	},
];

const socialLinks = [
	{
		icon: <GithubIcon />,
		link: "#",
	},
	{
		icon: <InstagramIcon />,
		link: "#",
	},
	{
		icon: <XIcon />,
		link: "#",
	},
];

demo.tsx
import { Footer2 } from "@/components/ui/footer-2";

export default function DemoOne() {
   return (
    <div className="w-full">
      <div className="w-full relative flex min-h-[calc(100vh-20rem)] h-full items-center">
        {/* Subtle dotted grid */}
        <div
          aria-hidden="true"
          className="pointer-events-none absolute inset-0"
          style={{
            backgroundImage:
              "radial-gradient(rgba(255,255,255,0.08) 0.8px, transparent 0.8px)",
            backgroundSize: "14px 14px",
            maskImage:
              "radial-gradient( circle at 50% 10%, rgba(0,0,0,1), rgba(0,0,0,0.2) 40%, rgba(0,0,0,0) 70% )",
          }}
        />
      </div>

      <Footer2 />
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
npx shadcn@latest add app-store-button button github-icon instagram-icon logo play-store-button x-icon
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
