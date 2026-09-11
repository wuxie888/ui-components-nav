<!-- Conveyor Loop · @loading-ui · https://21st.dev/@loading-ui/components/conveyor-loop
     license: MIT · category: progress
     An ASCII-style loading indicator that moves a staggered unicode block trail across a fixed text track in a one-way conveyor loop. -->

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
components/loading-ui/conveyor-loop.tsx
import { cn } from "@/lib/utils";

const CONVEYOR_LOOP_BLOCKS = ["█", "▓", "▒"] as const;

type ConveyorLoopProps = React.ComponentProps<"span"> & {
  blocks?: readonly string[];
  track?: string;
  trackLength?: number;
};

function ConveyorLoop({
  className,
  blocks = CONVEYOR_LOOP_BLOCKS,
  track = "░",
  trackLength = 10,
  style,
  ...props
}: ConveyorLoopProps) {
  const columns = Math.max(2, Math.floor(trackLength));
  const glyphs = CONVEYOR_LOOP_BLOCKS.map(
    (_, index) => blocks[index] ?? CONVEYOR_LOOP_BLOCKS[index],
  );
  const tailOffset = glyphs.length - 1;

  return (
    <>
      <style>{`
        @keyframes loading-ui-conveyor-loop {
          0% {
            transform: translateX(var(--loader-start-x));
          }

          100% {
            transform: translateX(var(--loader-end-x));
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
            "--loader-start-x": `-${tailOffset}ch`,
            "--loader-end-x": `${columns + tailOffset}ch`,
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
                "loading-ui-conveyor-loop var(--duration, 1.8s) linear infinite",
              animationDelay: `calc(var(--delay, 0.05s) * ${index})`,
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

export { ConveyorLoop };

demo.tsx
import { ConveyorLoop } from "@/components/ui/conveyor-loop";

export default function ConveyorLoopDemo() {
  return (
    <div className="flex min-h-[200px] items-center justify-center">
      <span className="inline-flex items-center font-mono text-xl text-foreground">
        <ConveyorLoop />
        Loading
      </span>
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
