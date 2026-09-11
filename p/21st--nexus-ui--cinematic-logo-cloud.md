<!-- Cinematic Logo Cloud · @nexus-ui · https://21st.dev/@nexus-ui/components/cinematic-logo-cloud
     license: no-license · category: clients
     A logo cloud that reveals brand logos with a staggered blur-fade entrance, with grid and marquee layout variants. -->

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
components/ui/cinematic-logo-cloud.tsx
"use client";

import * as React from "react";
import { motion } from "framer-motion";
import { cn } from "@/lib/utils";
import { Marquee } from "@/components/ui/marquee";

export type LogoCloudClient = {
  name: string;
  slug?: string;
  text?: boolean;
  className?: string;
  nameClassName?: string;
  invertDark?: boolean;
};

export interface CinematicLogoCloudProps {
  clients: LogoCloudClient[];
  variant?: "grid" | "marquee" | "marquee-named";
  className?: string;
  eyebrow?: string;
  description?: string;
}

export function CinematicLogoCloud({
  clients,
  variant = "grid",
  className,
  eyebrow = "Trusted by teams building the future of AI.",
  description = "From prototype to production, autonomously.",
}: CinematicLogoCloudProps) {
  const renderClient = (client: LogoCloudClient, size: "lg" | "sm" = "lg") => {
    if (client.text) {
      return (
        <span
          className={cn(
            size === "lg"
              ? "text-xl font-bold text-zinc-900 dark:text-white"
              : "text-sm font-semibold text-zinc-700 dark:text-zinc-300",
            client.className,
          )}
        >
          {client.name}
        </span>
      );
    }
    return (
      <img
        src={`https://cdn.simpleicons.org/${client.slug}`}
        alt={client.name}
        className={cn(size === "lg" ? "h-6 w-auto" : "h-5 w-auto", client.invertDark && "dark:invert")}
        loading="lazy"
      />
    );
  };

  if (variant === "marquee") {
    return (
      <div className={cn("w-full bg-zinc-50/50 py-12 md:py-16 dark:bg-zinc-950", className)}>
        <p className="mb-8 text-center text-xs font-semibold uppercase tracking-widest text-zinc-400">
          {eyebrow}
        </p>
        <Marquee speed={35}>
          {clients.map((brand) => (
            <div
              key={brand.name}
              className="flex shrink-0 items-center justify-center rounded-xl border border-zinc-200/80 bg-white px-5 py-3 shadow-sm dark:border-white/8 dark:bg-zinc-900"
            >
              {renderClient(brand, "sm")}
            </div>
          ))}
        </Marquee>
      </div>
    );
  }

  if (variant === "marquee-named") {
    return (
      <div className={cn("w-full bg-zinc-50/50 py-12 md:py-16 dark:bg-zinc-950", className)}>
        <p className="mb-8 text-center text-xs font-semibold uppercase tracking-widest text-zinc-400">
          {eyebrow}
        </p>
        <Marquee speed={40}>
          {clients.map((brand) => (
            <div
              key={brand.name}
              className="flex shrink-0 items-center gap-2.5 rounded-xl border border-zinc-200/80 bg-white px-4 py-2.5 shadow-sm dark:border-white/8 dark:bg-zinc-900"
            >
              {brand.slug && (
                <img
                  src={`https://cdn.simpleicons.org/${brand.slug}`}
                  alt={brand.name}
                  className={cn("h-4 w-4 shrink-0", brand.invertDark && "dark:invert")}
                  loading="lazy"
                />
              )}
              <span
                className={cn(
                  "text-sm text-zinc-800 dark:text-zinc-200",
                  brand.nameClassName,
                )}
              >
                {brand.name}
              </span>
            </div>
          ))}
        </Marquee>
      </div>
    );
  }

  return (
    <div className={cn("w-full bg-zinc-50/50 py-12 md:py-16 dark:bg-zinc-950", className)}>
      <div className="mx-auto max-w-7xl px-4 text-center sm:px-6 lg:px-8">
        <p className="text-xs font-semibold uppercase tracking-widest text-zinc-400">
          {eyebrow}
        </p>
        {description && <p className="mt-1 text-xs text-zinc-500">{description}</p>}

        <motion.div
          initial="hidden"
          whileInView="visible"
          viewport={{ once: true, margin: "-50px" }}
          variants={{
            visible: { transition: { staggerChildren: 0.1 } },
            hidden: {},
          }}
          className="mt-8 flex flex-wrap items-center justify-center gap-x-8 gap-y-6 transition-all"
        >
          {clients.map((brand) => (
            <motion.div
              key={brand.name}
              variants={{
                hidden: { opacity: 0, y: 20, filter: "blur(12px)" },
                visible: {
                  opacity: 1,
                  y: 0,
                  filter: "blur(0px)",
                  transition: { duration: 1.2, ease: [0.16, 1, 0.3, 1] },
                },
              }}
              className="flex min-w-25 items-center justify-center px-2 sm:min-w-30"
            >
              {renderClient(brand)}
            </motion.div>
          ))}
        </motion.div>
      </div>
    </div>
  );
}

