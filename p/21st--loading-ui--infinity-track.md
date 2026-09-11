<!-- Infinity Track · @loading-ui · https://21st.dev/@loading-ui/components/infinity-track
     license: MIT · category: spinner
     A monospace unicode loading indicator that animates staggered block glyphs along an infinity-shaped track. -->

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
components/loading-ui/infinity-track.tsx
import { cn } from "@/lib/utils";

const INFINITY_TRACK_BLOCKS = ["█", "▓", "▒"] as const;

type InfinityTrackProps = React.ComponentProps<"span"> & {
  blocks?: readonly string[];
  track?: string;
};

function InfinityTrack({
  className,
  blocks = INFINITY_TRACK_BLOCKS,
  track = "░",
  ...props
}: InfinityTrackProps) {
  const glyphs = INFINITY_TRACK_BLOCKS.map(
    (_, index) => blocks[index] ?? INFINITY_TRACK_BLOCKS[index],
  );
  const gridCells = Array.from({ length: 45 }, (_, index) => {
    const row = Math.floor(index / 9);
    const col = index % 9;

    return row === 0 || row === 4 || col === 0 || col === 4 || col === 8
      ? track
      : " ";
  });

  return (
    <>
      <style>{`
        @keyframes loading-ui-infinity-track {
          0%,
          100% {
            transform: translate(4ch, 0);
          }

          12.5% {
            transform: translate(0, 0);
          }

          25% {
            transform: translate(0, 4ch);
          }

          37.5% {
            transform: translate(4ch, 4ch);
          }

          50% {
            transform: translate(4ch, 0);
          }

          62.5% {
            transform: translate(8ch, 0);
          }

          75% {
            transform: translate(8ch, 4ch);
          }

          87.5% {
            transform: translate(4ch, 4ch);
          }
        }
      `}</style>
      <span
        role="status"
        className={cn(
          "relative inline-flex h-[5ch] w-[9ch] overflow-hidden font-mono text-xl leading-none text-current select-none",
          className,
        )}
        {...props}
      >
        <span
          aria-hidden="true"
          className="pointer-events-none absolute inset-0 grid grid-cols-9 grid-rows-5"
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
                "loading-ui-infinity-track var(--duration, 3.2s) linear infinite",
              animationDelay: `calc(var(--delay, 0.1s) * ${index})`,
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

export { InfinityTrack };

demo.tsx
import { InfinityTrack } from "@/components/ui/infinity-track";

export default function InfinityTrackDemo() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center bg-background text-foreground">
      <InfinityTrack />
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
