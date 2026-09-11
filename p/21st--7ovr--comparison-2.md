<!-- Us vs Them Comparison · @7ovr · https://21st.dev/@7ovr/components/comparison-2
     license: no-license · category: cta
     A two-column "Us vs. Them" comparison section with side-by-side cards — one listing product advantages with check rows and a primary CTA, the other listing competitor pain points with cross rows. -->

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
components/ui/comparison-block.tsx
import { Badge } from "@/components/ui/badge"
import { Button } from "@/components/ui/button"
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from "@/components/ui/card"
import { Separator } from "@/components/ui/separator"
import { IconPlaceholder } from "@/components/icons/icon-placeholder"

const acmePoints = [
  "Onboarding completed in under 10 minutes",
  "Real-time sync across all devices and team members",
  "Granular role-based access control included",
  "99.99 % uptime SLA with status page",
  "Dedicated Slack channel for Pro customers",
  "SOC 2 Type II certified infrastructure",
  "One-click data export in any format",
  "No per-seat fees, unlimited collaborators",
]

const othersPoints = [
  "Complex setup requires engineering resources",
  "Sync delays of up to 15 minutes on free tiers",
  "Permissions locked behind enterprise plan",
  "SLA only available on custom contracts",
  "Email-only support with 48 h response time",
  "Compliance docs behind sales call",
  "Data export limited to CSV on lower plans",
  "Per-seat pricing adds up fast at scale",
]

function CheckRow({ text }: { text: string }) {
  return (
    <li className="flex items-start gap-3">
      <span className="mt-0.5 flex size-4 shrink-0 items-center justify-center rounded-md bg-primary">
        <IconPlaceholder
          lucide="Check"
          tabler="IconCheck"
          hugeicons="Tick02Icon"
          phosphor="Check"
          remixicon="RiCheckLine"
          className="size-3 text-primary-foreground"
          aria-hidden
        />
      </span>
      <span className="text-sm text-foreground">{text}</span>
    </li>
  )
}

function CrossRow({ text }: { text: string }) {
  return (
    <li className="flex items-start gap-3">
      <span className="mt-0.5 flex size-4 shrink-0 items-center justify-center rounded-md bg-muted">
        <IconPlaceholder
          lucide="X"
          tabler="IconX"
          hugeicons="Cancel01Icon"
          phosphor="X"
          remixicon="RiCloseLine"
          className="size-3 text-muted-foreground"
          aria-hidden
        />
      </span>
      <span className="text-sm text-muted-foreground">{text}</span>
    </li>
  )
}

export default function ComparisonBlock() {
  return (
    <section className="flex w-full items-center justify-center bg-background px-6 py-12 text-foreground">
      <div className="mx-auto w-full max-w-4xl">
        <div className="mb-10 text-center">
          <Badge variant="outline" className="mb-4">
            <IconPlaceholder
              lucide="ShieldCheck"
              tabler="IconShieldCheck"
              hugeicons="Shield01Icon"
              phosphor="ShieldCheck"
              remixicon="RiShieldCheckLine"
              data-icon="inline-start"
            />
            Why Acme
          </Badge>
          <h2 className="font-heading text-3xl font-bold tracking-tight sm:text-4xl">
            Built differently, on purpose
          </h2>
          <p className="mt-3 text-sm text-muted-foreground">
            We obsessed over the details others skip. Here is what that means
            for your team every single day.
          </p>
        </div>

        <div className="grid gap-4 sm:grid-cols-2">
          <Card className="border-primary/30 ring-primary/20">
            <CardHeader>
              <div className="flex items-center gap-2">
                <CardTitle className="text-base font-semibold">Acme</CardTitle>
                <Badge variant="default">Recommended</Badge>
              </div>
              <CardDescription>
                Everything your team needs, without the enterprise runaround.
              </CardDescription>
            </CardHeader>

            <Separator />

            <CardContent className="pt-4">
              <ul className="flex flex-col gap-3">
                {acmePoints.map((point) => (
                  <CheckRow key={point} text={point} />
                ))}
              </ul>
            </CardContent>

            <CardFooter className="border-t">
              <Button
                nativeButton={false}
                className="w-full"
                render={<a href="#" />}
              >
                Start For Free
                <IconPlaceholder
                  lucide="ArrowRight"
                  tabler="IconArrowRight"
                  hugeicons="ArrowRightIcon"
                  phosphor="ArrowRight"
                  remixicon="RiArrowRightLine"
                  data-icon="inline-end"
                />
              </Button>
            </CardFooter>
          </Card>

          <Card className="bg-muted/30">
            <CardHeader>
              <CardTitle className="text-base font-semibold text-muted-foreground">
                The others
              </CardTitle>
              <CardDescription>
                Common friction points teams encounter with legacy platforms.
              </CardDescription>
            </CardHeader>

            <Separator />

            <CardContent className="pt-4">
              <ul className="flex flex-col gap-3">
                {othersPoints.map((point) => (
                  <CrossRow key={point} text={point} />
                ))}
              </ul>
            </CardContent>

            <CardFooter className="border-t">
              <Button
                variant="secondary"
                nativeButton={false}
                className="w-full"
                render={<a href="#" />}
              >
                See Why Teams Switch
                <IconPlaceholder
                  lucide="ArrowRight"
                  tabler="IconArrowRight"
                  hugeicons="ArrowRightIcon"
                  phosphor="ArrowRight"
                  remixicon="RiArrowRightLine"
                  data-icon="inline-end"
                />
              </Button>
            </CardFooter>
          </Card>
        </div>
      </div>
    </section>
  )
}

demo.tsx
import ComparisonBlock from "@/components/ui/comparison-2";

export default function Default() {
  return <ComparisonBlock />;
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @remixicon/react
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
