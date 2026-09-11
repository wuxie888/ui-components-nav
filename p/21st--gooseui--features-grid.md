<!-- Features Grid · @gooseui · https://21st.dev/@gooseui/components/features-grid
     license: MIT · category: features
     A responsive grid of product feature cards with icons, titles and descriptions for landing page sections. -->

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
components/ui/features-grid.tsx
import {
  Code,
  LayoutGrid,
  Palette,
  Sparkles,
  Terminal,
  Zap,
} from "lucide-react"
import { cn } from "@/lib/utils"

const demoFeatures = [
  {
    icon: Sparkles,
    title: "Animations & Effects",
    description:
      "Border Beam, text effects and other animations for attractive interfaces",
  },
  {
    icon: Zap,
    title: "Instant Installation",
    description:
      "One CLI command — and the component is in your project. No package dependencies",
  },
  {
    icon: Code,
    title: "Full Control",
    description:
      "Code is copied to your project. Modify anything without restrictions",
  },
  {
    icon: Palette,
    title: "Flexible Styling",
    description:
      "Tailwind CSS and CSS variables for easy customization to match your brand",
  },
  {
    icon: LayoutGrid,
    title: "Ready-made Blocks",
    description:
      "Sections for landing pages, forms, cards — assemble pages like building blocks",
  },
  {
    icon: Terminal,
    title: "shadcn CLI",
    description: "Full compatibility with shadcn CLI. Use familiar commands",
  },
]

interface Feature {
  icon: React.ComponentType<{ className?: string }>
  title: string
  description: string
}

interface FeaturesGridProps {
  title?: string
  subtitle?: string
  features?: Feature[]
  className?: string
}

function FeatureCard({ icon: Icon, title, description }: Feature) {
  return (
    <div className="rounded-xl border bg-card p-6 transition-colors hover:bg-muted/50">
      <div className="mb-4 flex size-12 items-center justify-center rounded-lg bg-primary/10">
        <Icon className="size-6 text-primary" />
      </div>
      <h3 className="mb-2 font-semibold tracking-tight">{title}</h3>
      <p className="text-sm text-muted-foreground leading-relaxed">
        {description}
      </p>
    </div>
  )
}

export function FeaturesGrid({
  title = "Everything for Modern Development",
  subtitle = "Components that save time and help you build quality products",
  features = demoFeatures,
  className,
}: FeaturesGridProps) {
  return (
    <section className={cn("py-20 px-6", className)}>
      <div className="mx-auto max-w-6xl">
        <div className="mb-12 text-center">
          <h2 className="text-3xl font-bold tracking-tight md:text-4xl">
            {title}
          </h2>
          <p className="mx-auto mt-4 max-w-2xl text-lg text-muted-foreground">
            {subtitle}
          </p>
        </div>

        <div className="grid gap-6 sm:grid-cols-2 lg:grid-cols-3">
          {features.map((feature) => (
            <FeatureCard key={feature.title} {...feature} />
          ))}
        </div>
      </div>
    </section>
  )
}

export type { Feature, FeaturesGridProps }

demo.tsx
import FeaturesGrid from "@/components/ui/features-grid";

export default function Default() {
  return (
    <div className="min-h-screen w-full bg-background text-foreground">
      <FeaturesGrid />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
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
