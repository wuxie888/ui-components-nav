<!-- Testimonials Section · @efferd · https://21st.dev/@efferd/components/testimonials-2
     license: no-license · category: testimonials
     Minimalist testimonial block with a quote, author info, and decorative cross-hair mask lines framing the avatar. -->

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
components/ui/testimonials-section.tsx
import { cn } from "@/lib/utils";
import {
	Avatar,
	AvatarFallback,
	AvatarImage,
} from "@/components/ui/avatar";

export function TestimonialsSection() {
	return (
		<figure className="mx-auto flex w-full max-w-lg flex-col items-center justify-center md:grid md:grid-cols-[auto_1fr]">
			<div className="relative">
				{/* Vertical lines */}
				<MaskLine className="left-0" orientation="vertical" />
				<MaskLine className="right-0" orientation="vertical" />
				{/*  Horizontal lines */}
				<MaskLine className="top-0 md:w-xl" orientation="horizontal" />
				<MaskLine className="bottom-0 md:w-xl" orientation="horizontal" />

				<Avatar className="mask-[radial-gradient(circle,black_60%,transparent)] size-24 rounded-none *:rounded-none md:size-32">
					<AvatarImage
						alt="Shadcn's profile picture"
						src="https://github.com/shadcn.png"
					/>
					<AvatarFallback>SH</AvatarFallback>
				</Avatar>
			</div>
			<figcaption className="space-y-4 p-8 text-center md:p-6 md:text-left">
				<blockquote className="text-lg text-muted-foreground leading-tight tracking-tight">
					&quot;<span className="font-medium text-foreground">Efferd</span> is
					so polished I might just retire. The ecosystem is in safe hands.&quot;
				</blockquote>

				<div>
					<cite className="font-medium text-foreground text-xs not-italic">
						Shadcn
					</cite>
					<div className="text-[10px] text-muted-foreground">
						Founder, shadcn/ui
					</div>
				</div>
			</figcaption>
		</figure>
	);
}

export function MaskLine({
	className,
	orientation,
	...props
}: React.ComponentProps<"div"> & { orientation?: "horizontal" | "vertical" }) {
	return (
		<div
			aria-hidden="true"
			className={cn(
				"absolute bg-foreground/20",
				orientation === "vertical" &&
					"mask-t-from-80% mask-b-from-80% -inset-y-1/2 w-px",
				orientation === "horizontal" &&
					"mask-l-from-80% mask-r-from-80% -inset-x-1/2 h-px",
				className
			)}
			{...props}
		/>
	);
}

demo.tsx
import { TestimonialsSection } from "@/components/ui/testimonials-2";

export default function Default() {
	return (
		<div className="flex min-h-svh w-full items-center justify-center p-10">
			<TestimonialsSection />
		</div>
	);
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar
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
