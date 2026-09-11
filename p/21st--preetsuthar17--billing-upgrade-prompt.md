<!-- Billing Upgrade Prompt · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/billing-upgrade-prompt
     license: MIT · category: pricing-section
     A plan upgrade prompt that highlights recommended plan pricing, savings and feature benefits with banner and card layouts. -->

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
components/ui/billing-upgrade-prompt.tsx
"use client";

import { Check, Loader2, Sparkles, TrendingUp, X } from "lucide-react";
import { useState } from "react";
import { cn } from "@/lib/utils";
import { Badge } from "@/registry/new-york/ui/badge";
import { Button } from "@/registry/new-york/ui/button";
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from "@/registry/new-york/ui/card";
import { Separator } from "@/registry/new-york/ui/separator";

export interface UpgradeFeature {
  name: string;
  description?: string;
  icon?: React.ComponentType<{ className?: string }>;
}

export interface BillingUpgradePromptProps {
  currentPlan: {
    id: string;
    name: string;
  };
  recommendedPlan: {
    id: string;
    name: string;
    price: number;
    currency?: string;
    billingPeriod?: "monthly" | "annual";
  };
  features?: UpgradeFeature[];
  reason?: "usage_limit" | "feature_unlock" | "recommended" | "custom";
  customMessage?: string;
  onUpgrade?: () => void;
  onDismiss?: () => void;
  onLearnMore?: () => void;
  className?: string;
  variant?: "banner" | "card" | "modal";
  showSavings?: boolean;
  savingsAmount?: number;
  limitedTime?: boolean;
}

function formatPrice(amount: number, currency = "USD"): string {
  return new Intl.NumberFormat("en-US", {
    style: "currency",
    currency,
    minimumFractionDigits: 0,
    maximumFractionDigits: 0,
  }).format(amount);
}

