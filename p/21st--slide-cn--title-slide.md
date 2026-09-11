<!-- Title Slide · @slide-cn · https://21st.dev/@slide-cn/components/title-slide
     license: MIT · category: hero
     A centered slide layout with heading, subheading, and meta text for building presentation title screens. -->

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
components/ui/slide-cn/title-slide.tsx
import { cn } from "@/lib/utils"

export function TitleSlide({ children, className, ...props }: React.ComponentProps<"div">) {
	return (
		<div className={cn(
			"flex h-full w-full flex-col items-center justify-center text-center gap-4",
			className
		)}
			{...props}
		>
			{children}
		</div>
	)
}

TitleSlide.Heading = function Heading({ children, className, ...props }: React.ComponentProps<"h1">) {
	return (
		<h1
			className={cn(
				"text-6xl font-bold text-foreground tracking-tight",
				className
			)}
			{...props}
		>
			{children}
		</h1>
	)
}

TitleSlide.SubHeading = function Subheading({ children, className, ...props }: React.ComponentProps<"p">) {
	return (
		<p
			className={cn(
				"text-2xl text-muted-foreground",
				className
			)}
			{...props}
		>
			{children}
		</p>

	)
}

TitleSlide.Meta = function Meta({ children, className, ...props }: React.ComponentProps<"p">) {
	return (
		<p
			className={cn(
				"text-sm text-muted-foreground",
				className
			)}
			{...props}
		>
			{children}
		</p>
	);
}

demo.tsx
import { TitleSlide } from "@/components/ui/title-slide";

export default function TitleSlideDemo() {
  return (
    <div className="flex min-h-[520px] w-full items-center justify-center p-6">
      <div
        className="relative flex aspect-[16/9] w-full max-w-[1120px] items-center justify-center overflow-hidden rounded-2xl"
        style={{
          background:
            "linear-gradient(135deg, #1b2657 0%, #29386f 45%, #1a2450 100%)",
        }}
      >
        {/* dotted aurora-corner pattern (top-left) */}
        <div
          className="pointer-events-none absolute inset-0"
          style={{
            backgroundImage:
              "radial-gradient(rgba(255,255,255,0.22) 1px, transparent 1px)",
            backgroundSize: "7px 7px",
            WebkitMaskImage:
              "linear-gradient(135deg, black 0%, rgba(0,0,0,0.4) 22%, transparent 40%)",
            maskImage:
              "linear-gradient(135deg, black 0%, rgba(0,0,0,0.4) 22%, transparent 40%)",
          }}
        />
        <TitleSlide className="relative z-10 gap-5 px-8">
          <TitleSlide.Heading className="text-white text-7xl">
            Welcome to Slide-CN
          </TitleSlide.Heading>
          <TitleSlide.SubHeading className="text-white/70">
            Create beautiful presentations using code
          </TitleSlide.SubHeading>
          <TitleSlide.Meta className="text-white/50">
            Internal architecture review - Jan 2026
          </TitleSlide.Meta>
        </TitleSlide>
      </div>
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
