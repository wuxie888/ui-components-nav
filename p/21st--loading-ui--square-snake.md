<!-- Square Snake · @loading-ui · https://21st.dev/@loading-ui/components/square-snake
     license: MIT · category: spinner
     An ASCII block loading spinner where shaded square glyphs chase each other around a square path. -->

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
components/loading-ui/square-snake.tsx
import { cn } from "@/lib/utils";

const SQUARE_SNAKE_BLOCKS = ["█", "▓", "▒", "░"] as const;

type SquareSnakeProps = React.ComponentProps<"span"> & {
  blocks?: readonly string[];
  size?: number;
};

function SquareSnake({
  className,
  blocks = SQUARE_SNAKE_BLOCKS,
  size = 5,
  style,
  ...props
}: SquareSnakeProps) {
  const cells = Math.max(2, Math.floor(size));
  const glyphs = SQUARE_SNAKE_BLOCKS.map(
    (_, index) => blocks[index] ?? SQUARE_SNAKE_BLOCKS[index],
  );

  return (
    <>
      <style>{`
        @keyframes loading-ui-square-snake {
          0%,
          100% {
            transform: translate(0, 0);
          }

          25% {
            transform: translate(var(--loader-x), 0);
          }

          50% {
            transform: translate(var(--loader-x), var(--loader-y));
          }

          75% {
            transform: translate(0, var(--loader-y));
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
        {glyphs.map((glyph, index) => (
          <span
            key={`${glyph}-${index}`}
            aria-hidden="true"
            className="pointer-events-none absolute top-0 left-0 flex h-[1ch] w-[1ch] items-center justify-center"
            style={{
              animation:
                "loading-ui-square-snake var(--duration, 2s) linear infinite",
              animationDelay: `calc(var(--delay, 0.125s) * -${glyphs.length - 1 - index})`,
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

export { SquareSnake };

demo.tsx
import { SquareSnake } from "@/components/ui/square-snake";

export default function SquareSnakeDemo() {
  return (
    <div className="flex min-h-[200px] items-center justify-center">
      <SquareSnake />
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
