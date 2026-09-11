<!-- Scroll Reveal · @cnippet-dev · https://21st.dev/@cnippet-dev/components/scroll-reveal
     license: MIT · category: features
     Elements animate into view when they enter the viewport. Supports any Motion variant, configurable thresholds, and a once-only mode. -->

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
components/ui/m-scroll-reveal-1.tsx
import { ScrollReveal } from "@/registry/default/motion/scroll-reveal";

const cards = [
  {
    description: "Composable components that snap together cleanly.",
    icon: "◈",
    title: "Composable",
  },
  {
    description: "Every animation respects prefers-reduced-motion.",
    icon: "⬡",
    title: "Accessible",
  },
  {
    description: "Zero runtime overhead — pure CSS where possible.",
    icon: "◎",
    title: "Performant",
  },
];

export default function ScrollRevealCards() {
  return (
    <div className="flex min-h-50 items-center justify-center px-6">
      <div className="grid w-full max-w-2xl grid-cols-1 gap-4 sm:grid-cols-3">
        {cards.map((card, i) => (
          <ScrollReveal
            key={card.title}
            transition={{ delay: i * 0.12, duration: 0.5, ease: "easeOut" }}
            variants={{
              hidden: { opacity: 0, y: 24 },
              visible: { opacity: 1, y: 0 },
            }}
            viewOptions={{ amount: 0.3 }}
          >
            <div className="rounded-xl border border-border bg-card p-5">
              <p className="mb-2 text-2xl">{card.icon}</p>
              <h3 className="font-semibold text-foreground">{card.title}</h3>
              <p className="mt-1 text-muted-foreground text-sm">
                {card.description}
              </p>
            </div>
          </ScrollReveal>
        ))}
      </div>
    </div>
  );
}

demo.tsx
import { ScrollReveal } from "@/components/ui/scroll-reveal";

const cards = [
  {
    description: "Composable components that snap together cleanly.",
    icon: "◈",
    title: "Composable",
  },
  {
    description: "Every animation respects prefers-reduced-motion.",
    icon: "⬡",
    title: "Accessible",
  },
  {
    description: "Zero runtime overhead — pure CSS where possible.",
    icon: "◎",
    title: "Performant",
  },
];

export default function ScrollRevealCards() {
  return (
    <div className="flex min-h-50 items-center justify-center px-6">
      <div className="grid w-full max-w-2xl grid-cols-1 gap-4 sm:grid-cols-3">
        {cards.map((card, i) => (
          <ScrollReveal
            key={card.title}
            transition={{ delay: i * 0.12, duration: 0.5, ease: "easeOut" }}
            variants={{
              hidden: { opacity: 0, y: 24 },
              visible: { opacity: 1, y: 0 },
            }}
            viewOptions={{ amount: 0.3 }}
          >
            <div className="rounded-xl border border-border bg-card p-5">
              <p className="mb-2 text-2xl">{card.icon}</p>
              <h3 className="font-semibold text-foreground">{card.title}</h3>
              <p className="mt-1 text-muted-foreground text-sm">
                {card.description}
              </p>
            </div>
          </ScrollReveal>
        ))}
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add scroll-reveal
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
