<!-- 8bit CTA 1 — Comparison · @theorcdev · https://21st.dev/@theorcdev/components/8bit-cta1
     license: MIT · category: cta
     An 8-bit styled CTA comparison block: a retro card with a three-column us-vs-them feature table, built on the 8bit card primitive with the Press Start 2P pixel font. -->

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
components/ui/8bit/blocks/cta1.tsx
import { cn } from "@/lib/utils";

import {
  Card,
  CardContent,
  CardHeader,
  CardTitle,
} from "@/components/ui/8bit/card";

import "@/components/ui/8bit/styles/retro.css";

export interface ComparisonRow {
  feature: string;
  theirs: string;
  yours: string;
}

interface CTA1Props {
  className?: string;
  description?: string;
  rows?: ComparisonRow[];
  theirsLabel?: string;
  title?: string;
  yoursLabel?: string;
}

const defaultRows: ComparisonRow[] = [
  { feature: "Setup Time", yours: "2 minutes", theirs: "2 hours" },
  { feature: "Customizable", yours: "+ Full source", theirs: "- Config only" },
  { feature: "Dependencies", yours: "0 runtime", theirs: "5+ packages" },
  { feature: "Dark Mode", yours: "+ Built-in", theirs: "- Manual" },
  { feature: "Pixel Borders", yours: "+ Obviously", theirs: "- Nope" },
  { feature: "Fun Factor", yours: "MAX", theirs: "Corporate" },
];

export default function CTA1({
  title = "Why Us?",
  description = "Side-by-side. No fluff.",
  yoursLabel = "8bitcn",
  theirsLabel = "Others",
  rows = defaultRows,
  className,
}: CTA1Props) {
  return (
    <section className={cn("w-full px-4 py-16", className)}>
      <div className="mx-auto max-w-3xl">
        {(title || description) && (
          <div className="mb-10 text-center">
            {title && (
              <h2 className="retro mb-3 font-bold text-2xl tracking-tight md:text-3xl">
                {title}
              </h2>
            )}
            {description && (
              <p className="retro text-muted-foreground text-[9px]">{description}</p>
            )}
          </div>
        )}

        <Card>
          <CardHeader>
            <div className="grid md:grid-cols-3 gap-4">
              <CardTitle className="retro text-[10px] text-muted-foreground">
                FEATURE
              </CardTitle>
              <CardTitle className="retro text-center text-xs text-primary">
                {yoursLabel}
              </CardTitle>
              <CardTitle className="retro text-center text-xs text-muted-foreground">
                {theirsLabel}
              </CardTitle>
            </div>
          </CardHeader>
          <CardContent>
            <div className="flex flex-col divide-y">
              {rows.map((row) => (
                <div className="grid md:grid-cols-3 gap-4 py-3" key={row.feature}>
                  <span className="text-xs font-medium">{row.feature}</span>
                  <span className="retro text-center text-[10px]">
                    {row.yours}
                  </span>
                  <span className="retro text-center text-[10px] text-muted-foreground">
                    {row.theirs}
                  </span>
                </div>
              ))}
            </div>
          </CardContent>
        </Card>
      </div>
    </section>
  );
}

components/ui/8bit/styles/retro.css
@import url("https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap");

.retro {
  font-family:
    "Press Start 2P",
    system-ui,
    -apple-system,
    sans-serif;
  line-height: 1.5;
  letter-spacing: 0.5px;
}

.pixelated {
  image-rendering: pixelated;
  image-rendering: crisp-edges;
}

components/ui/8bit/card.tsx
import { type VariantProps, cva } from "class-variance-authority";

import { cn } from "@/lib/utils";

import {
  Card as ShadcnCard,
  CardAction as ShadcnCardAction,
  CardContent as ShadcnCardContent,
  CardDescription as ShadcnCardDescription,
  CardFooter as ShadcnCardFooter,
  CardHeader as ShadcnCardHeader,
  CardTitle as ShadcnCardTitle,
} from "@/components/ui/card";

import "@/components/ui/8bit/styles/retro.css";

export const cardVariants = cva("", {
  variants: {
    font: {
      normal: "",
      retro: "retro",
    },
  },
  defaultVariants: {
    font: "retro",
  },
});

export interface BitCardProps
  extends React.ComponentProps<"div">,
    VariantProps<typeof cardVariants> {
  asChild?: boolean;
}

function Card({ className, font, ...props }: BitCardProps) {
  return (
    <div
      className={cn(
        "relative bg-card text-card-foreground border-y-6 border-foreground dark:border-ring p-0!",
        className
      )}
    >
      <ShadcnCard
        {...props}
        className={cn(
          "rounded-none border-0 w-full! h-full flex flex-col bg-card text-card-foreground shadow-none",
          font !== "normal" && "retro",
          className
        )}
      />

      <div
        className={cn("absolute inset-0 border-x-6 -mx-1.5 border-inherit pointer-events-none")}
        aria-hidden="true"
      />
    </div>
  );
}

function CardHeader({ ...props }: BitCardProps) {
  const { className, font } = props;

  return (
    <ShadcnCardHeader
      className={cn(font !== "normal" && "retro", className)}
      {...props}
    />
  );
}

function CardTitle({ ...props }: BitCardProps) {
  const { className, font } = props;

  return (
    <ShadcnCardTitle
      className={cn(font !== "normal" && "retro", className)}
      {...props}
    />
  );
}

function CardDescription({ ...props }: BitCardProps) {
  const { className, font } = props;

  return (
    <ShadcnCardDescription
      className={cn(font !== "normal" && "retro", className)}
      {...props}
    />
  );
}

function CardAction({ ...props }: BitCardProps) {
  const { className, font } = props;

  return (
    <ShadcnCardAction
      className={cn(font !== "normal" && "retro", className)}
      {...props}
    />
  );
}

function CardContent({ ...props }: BitCardProps) {
  const { className, font } = props;

  return (
    <ShadcnCardContent
      className={cn("flex-1", font !== "normal" && "retro", className)}
      {...props}
    />
  );
}

function CardFooter({ ...props }: BitCardProps) {
  const { className, font } = props;

  return (
    <ShadcnCardFooter
      data-slot="card-footer"
      className={cn(font !== "normal" && "retro", className)}
      {...props}
    />
  );
}

export {
  Card,
  CardHeader,
  CardFooter,
  CardTitle,
  CardAction,
  CardDescription,
  CardContent,
};

demo.tsx
import CTA1 from "@/components/ui/8bit-cta1";

export default function Demo() {
  return (
    <div className="retro flex min-h-screen w-full items-center justify-center p-6">
      <CTA1 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority tailwindcss tw-animate-css
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add card
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
