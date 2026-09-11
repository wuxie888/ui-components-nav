<!-- Feature Showcase · @ruixen.ui · https://21st.dev/@ruixen.ui/components/feature-showcase
     license: unspecified · category: cta
     The FeatureShowcase component is a responsive, theme-aware section built entirely with shadcn/ui primitives that blends storytelling and interaction. It pairs an informative left panel—featuring a headline, description, subtle stats, step-by-step accordion, and clear call-to-action buttons—with a dynamic right panel that displays tab-based media. Each tab reveals a full-bleed image that automatically fills the container, making the layout visually balanced and immersive. Ideal for product introductions or feature highlights, this component lets designers combine concise narrative content with interactive visuals to engage users across light and dark themes. -->

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
components/ui/split-feature-showcase.tsx
"use client";

import * as React from "react";
import { motion } from "motion/react";
import { cn } from "@/lib/utils";

/* ═══════════════════════════════════════════════════════════
   Split Feature Showcase — Rauno-minimal.

   Two cells in a sharp-cornered container, no border chrome:
     Left:  Conversation thread (sender + reply bubbles, online pulse)
     Right: Bare text list — even hierarchy, subtle stagger animation

   Dependencies: motion.
   ═══════════════════════════════════════════════════════════ */

const S = { type: "spring" as const, stiffness: 300, damping: 28 };
const S_SNAP = { type: "spring" as const, stiffness: 440, damping: 26 };
const S_SOFT = { type: "spring" as const, stiffness: 260, damping: 24 };
const S_MICRO = { type: "spring" as const, stiffness: 500, damping: 30 };

export interface SplitFeatureShowcaseProps {
  leftTitle?: string;
  leftDescription?: string;
  rightTitle?: string;
  rightDescription?: string;
  className?: string;
}

const WORKFLOW_ITEMS = [
  "Alerts",
  "Lead routing",
  "Re-engage cold leads",
  "New Deal email campaign flow",
  "Lead form submissions",
  "Health scoring",
  "Upsell",
];

