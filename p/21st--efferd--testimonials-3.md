<!-- Staggered Testimonials Grid · @efferd · https://21st.dev/@efferd/components/testimonials-3
     license: no-license · category: testimonials
     Staggered grid of testimonial cards with quote icons, avatars, and decorative corner marks. -->

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
import { DecorIcon } from "@/components/decor-icon";

type Testimonial = {
	quote: string;
	name: string;
	role: string;
	company: string;
	image: string;
};

const testimonials: Testimonial[] = [
	{
		quote:
			"We just acquired Efferd for 3 gazillion dollars. We're calling it iEfferd. It's our best product yet.",
		image: "https://unavatar.io/x/tim_cook",
		name: "Tim Cook",
		role: "CEO",
		company: "Apple",
	},
	{
		quote:
			"I'm considering shipping Efferd components with Prime delivery. 2-day shipping on beautiful UIs? Done.",
		image: "https://unavatar.io/x/JeffBezos",
		name: "Jeff Bezos",
		role: "Founder",
		company: "Amazon",
	},
	{
		quote:
			"We're rewriting OpenAI's entire frontend in Efferd. The AGI told us it's the only logical choice.",
		image: "https://unavatar.io/x/sama",
		name: "Sam Altman",
		role: "CEO",
		company: "OpenAI",
	},
];

export function TestimonialsSection() {
	return (
		<div className="mx-auto -mt-10 grid w-full max-w-5xl gap-8 md:grid-cols-3 md:gap-6">
			{testimonials.map((testimonial, index) => (
				<TestimonialCard
					index={index}
					key={testimonial.name}
					testimonial={testimonial}
				/>
			))}
		</div>
	);
}

function TestimonialCard({
	testimonial,
	index,
	className,
	...props
}: React.ComponentProps<"figure"> & {
	testimonial: Testimonial;
	index: number;
}) {
	const { quote, name, role, company, image } = testimonial;

	return (
		<figure
			className={cn(
				"relative flex flex-col justify-between gap-6 px-8 pt-8 pb-6 shadow-xs md:translate-y-[calc(3rem*var(--t-card-index))]",
				"dark:bg-[radial-gradient(50%_80%_at_25%_0%,--theme(--color-foreground/.1),transparent)]",
				className
			)}
			style={
				{
					"--t-card-index": index,
				} as React.CSSProperties
			}
			{...props}
		>
			<div className="absolute -inset-y-4 -left-px w-px bg-border" />
			<div className="absolute -inset-y-4 -right-px w-px bg-border" />
			<div className="absolute -inset-x-4 -top-px h-px bg-border" />
			<div className="absolute -right-4 -bottom-px -left-4 h-px bg-border" />
			<DecorIcon className="size-3.5" position="top-left" />

			<blockquote className="flex gap-4">
				<IconPlaceholder
					aria-hidden="true"
					className="size-6 shrink-0 stroke-1"
					hugeicons="QuoteDownIcon"
					lucide="QuoteIcon"
					phosphor="QuotesIcon"
					remixicon="RiDoubleQuotesL"
					tabler="IconQuote"
				/>

				<p className="flex-1 font-normal text-base text-muted-foreground leading-relaxed">
					{quote}
				</p>
			</blockquote>

			<figcaption className="flex items-center gap-3">
				<Avatar className="size-10 rounded-full ring-2 ring-border ring-offset-2 ring-offset-background transition-shadow group-hover:ring-foreground/20">
					<AvatarImage alt={`${name}'s profile picture`} src={image} />
					<AvatarFallback>{name.charAt(0)}</AvatarFallback>
				</Avatar>
				<div className="flex flex-col">
					<cite className="font-medium text-foreground text-sm not-italic">
						{name}
					</cite>
					<p className="text-muted-foreground text-xs">
						{role}, <span className="text-foreground/80">{company}</span>
					</p>
				</div>
			</figcaption>
		</figure>
	);
}

demo.tsx
import TestimonialsSection from "@/components/ui/testimonials-3";

export default function Default() {
  return (
    <section className="flex min-h-screen w-full items-center justify-center bg-background px-6 py-24 text-foreground">
      <div className="w-full">
        <div className="mx-auto mb-20 max-w-xl text-center">
          <p className="font-medium text-muted-foreground text-sm uppercase tracking-widest">
            Testimonials
          </p>
          <h2 className="mt-3 text-balance font-semibold text-3xl tracking-tight sm:text-4xl">
            Loved by builders everywhere
          </h2>
          <p className="mt-4 text-muted-foreground">
            Don't take our word for it — here's what industry leaders have to
            say.
          </p>
        </div>
        <TestimonialsSection />
      </div>
    </section>
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
