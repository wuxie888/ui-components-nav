<!-- Theme Switcher · @ncdai · https://21st.dev/@ncdai/components/theme-switcher-1
     license: unspecified · category: toggle
     Here is Theme Switcher component -->

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
components/theme-switcher/theme-switcher.tsx
"use client"

import type { JSX } from "react"
import { useSyncExternalStore } from "react"
import { motion } from "motion/react"
import { useTheme } from "next-themes"

import { IconPlaceholder } from "@/registry/icons/icon-placeholder"

function ThemeOption({
  icon,
  value,
  isActive,
  onClick,
}: {
  icon: JSX.Element
  value: string
  isActive?: boolean
  onClick: (value: string) => void
}) {
  return (
    <button
      data-active={isActive}
      className="relative flex size-8 items-center justify-center rounded-full text-muted-foreground transition-[color] hover:text-foreground data-[active=true]:text-foreground [&_svg]:size-4"
      role="radio"
      aria-checked={isActive}
      aria-label={`Switch to ${value} theme`}
      onClick={() => onClick(value)}
    >
      {icon}

      {isActive && (
        <motion.span
          layoutId="theme-option"
          transition={{ type: "spring", bounce: 0.3, duration: 0.6 }}
          className="absolute inset-0 rounded-full border"
        />
      )}
    </button>
  )
}

const THEME_OPTIONS = [
  {
    icon: (
      <IconPlaceholder
        lucide="MonitorIcon"
        tabler="IconDeviceDesktop"
        hugeicons="ComputerIcon"
        phosphor="DesktopIcon"
        remixicon="RiComputerLine"
      />
    ),
    value: "system",
  },
  {
    icon: (
      <IconPlaceholder
        lucide="SunIcon"
        tabler="IconSun"
        hugeicons="Sun03Icon"
        phosphor="SunIcon"
        remixicon="RiSunLine"
      />
    ),
    value: "light",
  },
  {
    icon: (
      <IconPlaceholder
        lucide="MoonIcon"
        tabler="IconMoon"
        hugeicons="Moon02Icon"
        phosphor="MoonIcon"
        remixicon="RiMoonLine"
      />
    ),
    value: "dark",
  },
]

function ThemeSwitcher() {
  const { theme, setTheme } = useTheme()

  const isMounted = useSyncExternalStore(
    () => () => {},
    () => true,
    () => false
  )

  if (!isMounted) {
    return <div className="flex h-8 w-24" />
  }

  return (
    <motion.div
      key={String(isMounted)}
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
      transition={{ duration: 0.3 }}
      className="inline-flex items-center overflow-clip rounded-full bg-background inset-ring-1 inset-ring-border"
      role="radiogroup"
    >
      {THEME_OPTIONS.map((option) => (
        <ThemeOption
          key={option.value}
          icon={option.icon}
          value={option.value}
          isActive={theme === option.value}
          onClick={setTheme}
        />
      ))}
    </motion.div>
  )
}

export { ThemeSwitcher }

demo.tsx
import { ThemeSwitcher } from "@/components/ui/theme-switcher-1";

export default function ThemeSwitcherDemo() {
  return <ThemeSwitcher />;
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion next-themes
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