export function SplitFeatureShowcase({
  leftTitle = "Conversations that convert.",
  leftDescription = "Collaborate in real time with threaded messages, presence indicators, and instant reactions every exchange captured in context.",
  rightTitle = "Automate your entire pipeline.",
  rightDescription = "From lead capture to customer success, every workflow runs on autopilot, so your team can focus on closing deals.",
  className,
}: SplitFeatureShowcaseProps) {
  return (
    <section className={cn("", className)}>
      <div className="mx-auto w-full max-w-5xl px-6">
        <div className="relative grid overflow-hidden bg-card/50 divide-y divide-border/40 md:grid-cols-2 md:divide-x md:divide-y-0">
          {/* ── Left cell ──────────────────────────────────── */}
          <motion.div
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ ...S, delay: 0 }}
            className="row-span-2 grid grid-rows-subgrid gap-8 p-8"
          >
            <div className="mx-auto max-w-xs self-center">
              <div aria-hidden="true" className="space-y-3">
                <motion.div
                  initial={{ opacity: 0, scale: 0.6 }}
                  animate={{ opacity: 1, scale: 1 }}
                  transition={{ ...S_SNAP, delay: 0.2 }}
                  className="flex items-center gap-2"
                >
                  <div className="relative">
                    <img
                      src="/avatar-images/avatar-01.jpg"
                      alt=""
                      className="size-7 shrink-0 rounded-full object-cover ring-2 ring-background"
                    />
                    <span className="absolute -bottom-0.5 -right-0.5 block size-2 rounded-full bg-foreground/40 ring-2 ring-background" />
                  </div>
                  <span className="text-[13px] font-medium tracking-tight">
                    Irung
                  </span>
                  <span className="text-muted-foreground/60 text-[10px]">
                    2m ago
                  </span>
                </motion.div>

                <motion.div
                  initial={{ opacity: 0, x: -16 }}
                  animate={{ opacity: 1, x: 0 }}
                  transition={{ ...S_SOFT, delay: 0.35 }}
                  whileHover={{ scale: 1.02, y: -1, transition: S_MICRO }}
                  className="relative ml-9 w-fit"
                >
                  <div className="cursor-default rounded-2xl rounded-tl-[4px] bg-muted px-3.5 py-2 text-[13px] leading-relaxed shadow-sm shadow-black/5 ring-1 ring-border/60">
                    Hey{" "}
                    <span className="font-medium text-foreground">
                      @bernard
                    </span>
                    , I&apos;ve updated the dashboard metrics.
                  </div>
                  <motion.div
                    initial={{ opacity: 0, scale: 0 }}
                    animate={{ opacity: 1, scale: 1 }}
                    transition={{ ...S_SNAP, delay: 0.75 }}
                    className="absolute -bottom-2 right-3 flex h-[22px] items-center rounded-full bg-card px-1.5 text-[10px] shadow-sm shadow-black/5 ring-1 ring-border/40"
                  >
                    🔥
                  </motion.div>
                </motion.div>

                <div className="flex items-end justify-end gap-2 pt-1">
                  <motion.div
                    initial={{ opacity: 0, x: 16 }}
                    animate={{ opacity: 1, x: 0 }}
                    transition={{ ...S_SOFT, delay: 0.55 }}
                    whileHover={{ scale: 1.02, y: -1, transition: S_MICRO }}
                    className="w-fit cursor-default rounded-2xl rounded-tr-[4px] bg-foreground px-3.5 py-2 text-[13px] leading-relaxed text-background shadow-sm shadow-black/10"
                  >
                    The conversion rate looks great
                  </motion.div>
                  <motion.div
                    initial={{ opacity: 0, scale: 0.6 }}
                    animate={{ opacity: 1, scale: 1 }}
                    transition={{ ...S_SNAP, delay: 0.5 }}
                    className="shrink-0"
                  >
                    <img
                      src="/avatar-images/avatar-02.jpg"
                      alt=""
                      className="size-7 rounded-full object-cover ring-2 ring-background"
                    />
                  </motion.div>
                </div>

                <motion.div
                  initial={{ opacity: 0 }}
                  animate={{ opacity: 1 }}
                  transition={{ duration: 0.3, delay: 0.7 }}
                  className="flex items-center justify-end gap-1 pr-9"
                >
                  <svg
                    viewBox="0 0 24 24"
                    className="size-3 text-foreground/40"
                    fill="none"
                    stroke="currentColor"
                    strokeWidth={2.5}
                    strokeLinecap="round"
                    strokeLinejoin="round"
                  >
                    <path d="M20 6 9 17l-5-5" />
                  </svg>
                  <span className="text-[10px] text-muted-foreground/50">
                    Read
                  </span>
                </motion.div>

                <motion.div
                  initial={{ opacity: 0, y: 6 }}
                  animate={{ opacity: 1, y: 0 }}
                  transition={{ ...S_SOFT, delay: 0.9 }}
                  className="ml-9 w-fit"
                >
                  <div className="flex items-center gap-[3px] rounded-2xl rounded-tl-[4px] bg-muted px-3 py-2.5 shadow-sm shadow-black/5 ring-1 ring-border/60">
                    <span
                      className="block size-[5px] rounded-full bg-foreground/25"
                      style={{
                        animation: "bentoTypingDot 1.4s ease-in-out infinite",
                      }}
                    />
                    <span
                      className="block size-[5px] rounded-full bg-foreground/25"
                      style={{
                        animation:
                          "bentoTypingDot 1.4s ease-in-out 0.15s infinite",
                      }}
                    />
                    <span
                      className="block size-[5px] rounded-full bg-foreground/25"
                      style={{
                        animation:
                          "bentoTypingDot 1.4s ease-in-out 0.3s infinite",
                      }}
                    />
                  </div>
                </motion.div>
              </div>
            </div>

            <motion.div
              initial={{ opacity: 0, y: 10 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ ...S, delay: 0.5 }}
              className="mx-auto max-w-sm text-center"
            >
              <p className="text-base leading-relaxed text-balance">
                <strong className="font-semibold text-foreground">
                  {leftTitle}
                </strong>{" "}
                <span className="text-muted-foreground">{leftDescription}</span>
              </p>
            </motion.div>
          </motion.div>

          {/* ── Right cell: Bare text, even hierarchy ──────── */}
          <motion.div
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ ...S, delay: 0.12 }}
            className="row-span-2 grid grid-rows-subgrid gap-8 p-8"
          >
            <div className="flex items-center justify-center self-center">
              <div
                aria-hidden="true"
                className="flex flex-col items-center gap-3"
              >
                {WORKFLOW_ITEMS.map((label, i) => {
                  const center = Math.floor(WORKFLOW_ITEMS.length / 2);
                  const dist = Math.abs(i - center);

                  return (
                    <motion.span
                      key={label}
                      initial={{ opacity: 0, y: 6, filter: "blur(4px)" }}
                      animate={{ opacity: 1, y: 0, filter: "blur(0px)" }}
                      transition={{
                        ...S_SOFT,
                        delay: 0.2 + i * 0.07,
                      }}
                      className={cn(
                        "block text-[15px] tracking-tight transition-opacity duration-300",
                        dist === 0
                          ? "font-medium text-foreground"
                          : dist === 1
                            ? "text-foreground/70"
                            : dist === 2
                              ? "text-foreground/45"
                              : "text-foreground/25",
                      )}
                    >
                      {label}
                    </motion.span>
                  );
                })}
              </div>
            </div>

            <motion.div
              initial={{ opacity: 0, y: 10 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ ...S, delay: 0.5 }}
              className="relative z-10 mx-auto max-w-sm text-center"
            >
              <p className="text-base leading-relaxed text-balance">
                <strong className="font-semibold text-foreground">
                  {rightTitle}
                </strong>{" "}
                <span className="text-muted-foreground">
                  {rightDescription}
                </span>
              </p>
            </motion.div>
          </motion.div>
        </div>
      </div>

      <style
        dangerouslySetInnerHTML={{
          __html: `@keyframes bentoTypingDot{0%,100%{transform:translateY(0);opacity:.4}50%{transform:translateY(-3px);opacity:1}}`,
        }}
      />
    </section>
  );
}