export default function BillingUpgradePrompt({
  currentPlan,
  recommendedPlan,
  features = [],
  reason = "recommended",
  customMessage,
  onUpgrade,
  onDismiss,
  onLearnMore,
  className,
  variant = "card",
  showSavings = false,
  savingsAmount = 0,
  limitedTime = false,
}: BillingUpgradePromptProps) {
  const [isLoading, setIsLoading] = useState(false);
  const [isDismissed, setIsDismissed] = useState(false);

  const handleUpgrade = async () => {
    setIsLoading(true);
    try {
      await onUpgrade?.();
    } finally {
      setIsLoading(false);
    }
  };

  const handleDismiss = () => {
    setIsDismissed(true);
    onDismiss?.();
  };

  if (isDismissed) return null;

  const getReasonMessage = () => {
    switch (reason) {
      case "usage_limit":
        return `You've used 90% of your ${currentPlan.name} plan limit. Upgrade to continue using all features.`;
      case "feature_unlock":
        return `Unlock advanced features with ${recommendedPlan.name}.`;
      case "recommended":
        return `${recommendedPlan.name} is recommended for your usage.`;
      case "custom":
        return (
          customMessage ||
          `Upgrade to ${recommendedPlan.name} for more features.`
        );
      default:
        return `Upgrade to ${recommendedPlan.name} for more features.`;
    }
  };

  const currency = recommendedPlan.currency || "USD";
  const price = formatPrice(recommendedPlan.price, currency);
  const period =
    recommendedPlan.billingPeriod === "annual" ? "/year" : "/month";

  if (variant === "banner") {
    return (
      <div
        className={cn(
          "flex flex-col gap-4 rounded-lg border bg-linear-to-r from-primary/10 to-primary/5 p-4 sm:flex-row sm:items-center sm:justify-between",
          className
        )}
      >
        <div className="flex min-w-0 flex-1 flex-col gap-2">
          <div className="flex flex-wrap items-center gap-2">
            <Sparkles className="size-5 shrink-0 text-primary" />
            <h3 className="font-semibold text-sm">
              Upgrade to {recommendedPlan.name}
            </h3>
            {limitedTime && (
              <Badge className="bg-primary text-primary-foreground text-xs">
                Limited time
              </Badge>
            )}
          </div>
          <p className="wrap-break-word text-muted-foreground text-sm">
            {getReasonMessage()}
          </p>
          {features.length > 0 && (
            <div className="flex flex-wrap items-center gap-2 text-sm">
              {features.slice(0, 3).map((feature, idx) => (
                <div className="flex items-center gap-1" key={idx}>
                  <Check className="size-3.5 text-primary" />
                  <span className="wrap-break-word">{feature.name}</span>
                </div>
              ))}
              {features.length > 3 && (
                <span className="text-muted-foreground">
                  +{features.length - 3} more
                </span>
              )}
            </div>
          )}
        </div>
        <div className="flex shrink-0 flex-col gap-2 sm:flex-row">
          {onDismiss && (
            <Button
              aria-label="Dismiss upgrade prompt"
              onClick={handleDismiss}
              size="icon"
              type="button"
              variant="ghost"
            >
              <X className="size-4" />
            </Button>
          )}
          {onLearnMore && (
            <Button onClick={onLearnMore} type="button" variant="outline">
              Learn more
            </Button>
          )}
          {onUpgrade && (
            <Button
              aria-busy={isLoading}
              data-loading={isLoading}
              onClick={handleUpgrade}
              type="button"
            >
              {isLoading ? (
                <>
                  <Loader2 className="size-4 animate-spin" />
                  Upgrading…
                </>
              ) : (
                <>
                  <TrendingUp className="size-4" />
                  Upgrade to {price}
                  {period}
                </>
              )}
            </Button>
          )}
        </div>
      </div>
    );
  }

  return (
    <Card className={cn("relative w-full shadow-xs", className)}>
      {onDismiss && (
        <Button
          aria-label="Dismiss upgrade prompt"
          className="absolute top-4 right-4 z-10"
          onClick={handleDismiss}
          size="icon"
          type="button"
          variant="ghost"
        >
          <X className="size-4" />
        </Button>
      )}
      <CardHeader>
        <div className="flex min-w-0 flex-1 flex-col gap-2 pr-8">
          <div className="flex flex-wrap items-center gap-2">
            <CardTitle className="wrap-break-word">
              Upgrade to {recommendedPlan.name}
            </CardTitle>
            {limitedTime && (
              <Badge className="shrink-0 bg-primary text-primary-foreground text-xs">
                Limited time
              </Badge>
            )}
          </div>
          <CardDescription className="wrap-break-word">
            {getReasonMessage()}
          </CardDescription>
        </div>
      </CardHeader>
      <CardContent>
        <div className="flex flex-col gap-6">
          <div className="flex flex-col gap-2 sm:flex-row sm:items-baseline sm:gap-3">
            <div className="flex items-baseline gap-2">
              <span className="font-semibold text-3xl">{price}</span>
              <span className="text-muted-foreground text-sm">{period}</span>
            </div>
            {showSavings && savingsAmount > 0 && (
              <Badge className="shrink-0 text-xs" variant="secondary">
                Save {formatPrice(savingsAmount, currency)}/year
              </Badge>
            )}
          </div>

          {features.length > 0 && (
            <>
              <Separator />
              <div className="flex flex-col gap-3">
                <h4 className="font-medium text-sm">What you&apos;ll get:</h4>
                <div className="flex flex-col gap-2">
                  {features.map((feature, idx) => (
                    <div className="flex items-start gap-3" key={idx}>
                      {feature.icon ? (
                        <feature.icon className="size-4 shrink-0 text-primary" />
                      ) : (
                        <Check className="size-4 shrink-0 text-primary" />
                      )}
                      <div className="flex min-w-0 flex-1 flex-col gap-2">
                        <span className="wrap-break-word text-sm">
                          {feature.name}
                        </span>
                        {feature.description && (
                          <span className="wrap-break-word text-muted-foreground text-xs">
                            {feature.description}
                          </span>
                        )}
                      </div>
                    </div>
                  ))}
                </div>
              </div>
            </>
          )}

          <Separator />

          <div className="flex flex-col gap-2">
            {onUpgrade && (
              <Button
                aria-busy={isLoading}
                className="w-full"
                data-loading={isLoading}
                onClick={handleUpgrade}
                type="button"
              >
                {isLoading ? (
                  <>
                    <Loader2 className="size-4 animate-spin" />
                    Upgrading…
                  </>
                ) : (
                  <>
                    <TrendingUp className="size-4" />
                    Upgrade now
                  </>
                )}
              </Button>
            )}
            {onLearnMore && (
              <Button
                className="w-full"
                onClick={onLearnMore}
                type="button"
                variant="outline"
              >
                Learn more
              </Button>
            )}
          </div>
        </div>
      </CardContent>
    </Card>
  );
}

demo.tsx
import BillingUpgradePrompt from "@/components/ui/billing-upgrade-prompt";
import { BarChart3, ShieldCheck, Users, Zap } from "lucide-react";

export default function Default() {
  return (
    <div className="flex min-h-[520px] w-full items-center justify-center bg-background p-6 text-foreground">
      <div className="w-full max-w-md">
        <BillingUpgradePrompt
          currentPlan={{ id: "free", name: "Free" }}
          recommendedPlan={{
            id: "pro",
            name: "Pro",
            price: 20,
            billingPeriod: "monthly",
          }}
          reason="usage_limit"
          limitedTime
          showSavings
          savingsAmount={40}
          features={[
            {
              name: "Unlimited projects",
              description: "Create as many projects as you need",
              icon: Zap,
            },
            {
              name: "Advanced analytics",
              description: "Track performance with detailed insights",
              icon: BarChart3,
            },
            {
              name: "Team collaboration",
              description: "Invite up to 10 team members",
              icon: Users,
            },
            {
              name: "Priority support",
              description: "Get help within 24 hours",
              icon: ShieldCheck,
            },
          ]}
          onUpgrade={() => {}}
          onDismiss={() => {}}
          onLearnMore={() => {}}
        />
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge button card separator
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
