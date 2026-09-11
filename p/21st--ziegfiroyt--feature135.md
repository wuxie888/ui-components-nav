<!-- Integration Logos Grid · @ziegfiroyt · https://21st.dev/@ziegfiroyt/components/feature135
     license: MIT · category: grid
     A centered flexbox grid of company or product logos with grayscale hover effects, ideal for showcasing customers, integrations, or partners. -->

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
components/ui/feature135.tsx
"use client";

import { Badge } from "@/components/ui/badge";
import { Button } from "@/components/ui/button";
import Link from "next/link";
import { cn } from "@/lib/utils";

interface LogoItem {
  name: string;
  logo: string;
}

interface ButtonItem {
  label: string;
  href?: string;
  variant?: "default" | "secondary" | "outline" | "ghost" | "link" | "destructive";
}

interface Feature135Props {
  badge?: {
    label: string;
    variant?: "default" | "secondary" | "outline";
  };
  heading?: string;
  description?: string;
  logos?: LogoItem[];
  buttons?: ButtonItem[];
  className?: string;
}

export const feature135Demo: Feature135Props = {
  badge: { label: "Integrations", variant: "default" },
  heading: "Works with your stack",
  description: "Connect with the tools you already use.",
  logos: [
    { name: "Nike", logo: "https://oud.pics/sm/l/nike.png" },
    { name: "Adidas", logo: "https://oud.pics/sm/l/adidas.png" },
    { name: "Puma", logo: "https://oud.pics/sm/l/puma.png" },
    { name: "Converse", logo: "https://oud.pics/sm/l/converse.png" },
  ],
  buttons: [{ label: "View All Integrations", href: "https://beste.co" }],
};

export function Feature135({
  badge,
  heading,
  description,
  logos = [],
  buttons = [],
  className,
}: Feature135Props) {
  return (
    <section className={cn("py-16 md:py-24 w-full", className)}>
      <div className="mx-auto max-w-6xl px-4 md:px-6">
        {(badge || heading || description) && (
          <div className="mx-auto mb-12 max-w-3xl text-center">
            {badge && (
              <div className="mb-4 flex justify-center">
                <Badge variant={badge.variant ?? "default"}>{badge.label}</Badge>
              </div>
            )}
            {heading && <h2 className="text-3xl font-semibold md:text-5xl">{heading}</h2>}
            {description && (
              <p className="mt-4 text-base md:text-lg text-muted-foreground">{description}</p>
            )}
          </div>
        )}

        <div className="flex flex-wrap items-center justify-center gap-x-12 gap-y-8">
          {logos.map((item, index) => (
            <div
              key={index}
              className="flex h-10 w-24 items-center justify-center opacity-50 grayscale transition-all hover:opacity-100 hover:grayscale-0"
            >
              <img
                src={item.logo}
                alt={item.name}
                width={96}
                height={40}
                className="max-h-full w-auto object-contain"
              />
            </div>
          ))}
        </div>

        {buttons.length > 0 && (
          <div className="mt-12 flex flex-wrap justify-center gap-3">
            {buttons.map((button, index) => (
              <Button key={index} variant={button.variant ?? "default"} asChild>
                <Link href={button.href ?? "#"}>{button.label}</Link>
              </Button>
            ))}
          </div>
        )}
      </div>
    </section>
  );
}

demo.tsx
import { Feature135 } from "@/components/ui/feature135";

export default function Feature135Demo() {
  return (
    <Feature135
      badge={{ label: "Integrations", variant: "default" }}
      heading="Works with your stack"
      description="Connect with the tools you already use."
      logos={[
        { name: "Nike", logo: "https://cdn.21st.dev/assets/mirror/25/25c1a531576ebf955657b3e926b62a5da994292f6429a56bf8700021871b993d.png" },
        { name: "Adidas", logo: "https://cdn.21st.dev/assets/mirror/f6/f6213d396bec055b075ee6cdc461d8ced5e086cb2411e2aa4e260b8b5135c22c.png" },
        { name: "Puma", logo: "https://cdn.21st.dev/assets/mirror/8f/8fc20985c7b88bfbc6abc29ece54dc3355fad2281d76af039ea5ef3ffe8bedf3.png" },
        { name: "Converse", logo: "https://cdn.21st.dev/assets/mirror/11/116b235857b64f2d253469401a87db39504b21ea3bdc45ad4e02bdddceb4979f.png" },
      ]}
      buttons={[{ label: "View All Integrations", href: "https://beste.co" }]}
    />
  );
}
```

Install NPM dependencies:
```bash
npm install clsx lucide-react tailwind-merge
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge button
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