demo.tsx
// app/feature-demo/page.tsx
import { FeatureShowcase, type TabMedia } from "@/components/ui/feature-showcase";

export default function Page() {
  const tabs: TabMedia[] = [
    {
      value: "apparel",
      label: "Apparel",
      src: "https://cdn.21st.dev/assets/mirror/af/aff38f3726795680a4a5bad383106d742e54aca8728cd221901f766aff3650f4.png",
      alt: "Apparel mockup",
    },
    {
      value: "screen",
      label: "Screen",
      src: "https://cdn.21st.dev/assets/mirror/e1/e10c7175dec8e12ea3daa7204fd1cfba161d61aa3fda5387a022e7e258e3171f.png",
      alt: "Website template on screen",
    },
    {
      value: "abstract",
      label: "Abstract",
      src: "https://cdn.21st.dev/assets/mirror/61/6138ba1bccd2ae7cc89a81c656e36febe6fb4a3166232accf821861e8f8d86a1.jpg",
      alt: "Abstract background",
    },
  ];

  return (
    <FeatureShowcase
      eyebrow="Experience"
      title="Design that adapts to your vibe"
      description="Turn your ideas into visuals that match your style — whether it’s product mockups, website screens, or abstract art. Instantly switch views and find what clicks with your brand."
      stats={["3 styles", "Instant preview", "Creative-ready"]}
      steps={[
        {
          id: "step-1",
          title: "Upload your concept",
          text:
            "Start with any visual — a logo, sketch, or product photo. We’ll analyze it to set your creative tone.",
        },
        {
          id: "step-2",
          title: "Preview across styles",
          text:
            "Toggle between Apparel, Screen, and Abstract to visualize how your idea fits different mediums.",
        },
        {
          id: "step-3",
          title: "Refine and export",
          text:
            "Fine-tune the details, download polished assets, and share them directly with your team or clients.",
        },
      ]}
      tabs={tabs}
      defaultTab="screen"
      panelMinHeight={720}
    />
  );
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add accordion badge button card tabs
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
