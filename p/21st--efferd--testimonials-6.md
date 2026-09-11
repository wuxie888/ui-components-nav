<!-- Infinite Testimonials Scroller · @efferd · https://21st.dev/@efferd/components/testimonials-6
     license: no-license · category: testimonials
     Testimonials section with three columns of quote cards that scroll infinitely in a vertical marquee, slowing on hover. -->

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
import { InfiniteSlider } from "@/components/ui/infinite-slider";
import {
	Avatar,
	AvatarFallback,
	AvatarImage,
} from "@/components/ui/avatar";

type Testimonial = {
	quote: string;
	image: string;
	name: string;
	role: string;
	company?: string;
};

const testimonials: Testimonial[] = [
	{
		quote:
			"Efferd is so polished I might just retire and become a full-time potato farmer. The ecosystem is in safe hands.",
		image: "https://github.com/shadcn.png",
		name: "Shadcn",
		role: "Founder",
		company: "Shadcn UI",
	},
	{
		quote:
			"Efferd is why I still have hair. No more pulling it out over centering divs or fighting with CSS grid.",
		image: "https://github.com/rauchg.png",
		name: "Guillermo Rauch",
		role: "CEO",
		company: "Vercel",
	},

	{
		quote:
			"I tried to buy Efferd but they wouldn't sell. So I just bought Twitter instead to complain about it.",
		image: "https://unavatar.io/x/elonmusk",
		name: "Elon Musk",
		role: "CEO",
		company: "X.com",
	},
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
	{
		quote:
			"We processed 100 petabytes of data to find the perfect UI library. The algorithm returned 'Efferd' with 99.9% confidence.",
		image: "https://unavatar.io/x/sundarpichai",
		name: "Sundar Pichai",
		role: "CEO",
		company: "Google",
	},
	{
		quote:
			"Our links might 404 sometimes, but thanks to Efferd, at least the 404 page looks absolutely stunning.",
		image: "https://github.com/steven-tey.png",
		name: "Steven Tey",
		role: "Founder",
		company: "Dub.co",
	},
	{
		quote:
			"It's so fast, I finished my UI sprint before my next meeting even started. Open source for the win.",
		image: "https://unavatar.io/x/peer_rich",
		name: "Peer Richelsen",
		role: "Co-Founder",
		company: "Cal.com",
	},
];

const firstColumn = testimonials.slice(0, 3);
const secondColumn = testimonials.slice(3, 6);
const thirdColumn = testimonials.slice(6, 9);

export function TestimonialsSection() {
	return (
		<section className="relative py-10">
			<div className="mx-auto max-w-5xl">
				<div className="mx-auto flex max-w-sm flex-col items-center justify-center gap-4">
					<div className="flex justify-center">
						<div className="rounded-lg border px-4 py-1">Testimonials</div>
					</div>

					<h2 className="font-bold text-3xl tracking-tighter lg:text-4xl">
						What our users say
					</h2>
					<p className="text-center text-muted-foreground text-sm">
						See what our customers have to say about us.
					</p>
				</div>

				<div
					className={cn(
						"mt-10 flex max-h-160 justify-center gap-6 overflow-hidden",
						"mask-[linear-gradient(to_bottom,transparent,black_25%,black_75%,transparent)]"
					)}
				>
					<InfiniteSlider direction="vertical" speed={30} speedOnHover={15}>
						{firstColumn.map((testimonial) => (
							<TestimonialsCard
								key={testimonial.name}
								testimonial={testimonial}
							/>
						))}
					</InfiniteSlider>
					<InfiniteSlider
						className="hidden md:block"
						direction="vertical"
						speed={50}
						speedOnHover={25}
					>
						{secondColumn.map((testimonial) => (
							<TestimonialsCard
								key={testimonial.name}
								testimonial={testimonial}
							/>
						))}
					</InfiniteSlider>
					<InfiniteSlider
						className="hidden lg:block"
						direction="vertical"
						speed={35}
						speedOnHover={17}
					>
						{thirdColumn.map((testimonial) => (
							<TestimonialsCard
								key={testimonial.name}
								testimonial={testimonial}
							/>
						))}
					</InfiniteSlider>
				</div>
			</div>
		</section>
	);
}

function TestimonialsCard({
	testimonial,
	className,
	...props
}: React.ComponentProps<"figure"> & {
	testimonial: Testimonial;
}) {
	const { quote, image, name, role, company } = testimonial;
	return (
		<figure
			className={cn(
				"w-full max-w-xs rounded-3xl border bg-card p-8 shadow-foreground/10 shadow-lg dark:bg-card/20",
				className
			)}
			{...props}
		>
			<blockquote>{quote}</blockquote>
			<figcaption className="mt-5 flex items-center gap-2">
				<Avatar className="size-8 rounded-full">
					<AvatarImage alt={`${name}'s profile picture`} src={image} />
					<AvatarFallback>{name.charAt(0)}</AvatarFallback>
				</Avatar>
				<div className="flex flex-col">
					<cite className="font-medium not-italic leading-5 tracking-tight">
						{name}
					</cite>
					<span className="text-muted-foreground text-sm leading-5 tracking-tight">
						{role} {company && `, ${company}`}
					</span>
				</div>
			</figcaption>
		</figure>
	);
}

demo.tsx
import { TestimonialsSection } from "@/components/ui/testimonials-6";

export default function TestimonialsDemo() {
  return <TestimonialsSection />;
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar infinite-slider infinite-slider?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068
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
