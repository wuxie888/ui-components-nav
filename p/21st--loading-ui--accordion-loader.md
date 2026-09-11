<!-- Accordion Loader · @loading-ui · https://21st.dev/@loading-ui/components/accordion-loader
     license: no-license · category: progress
     An ASCII block loading indicator whose stacked glyphs slide across a track like an accordion to signal an in-progress state. -->

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
components/loading-ui/accordion-loader.tsx
import { cn } from "@/lib/utils";

const ACCORDION_LOADER_BLOCKS = ["█", "▓", "▒"] as const;

type AccordionLoaderProps = React.ComponentProps<"span"> & {
  blocks?: readonly string[];
  track?: string;
  trackLength?: number;
};

function AccordionLoader({
  className,
  blocks = ACCORDION_LOADER_BLOCKS,
  track = "░",
  trackLength = 16,
  style,
  ...props
}: AccordionLoaderProps) {
  const columns = Math.max(2, Math.floor(trackLength));
  const glyphs = ACCORDION_LOADER_BLOCKS.map(
    (_, index) => blocks[index] ?? ACCORDION_LOADER_BLOCKS[index],
  );

  return (
    <>
      <style>{`
        @keyframes loading-ui-accordion-loader {
          0%,
          100% {
            transform: translateX(0);
          }

          50% {
            transform: translateX(var(--loader-x));
          }
        }
      `}</style>
      <span
        role="status"
        className={cn(
          "relative inline-flex h-[1em] w-[var(--loader-width)] items-center overflow-hidden font-mono text-xl leading-none text-current select-none",
          className,
        )}
        style={
          {
            "--loader-width": `${columns}ch`,
            "--loader-x": `${columns - 1}ch`,
            ...style,
          } as React.CSSProperties
        }
        {...props}
      >
        <span
          aria-hidden="true"
          className="pointer-events-none absolute inset-0 whitespace-nowrap"
        >
          {track.repeat(columns)}
        </span>
        {glyphs.map((glyph, index) => (
          <span
            key={`${glyph}-${index}`}
            aria-hidden="true"
            className={cn(
              "pointer-events-none absolute top-0 left-0 flex h-full w-[1ch] items-center justify-center text-center",
              ["z-30", "z-20", "z-10"][index],
            )}
            style={{
              animation:
                "loading-ui-accordion-loader var(--duration, 2.8s) ease-in-out infinite",
              animationDelay: `calc(var(--delay, 0.04s) * ${index})`,
              backgroundColor: "var(--mask-color, var(--background))",
            }}
          >
            {glyph}
          </span>
        ))}
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { AccordionLoader };

demo.tsx
import { AccordionLoader } from "@/components/ui/accordion-loader";

export default function AccordionLoaderDemo() {
  return (
    <div className="flex min-h-56 w-full items-center justify-center bg-background text-foreground">
      <AccordionLoader />
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
