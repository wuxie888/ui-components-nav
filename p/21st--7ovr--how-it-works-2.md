<!-- How It Works Timeline · @7ovr · https://21st.dev/@7ovr/components/how-it-works-2
     license: MIT · category: timeline
     A vertical timeline that stacks numbered onboarding steps down the page, each with a bordered icon circle, a connecting line, a bold title, and supporting copy. -->

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
components/ui/how-it-works-block.tsx
import { Badge } from "@/components/ui/badge"
import { IconPlaceholder } from "@/components/icons/icon-placeholder"

/** Props a call site may pass through to an icon. */
type IconProps = { className?: string; size?: number | string }

const steps = [
  {
    icon: (p: IconProps) => (
      <IconPlaceholder
        lucide="UserPlus"
        tabler="IconUserPlus"
        hugeicons="UserAddIcon"
        phosphor="UserPlus"
        remixicon="RiUserAddLine"
        {...p}
      />
    ),
    title: "Create your account",
    copy: "Sign up in under two minutes, no credit card required. Your workspace is ready the moment you confirm your email.",
  },
  {
    icon: (p: IconProps) => (
      <IconPlaceholder
        lucide="Settings"
        tabler="IconSettings"
        hugeicons="SettingsIcon"
        phosphor="GearSix"
        remixicon="RiSettings4Line"
        {...p}
      />
    ),
    title: "Configure your workflow",
    copy: "Choose from pre-built templates or define your own pipeline. Acme adapts to how your team already works.",
  },
  {
    icon: (p: IconProps) => (
      <IconPlaceholder
        lucide="Users"
        tabler="IconUsersGroup"
        hugeicons="UserGroupIcon"
        phosphor="UsersThree"
        remixicon="RiTeamLine"
        {...p}
      />
    ),
    title: "Invite your team",
    copy: "Send role-based invites in bulk. Colleagues join with a single click and inherit the right permissions automatically.",
  },
  {
    icon: (p: IconProps) => (
      <IconPlaceholder
        lucide="Rocket"
        tabler="IconRocket"
        hugeicons="RocketIcon"
        phosphor="Rocket"
        remixicon="RiRocketLine"
        {...p}
      />
    ),
    title: "Ship with confidence",
    copy: "Run automated checks, review the audit trail, and deploy, knowing Acme has your back at every stage.",
  },
]

export default function HowItWorksBlock() {
  return (
    <section className="flex w-full items-center justify-center bg-background px-6 py-16 text-foreground">
      <div className="mx-auto w-full max-w-2xl">
        <div className="mb-14">
          <Badge variant="outline" className="mb-4">
            How It Works
          </Badge>
          <h2 className="font-heading text-3xl font-bold tracking-tight sm:text-4xl">
            Up and running in four steps
          </h2>
          <p className="mt-3 text-muted-foreground">
            Acme is designed for momentum. Go from sign-up to full team
            collaboration without a single support ticket.
          </p>
        </div>

        <ol className="flex flex-col">
          {steps.map(({ icon: Icon, title, copy }, index) => {
            const isLast = index === steps.length - 1
            return (
              <li key={title} className="flex gap-6">
                <div className="flex flex-col items-center">
                  <span className="flex size-10 shrink-0 items-center justify-center rounded-lg border border-border bg-muted">
                    <Icon
                      className="size-4 text-foreground"
                      aria-hidden="true"
                    />
                  </span>
                  {!isLast && <span className="mt-1 w-px flex-1 bg-border" />}
                </div>

                <div className={isLast ? "pb-0" : "pb-10"}>
                  <h3 className="font-heading text-base font-semibold">
                    {title}
                  </h3>
                  <p className="mt-1.5 text-sm text-muted-foreground">{copy}</p>
                </div>
              </li>
            )
          })}
        </ol>
      </div>
    </section>
  )
}

demo.tsx
import HowItWorksBlock from "@/components/ui/how-it-works-2";

export default function Demo() {
  return (
    <div className="min-h-screen w-full bg-background text-foreground">
      <HowItWorksBlock />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @base-ui/react @remixicon/react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge
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
