<!-- Symmetric Wave · @loading-ui · https://21st.dev/@loading-ui/components/symmetric-wave
     license: MIT · category: spinner
     An ASCII-style animated loading indicator where blocks pulse in a symmetric wave pattern. -->

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
components/loading-ui/symmetric-wave.tsx
import { cn } from "@/lib/utils";

const SYMMETRIC_WAVE_PHASES = [1, 2, 3, 4, 5, 5, 4, 3, 2, 1] as const;

type SymmetricWaveProps = React.ComponentProps<"span"> & {
  block?: string;
  track?: string;
};

function SymmetricWave({
  className,
  block = "█",
  track = "░",
  ...props
}: SymmetricWaveProps) {
  return (
    <>
      <style>{`
        @keyframes loading-ui-symmetric-wave-1 {
          0%,
          100% {
            opacity: 1;
          }

          12.5%,
          87.5% {
            opacity: 0.6;
          }

          25%,
          75% {
            opacity: 0.3;
          }

          37.5%,
          62.5%,
          50% {
            opacity: 0;
          }
        }

        @keyframes loading-ui-symmetric-wave-2 {
          0%,
          100% {
            opacity: 0.6;
          }

          12.5%,
          87.5% {
            opacity: 1;
          }

          25%,
          75% {
            opacity: 0.6;
          }

          37.5%,
          62.5% {
            opacity: 0.3;
          }

          50% {
            opacity: 0;
          }
        }

        @keyframes loading-ui-symmetric-wave-3 {
          0%,
          100% {
            opacity: 0.3;
          }

          12.5%,
          87.5% {
            opacity: 0.6;
          }

          25%,
          75% {
            opacity: 1;
          }

          37.5%,
          62.5% {
            opacity: 0.6;
          }

          50% {
            opacity: 0.3;
          }
        }

        @keyframes loading-ui-symmetric-wave-4 {
          0%,
          100% {
            opacity: 0;
          }

          12.5%,
          87.5% {
            opacity: 0.3;
          }

          25%,
          75% {
            opacity: 0.6;
          }

          37.5%,
          62.5% {
            opacity: 1;
          }

          50% {
            opacity: 0.6;
          }
        }

        @keyframes loading-ui-symmetric-wave-5 {
          0%,
          100%,
          12.5%,
          87.5% {
            opacity: 0;
          }

          25%,
          75% {
            opacity: 0.3;
          }

          37.5%,
          62.5% {
            opacity: 0.6;
          }

          50% {
            opacity: 1;
          }
        }
      `}</style>
      <span
        role="status"
        className={cn(
          "relative inline-flex h-[1em] w-[10ch] overflow-hidden font-mono text-xl leading-none text-current select-none",
          className,
        )}
        {...props}
      >
        {SYMMETRIC_WAVE_PHASES.map((phase, index) => (
          <span
            key={index}
            aria-hidden="true"
            className="relative flex h-full w-[1ch] items-center justify-center"
          >
            <span className="opacity-30">{track}</span>
            <span
              className="absolute inset-0 flex items-center justify-center"
              style={{
                animation: `loading-ui-symmetric-wave-${phase} var(--duration, 2s) linear infinite`,
              }}
            >
              {block}
            </span>
          </span>
        ))}
        <span className="sr-only">Loading</span>
      </span>
    </>
  );
}

export { SymmetricWave };

demo.tsx
import { SymmetricWave } from "@/components/ui/symmetric-wave";

export default function SymmetricWaveDemo() {
  return (
    <div className="flex min-h-64 items-center justify-center">
      <SymmetricWave />
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
