<!-- morphic-navbar · Kokonut UI · https://kokonutui.com/docs/navigation/morphic-navbar
     license: MIT · category: navigation-menu
      -->

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
components/kokonutui/morphic-navbar.tsx
"use client";

import clsx from "clsx";
import Link from "next/link";
import { useState } from "react";

interface NavItem {
  name: string;
}

interface MorphicNavbarProps {
  items?: Record<string, NavItem>;
  defaultPath?: string;
  className?: string;
}

const DEFAULT_NAV_ITEMS: Record<string, NavItem> = {
  "/": { name: "home" },
  "/works": { name: "works" },
  "/blog": { name: "blog" },
  "/about": { name: "about" },
};

export function MorphicNavbar({
  items = DEFAULT_NAV_ITEMS,
  defaultPath = "/",
  className,
}: MorphicNavbarProps) {
  const [activePath, setActivePath] = useState(defaultPath);

  const isActiveLink = (path: string) => {
    if (path === "/") {
      return activePath === "/";
    }
    return activePath.startsWith(path);
  };

  return (
    <nav className={clsx("mx-auto max-w-4xl px-4 py-2", className)}>
      <div className="flex items-center justify-center">
        <div className="glass flex items-center justify-between overflow-hidden rounded-xl">
          {Object.entries(items).map(([path, { name }], index, array) => {
            const isActive = isActiveLink(path);
            const isFirst = index === 0;
            const isLast = index === array.length - 1;
            const prevPath = index > 0 ? array[index - 1][0] : null;
            const nextPath =
              index < array.length - 1 ? array[index + 1][0] : null;

            return (
              <Link
                className={clsx(
                  "flex items-center justify-center bg-black p-1.5 px-4 text-sm text-white transition-all duration-300 dark:bg-white dark:text-black",
                  isActive
                    ? "mx-2 rounded-xl font-semibold text-sm"
                    : clsx(
                        (isActiveLink(prevPath || "") || isFirst) &&
                          "rounded-l-xl",
                        (isActiveLink(nextPath || "") || isLast) &&
                          "rounded-r-xl"
                      )
                )}
                href="#"
                key={path}
                onClick={() => setActivePath(path)}
              >
                {name}
              </Link>
            );
          })}
        </div>
      </div>
    </nav>
  );
}

export default MorphicNavbar;
```

Install NPM dependencies:
```bash
npm install clsx
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