components/ui/marquee.tsx
"use client";

import * as React from "react";
import { cn } from "@/lib/utils";

export interface MarqueeProps {
  children: React.ReactNode;
  className?: string;
  pauseOnHover?: boolean;
  reverse?: boolean;
  speed?: number;
}

export function Marquee({ children, className, pauseOnHover = false, reverse = false, speed = 40 }: MarqueeProps) {
  return (
    <div
      className={cn(
        "flex overflow-hidden",
        "[mask-image:linear-gradient(to_right,transparent,black_10%,black_90%,transparent)]",
        className,
      )}
    >
      <div
        className="flex gap-4"
        style={{
          animation: `marquee-scroll ${speed}s linear infinite`,
          animationDirection: reverse ? "reverse" : "normal",
        }}
        onMouseEnter={() => { if (pauseOnHover) { const el = document.querySelector<HTMLElement>("[data-marquee]"); if (el) el.style.animationPlayState = "paused"; } }}
        onMouseLeave={() => { if (pauseOnHover) { const el = document.querySelector<HTMLElement>("[data-marquee]"); if (el) el.style.animationPlayState = "running"; } }}
        data-marquee
      >
        <div className="flex shrink-0 gap-4">{children}</div>
        <div className="flex shrink-0 gap-4" aria-hidden="true">{children}</div>
      </div>
      <style>{`
        @keyframes marquee-scroll {
          from { transform: translateX(0); }
          to { transform: translateX(-50%); }
        }
      `}</style>
    </div>
  );
}

demo.tsx
import { CinematicLogoCloud } from "@/components/ui/cinematic-logo-cloud";

const clients = [
  { name: "OpenAI", text: true, className: "text-xl font-semibold" },
  {
    name: "hello patient",
    text: true,
    className: "text-lg font-medium lowercase",
  },
  { name: "granola", text: true, className: "text-lg font-medium" },
  { name: "character.ai", text: true, className: "text-lg font-medium" },
  {
    name: "ORACLE",
    text: true,
    className: "text-lg font-bold tracking-wide text-red-600 dark:text-red-500",
  },
  {
    name: "PORTOLA",
    text: true,
    className: "text-lg font-semibold tracking-wide",
  },
  { name: "Accel", text: true, className: "text-xl font-semibold" },
  { name: "Bloomberg", text: true, className: "text-lg font-bold" },
  {
    name: "FORBES",
    text: true,
    className: "text-lg font-bold font-serif tracking-wide",
  },
  { name: "SoftBank", text: true, className: "text-lg font-semibold" },
  { name: "WIRED", text: true, className: "text-lg font-black tracking-tight" },
  { name: "hulu", text: true, className: "text-xl font-bold" },
  { name: "YouTube", text: true, className: "text-lg font-semibold" },
  {
    name: "NETFLIX",
    text: true,
    className: "text-lg font-bold tracking-widest",
  },
];

export default function Default() {
  return (
    <div className="flex min-h-[400px] w-full items-center bg-background text-foreground">
      <CinematicLogoCloud clients={clients} />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add nexus-font utils
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
