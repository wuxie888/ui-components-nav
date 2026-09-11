<!-- Text Underline · @soralabs · https://21st.dev/@soralabs/components/text-underline
     license: no-license · category: text
     An animated inline link with a CSS-only underline that slides through on hover. -->

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
components/ui/index.tsx
"use client";

import { cn } from "@/lib/utils";
import { cva, type VariantProps } from "class-variance-authority";
import type {
  ComponentPropsWithoutRef,
  CSSProperties,
  ReactNode,
  Ref,
} from "react";

const COLOR_PRESETS = {
  currentColor: "currentColor",
  brand: "#fd551d",
  "brand-muted": "#fd8d68",
  gray: "#737373",
} as const;

const HEIGHT_PRESETS = {
  thin: "1px",
  medium: "2px",
  thick: "3px",
} as const;

const DURATION_PRESETS = {
  fast: "0.3s",
  normal: "0.4s",
  slow: "0.6s",
} as const;

const INITIAL_SCALE_PRESETS = {
  "0": "0",
  "25": "0.25",
  "50": "0.5",
  "75": "0.75",
} as const;

const textUnderlineVariants = cva(
  [
    "relative inline-block cursor-pointer pb-1 font-normal text-foreground text-lg leading-[1.4] no-underline antialiased",
    "after:absolute after:bottom-0 after:left-0 after:w-full after:will-change-transform after:content-['']",
    "after:transition-transform after:ease-[cubic-bezier(0.16,1,0.3,1)] motion-reduce:after:scale-x-100 motion-reduce:after:transition-none",
    "after:h-[var(--tu-height)] after:bg-[var(--tu-color)] after:duration-[var(--tu-duration)]",
    "after:scale-x-[var(--tu-initial-scale)]",
    "hover:after:scale-x-100 focus-visible:after:scale-x-100",
    "rounded-sm focus-visible:outline-2 focus-visible:outline-[#fd8d68] focus-visible:outline-offset-4",
    "focus:not(:focus-visible):outline-none",
  ],
  {
    variants: {
      anchor: {
        left: [
          "after:origin-right",
          "hover:after:origin-left focus-visible:after:origin-left",
        ],
        center: [
          "after:origin-center",
          "hover:after:origin-center focus-visible:after:origin-center",
        ],
      },
    },
    defaultVariants: {
      anchor: "left",
    },
  }
);

type ColorPreset = keyof typeof COLOR_PRESETS;
type HeightPreset = keyof typeof HEIGHT_PRESETS;
type DurationPreset = keyof typeof DURATION_PRESETS;
type InitialScalePreset = keyof typeof INITIAL_SCALE_PRESETS;

function resolveUnderlineColor(color: string): string {
  if (color in COLOR_PRESETS) {
    return COLOR_PRESETS[color as ColorPreset];
  }

  return color;
}

function resolveUnderlineHeight(height: HeightPreset): string {
  return HEIGHT_PRESETS[height];
}

function resolveDuration(duration: DurationPreset): string {
  return DURATION_PRESETS[duration];
}

function resolveInitialScale(scale: InitialScalePreset): string {
  return INITIAL_SCALE_PRESETS[scale];
}

export interface TextUnderlineProps
  extends Omit<ComponentPropsWithoutRef<"a">, "children">,
    VariantProps<typeof textUnderlineVariants> {
  /** Scale origin for the underline animation. */
  anchor?: "left" | "center";
  children?: ReactNode;
  /**
   * Animation speed preset.
   * @default "normal"
   */
  duration?: DurationPreset;
  /**
   * Initial underline width before hover, as a percentage of the link width.
   * @default "0"
   */
  initialScale?: InitialScalePreset;
  /** Alternative to `children` for demos and controlled previews. */
  label?: string;
  /**
   * Underline color preset or any valid CSS color.
   * @default "currentColor"
   */
  underlineColor?: ColorPreset | (string & {});
  /**
   * Underline thickness preset.
   * @default "thin"
   */
  underlineHeight?: HeightPreset;
}

function TextUnderline({
  anchor = "left",
  children,
  className,
  duration = "normal",
  href = "#",
  initialScale = "0",
  label,
  ref,
  underlineColor = "currentColor",
  underlineHeight = "thin",
  ...props
}: TextUnderlineProps & { ref?: Ref<HTMLAnchorElement> }) {
  const content = children ?? label;

  const style = {
    "--tu-color": resolveUnderlineColor(underlineColor),
    "--tu-duration": resolveDuration(duration),
    "--tu-height": resolveUnderlineHeight(underlineHeight),
    "--tu-initial-scale": resolveInitialScale(initialScale),
  } as CSSProperties;

  return (
    <a
      className={cn(textUnderlineVariants({ anchor, className }))}
      href={href}
      ref={ref}
      style={style}
      {...props}
    >
      {content}
    </a>
  );
}

export { TextUnderline, textUnderlineVariants };

demo.tsx
import { TextUnderline } from "@/components/ui/text-underline";

export default function Default() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center bg-background px-6 text-foreground">
      <p className="text-2xl leading-relaxed text-foreground">
        Shop the{" "}
        <TextUnderline href="#" underlineColor="brand" underlineHeight="medium">
          Performance Collection
        </TextUnderline>{" "}
        built for speed.
      </p>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add utils
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
