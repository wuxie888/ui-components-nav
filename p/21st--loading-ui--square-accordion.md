<!-- Square Accordion Loader · @loading-ui · https://21st.dev/@loading-ui/components/square-accordion
     license: MIT · category: spinner
     An animated ASCII square loader whose block glyphs trace the perimeter like an accordion for loading states. -->

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
components/loading-ui/square-accordion.tsx
import { cn } from "@/lib/utils";

const SQUARE_ACCORDION_BLOCKS = ["█", "▓", "▒"] as const;

type SquareAccordionProps = React.ComponentProps<"span"> & {
  blocks?: readonly string[];
  size?: number;
  track?: string;
};

function SquareAccordion({
  className,
  blocks = SQUARE_ACCORDION_BLOCKS,
  size = 5,
  track = "░",
  style,
  ...props
}: SquareAccordionProps) {
  const cells = Math.max(2, Math.floor(size));
  const glyphs = SQUARE_ACCORDION_BLOCKS.map(
    (_, index) => blocks[index] ?? SQUARE_ACCORDION_BLOCKS[index],
  );
  const gridCells = Array.from({ length: cells * cells }, (_, index) => {
    const row = Math.floor(index / cells);
    const col = index % cells;

    return row === 0 || row === cells - 1 || col === 0 || col === cells - 1
      ? track
      : " ";
  });

  return (
    <>
      <style>{`
        @keyframes loading-ui-square-accordion {
          0% {
            transform: translate(0, 0);
          }

          15%,
          25% {
            transform: translate(var(--loader-x), 0);
          }

          40%,
          50% {
            transform: translate(var(--loader-x), var(--loader-y));
          }

          65%,
          75% {
            transform: translate(0, var(--loader-y));
          }

          90%,
          100% {
            transform: translate(0, 0);
          }
        }
      `}</style>
      <span
        role="status"
        className={cn(
          "relative inline-flex h-[var(--loader-size)] w-[var(--loader-size)] overflow-hidden font-mono text-xl leading-none text-current select-none",
          className,
        )}
        style={
          {
            "--loader-size": `${cells}ch`,
            "--loader-x": `${cells - 1}ch`,
            "--loader-y": `${cells - 1}ch`,
            ...style,
          } as React.CSSProperties
        }
        {...props}
      >
        <span
          aria-hidden="true"
          className="pointer-events-none absolute inset-0 grid"
          style={{
            gridTemplateColumns: `repeat(${cells}, 1ch)`,
            gridTemplateRows: `repeat(${cells}, 1ch)`,
          }}
        >
          {gridCells.map((glyph, index) => (
            <span
              key={index}
              className="flex h-[1ch] w-[1ch] items-center justify-center"
            >
              {glyph}
            </span>
          ))}
        </span>
        {glyphs.map((glyph, index) => (
          <span
            key={`${glyph}-${index}`}
            aria-hidden="true"
            className={cn(
              "pointer-events-none absolute top-0 left-0 flex h-[1ch] w-[1ch] items-center justify-center",
              ["z-30", "z-20", "z-10"][index],
            )}
            style={{
              animation:
                "loading-ui-square-accordion var(--duration, 3.5s) linear infinite",
              animationDelay: `calc(var(--delay, 0.08s) * ${index})`,
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

export { SquareAccordion };

demo.tsx
import { SquareAccordion } from "@/components/ui/square-accordion";

export default function SquareAccordionDemo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center">
      <SquareAccordion />
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
