<!-- Pricing Section · @mikolajdobrucki · https://21st.dev/@mikolajdobrucki/components/pricing
     license: MIT · category: cta
     A responsive pricing section with feature-list plan cards, glass effects and highlighted tiers for landing pages. -->

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
components/sections/pricing/default.tsx
import { User, Users } from "lucide-react";

import { cn } from "@/lib/utils";

import { PricingColumn, PricingColumnProps } from "../../ui/pricing-column";
import { Section } from "../../ui/section";

interface PricingProps {
  title?: string | false;
  description?: string | false;
  plans?: PricingColumnProps[] | false;
  className?: string;
}

const DEFAULT_PRICING_PLANS: PricingColumnProps[] = [
  {
    name: "Free",
    description: "For everyone starting out on a website for their big idea",
    price: 0,
    priceNote: "Free and open-source forever. Get started now.",
    cta: {
      variant: "glow",
      label: "Get started for free",
      href: "https://www.launchuicomponents.com/",
    },
    features: [
      "1 website template",
      "9 blocks and sections",
      "4 custom animations",
    ],
    variant: "default",
    className: "hidden lg:flex",
  },
  {
    name: "Pro",
    icon: <User className="size-4" />,
    description: "For early-stage founders, solopreneurs and indie devs",
    price: 99,
    priceNote: "Lifetime access. Free updates. No recurring fees.",
    cta: {
      variant: "default",
      label: "Get all-access",
      href: "https://www.launchuicomponents.com/",
    },
    features: [
      `$1000 templates`,
      `$1000 blocks and sections`,
      `$1000 illustrations`,
      `$1000 custom animations`,
    ],
    variant: "glow-brand",
  },
  {
    name: "Pro Team",
    icon: <Users className="size-4" />,
    description: "For teams and agencies working on cool products together",
    price: 499,
    priceNote: "Lifetime access. Free updates. No recurring fees.",
    cta: {
      variant: "default",
      label: "Get all-access for your team",
      href: "https://www.launchuicomponents.com/",
    },
    features: [
      "All the templates, components and sections available for your entire team",
    ],
    variant: "glow",
  },
];

export default function Pricing({
  title = "Build your dream landing page, today.",
  description = "Get lifetime access to all the components. No recurring fees. Just simple, transparent pricing.",
  plans = DEFAULT_PRICING_PLANS,
  className = "",
}: PricingProps) {
  return (
    <Section className={cn(className)}>
      <div className="mx-auto flex max-w-6xl flex-col items-center gap-12">
        {(title || description) && (
          <div className="flex flex-col items-center gap-4 px-4 text-center sm:gap-8">
            {title && (
              <h2 className="text-3xl leading-tight font-semibold sm:text-5xl sm:leading-tight">
                {title}
              </h2>
            )}
            {description && (
              <p className="text-md text-muted-foreground max-w-[600px] font-medium sm:text-xl">
                {description}
              </p>
            )}
          </div>
        )}
        {plans !== false && plans.length > 0 && (
          <div className="max-w-container mx-auto grid grid-cols-1 gap-8 sm:grid-cols-2 lg:grid-cols-3">
            {plans.map((plan) => (
              <PricingColumn
                key={plan.name}
                name={plan.name}
                icon={plan.icon}
                description={plan.description}
                price={plan.price}
                originalPrice={plan.originalPrice}
                promotionText={plan.promotionText}
                priceNote={plan.priceNote}
                cta={plan.cta}
                features={plan.features}
                variant={plan.variant}
                className={plan.className}
              />
            ))}
          </div>
        )}
      </div>
    </Section>
  );
}

components/ui/pricing-column.tsx
import { cva, type VariantProps } from "class-variance-authority";
import { CircleCheckBig } from "lucide-react";
import Link from "next/link";
import { ReactNode } from "react";

import { cn } from "@/lib/utils";

import { Button } from "./button";

const pricingColumnVariants = cva(
  "max-w-container relative flex flex-col gap-6 overflow-hidden rounded-2xl p-8 shadow-xl",
  {
    variants: {
      variant: {
        default: "glass-1 to-transparent dark:glass-3",
        glow: "glass-2 to-transparent dark:glass-3 after:content-[''] after:absolute after:-top-[128px] after:left-1/2 after:h-[128px] after:w-[100%] after:max-w-[960px] after:-translate-x-1/2 after:rounded-[50%] dark:after:bg-foreground/30 after:blur-[72px]",
        "glow-brand":
          "glass-3 from-card/100 to-card/100 dark:glass-4 after:content-[''] after:absolute after:-top-[128px] after:left-1/2 after:h-[128px] after:w-[100%] after:max-w-[960px] after:-translate-x-1/2 after:rounded-[50%] after:bg-brand-foreground/70 after:blur-[72px]",
      },
    },
    defaultVariants: {
      variant: "default",
    },
  },
);

export interface PricingColumnProps
  extends
    React.HTMLAttributes<HTMLDivElement>,
    VariantProps<typeof pricingColumnVariants> {
  name: string;
  icon?: ReactNode;
  description: string;
  price: number;
  originalPrice?: number;
  promotionText?: ReactNode;
  priceNote: string;
  cta: {
    variant: "glow" | "default";
    label: string;
    href: string;
  };
  features: string[];
}

export function PricingColumn({
  name,
  icon,
  description,
  price,
  originalPrice,
  promotionText,
  priceNote,
  cta,
  features,
  variant,
  className,
  ...props
}: PricingColumnProps) {
  return (
    <div
      className={cn(pricingColumnVariants({ variant, className }))}
      {...props}
    >
      <hr
        className={cn(
          "via-foreground/60 absolute top-0 left-[10%] h-[1px] w-[80%] border-0 bg-linear-to-r from-transparent to-transparent",
          variant === "glow-brand" && "via-brand",
        )}
      />
      <div className="flex flex-col gap-7">
        <header className="flex flex-col gap-2">
          <h2 className="flex items-center gap-2 font-bold">
            {icon && (
              <div className="text-muted-foreground flex items-center gap-2">
                {icon}
              </div>
            )}
            {name}
          </h2>
          <p className="text-muted-foreground max-w-[220px] text-sm">
            {description}
          </p>
        </header>
        <section className="flex flex-col gap-3">
          {originalPrice !== undefined && (
            <div className="flex h-6 items-baseline gap-1">
              <span className="text-muted-foreground text-lg font-medium line-through">
                {originalPrice > 0 && price !== originalPrice
                  ? `$${originalPrice}`
                  : ""}
              </span>
            </div>
          )}
          <div className="flex items-center gap-3 lg:flex-col lg:items-start xl:flex-row xl:items-center">
            <div className="flex flex-col gap-1">
              <div className="flex items-baseline gap-1">
                <span className="text-muted-foreground text-2xl font-bold">
                  $
                </span>
                <span className="text-6xl font-bold">{price}</span>
              </div>
            </div>
            <div className="flex min-h-[40px] flex-col">
              {price > 0 && (
                <>
                  <span className="text-sm">one-time payment</span>
                  <span className="text-muted-foreground text-sm">
                    plus local taxes
                  </span>
                </>
              )}
            </div>
          </div>
          {promotionText && (
            <div className="text-brand-foreground h-6 text-sm font-medium">
              {promotionText}
            </div>
          )}
        </section>
        <Button variant={cta.variant} size="lg" asChild>
          <Link href={cta.href}>{cta.label}</Link>
        </Button>
        <p className="text-muted-foreground min-h-[40px] max-w-[220px] text-sm">
          {priceNote}
        </p>
        <hr className="border-input" />
      </div>
      <div>
        <ul className="flex flex-col gap-2">
          {features.map((feature, index) => (
            <li
              key={`${feature}-${index}`}
              className="flex items-center gap-2 text-sm"
            >
              <CircleCheckBig className="text-muted-foreground size-4 shrink-0" />
              {feature}
            </li>
          ))}
        </ul>
      </div>
    </div>
  );
}

export { pricingColumnVariants };

demo.tsx
import Pricing from "@/components/ui/pricing";

export default function PricingDemo() {
  return <Pricing />;
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-slot lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button button.json
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
