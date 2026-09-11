<!-- Preview Switch Hero · @ruixen.ui · https://21st.dev/@ruixen.ui/components/preview-switch-hero
     license: unspecified · category: hero
     A split hero with an icon-rail that switches the product preview, a lead-capture column (badge, ratings, email form, CTAs, avatars), and a full-width logo strip. Responsive and theme-aware. -->

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
components/ui/preview-switch-hero.tsx
"use client";

import * as React from "react";
import Link from "next/link";
import { ArrowUpRight, Mail, Star } from "lucide-react";
import { cn } from "@/lib/utils";

/* ── types ───────────────────────────────────────────────────── */

export interface PreviewTab {
  id: string;
  /** Text label shown in the switcher rail. */
  label: string;
  /** Panel shown when this tab is active. All panels should share one size. */
  media: React.ReactNode;
}

export interface HeroRating {
  source: string;
  score: string;
  /** Defaults to a filled star. */
  icon?: React.ReactNode;
}

export interface HeroLogo {
  name: string;
  /** Optional custom mark; falls back to the name as a text wordmark. */
  logo?: React.ReactNode;
}

export interface HeroAvatar {
  initials?: string;
  src?: string;
}

export interface Cta {
  label: string;
  href?: string;
}

export interface PreviewSwitchHeroProps {
  /** Small pill above the title, e.g. `{ tag: "NEW", label: "…" }`. */
  badge?: { tag?: string; label: React.ReactNode };
  title: React.ReactNode;
  description?: React.ReactNode;
  ratings?: HeroRating[];
  /**
   * Show the email-capture field above the CTAs. When `false`, the CTAs render
   * on their own as plain actions (no form/input). Default `true`.
   */
  showEmail?: boolean;
  /** Label above the email field. */
  emailLabel?: React.ReactNode;
  emailPlaceholder?: string;
  /** Called with the email on submit. */
  onSubmit?: (email: string) => void;
  /** Submits the form when no `href`; otherwise renders as a link. */
  primaryCta?: Cta;
  secondaryCta?: Cta;
  avatars?: HeroAvatar[];
  socialProof?: React.ReactNode;
  /** Text tabs that switch the preview. */
  tabs: PreviewTab[];
  /** Logo strip rendered full-width below the split. */
  logos?: HeroLogo[];
  className?: string;
}

/* ── pieces ──────────────────────────────────────────────────── */

function TabRail({
  tabs,
  active,
  onSelect,
}: {
  tabs: PreviewTab[];
  active: number;
  onSelect: (i: number) => void;
}) {
  return (
    <div
      role="tablist"
      aria-label="Preview switcher"
      className="flex shrink-0 gap-2 overflow-x-auto [scrollbar-width:none] md:flex-col md:overflow-visible [&::-webkit-scrollbar]:hidden"
    >
      {tabs.map((t, i) => {
        const isActive = i === active;
        return (
          <button
            key={t.id}
            type="button"
            role="tab"
            aria-selected={isActive}
            onClick={() => onSelect(i)}
            className={cn(
              "whitespace-nowrap rounded-xl px-4 py-2.5 text-left text-sm transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 focus-visible:ring-offset-background",
              isActive
                ? "bg-muted font-semibold text-foreground shadow-sm ring-1 ring-border"
                : "font-medium text-muted-foreground hover:bg-muted/50 hover:text-foreground",
            )}
          >
            {t.label}
          </button>
        );
      })}
    </div>
  );
}

function PreviewStack({
  tabs,
  active,
}: {
  tabs: PreviewTab[];
  active: number;
}) {
  return (
    <div className="relative w-full min-w-0 md:flex-1">
      {tabs.map((t, i) => {
        const isActive = i === active;
        return (
          <div
            key={t.id}
            role="tabpanel"
            aria-hidden={!isActive}
            className={cn(
              "transition-opacity duration-500",
              isActive
                ? "relative opacity-100"
                : "pointer-events-none absolute inset-0 opacity-0",
            )}
          >
            {t.media}
          </div>
        );
      })}
    </div>
  );
}

function CtaButton({
  cta,
  variant,
  type,
}: {
  cta: Cta;
  variant: "primary" | "secondary";
  type?: "submit" | "button";
}) {
  const className = cn(
    "inline-flex h-10 items-center justify-center gap-2 whitespace-nowrap rounded-xl px-3.5 text-sm font-medium transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 focus-visible:ring-offset-background",
    variant === "primary"
      ? "bg-primary text-primary-foreground shadow-sm hover:bg-primary/90"
      : "bg-muted text-muted-foreground hover:bg-background hover:text-foreground hover:shadow-sm hover:ring-1 hover:ring-border",
  );
  const body = (
    <>
      {cta.label}
      {variant === "primary" && <ArrowUpRight className="size-4 shrink-0" />}
    </>
  );
  if (cta.href) {
    return (
      <Link href={cta.href} className={className}>
        {body}
      </Link>
    );
  }
  return (
    <button type={type ?? "button"} className={className}>
      {body}
    </button>
  );
}

/* ── component ───────────────────────────────────────────────── */

export function PreviewSwitchHero({
  badge,
  title,
  description,
  ratings,
  showEmail = true,
  emailLabel = "Enter email address",
  emailPlaceholder = "you@example.com",
  onSubmit,
  primaryCta,
  secondaryCta,
  avatars,
  socialProof,
  tabs,
  logos,
  className,
}: PreviewSwitchHeroProps) {
  const emailId = React.useId();
  const [active, setActive] = React.useState(0);

  // Tabs are click-driven; clicking one swaps the preview in place.
  const handleSelect = (i: number) => setActive(i);

  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const data = new FormData(e.currentTarget);
    onSubmit?.(String(data.get("email") ?? ""));
  };

  return (
    <section
      aria-label="Hero"
      className={cn("relative w-full bg-background", className)}
    >
      <div>
        <div className="mx-auto w-full max-w-7xl px-6 py-10 lg:py-14">
          <div className="flex flex-col-reverse justify-center gap-8 md:flex-row md:items-start md:gap-6 lg:gap-10 xl:gap-[72px]">
            {/* ── left: text-tab rail + switchable preview ─────── */}
            <div
              className={cn(
                "flex min-w-0 flex-col gap-5 md:w-[400px] md:shrink-0 md:flex-row md:gap-4 lg:w-[480px] lg:gap-6",
                // When a badge sits above the title, drop the rail + preview by
                // the badge's footprint (md+) so their tops line up with the
                // title rather than the badge.
                badge && "md:mt-11",
              )}
            >
              <TabRail tabs={tabs} active={active} onSelect={handleSelect} />
              <PreviewStack tabs={tabs} active={active} />
            </div>

            {/* ── right: content (centered on mobile, left on md+) ── */}
            <div className="flex min-w-0 flex-col items-center text-center md:max-w-[496px] md:flex-1 md:items-start md:text-left">
              {badge && (
                <div className="mb-4 flex w-fit items-center gap-2 rounded-lg bg-muted py-1 pl-1.5 pr-2.5">
                  {badge.tag && (
                    <span className="inline-flex h-4 items-center rounded-[5px] bg-background px-1.5 text-[10px] font-semibold uppercase tracking-wide text-primary shadow-sm">
                      {badge.tag}
                    </span>
                  )}
                  <span className="text-sm text-muted-foreground">
                    {badge.label}
                  </span>
                </div>
              )}

              <h1 className="mb-4 text-balance text-3xl font-semibold tracking-tight text-foreground sm:text-4xl lg:mb-5 lg:text-5xl xl:text-[56px] xl:leading-[1.05]">
                {title}
              </h1>

              {description && (
                <p className="text-balance text-base text-muted-foreground lg:text-lg">
                  {description}
                </p>
              )}

              {ratings && ratings.length > 0 && (
                <div className="mt-6 flex flex-wrap items-center justify-center gap-2 md:justify-start lg:mt-8">
                  {ratings.map((r, i) => (
                    <div
                      key={`${r.source}-${i}`}
                      className="inline-flex items-center gap-1.5 rounded-full border border-border bg-muted/40 py-1 pl-2 pr-3"
                    >
                      {r.icon ?? (
                        <Star className="size-3.5 fill-amber-400 text-amber-400" />
                      )}
                      <span className="text-sm font-semibold text-foreground">
                        {r.score}
                      </span>
                      <span className="text-sm text-muted-foreground">
                        {r.source}
                      </span>
                    </div>
                  ))}
                </div>
              )}

              {showEmail ? (
                <form onSubmit={handleSubmit} className="mt-6 lg:mt-8">
                  <div className="mx-auto flex w-full max-w-[420px] flex-col gap-2 md:mx-0">
                    <label
                      htmlFor={emailId}
                      className="text-sm text-muted-foreground"
                    >
                      {emailLabel}
                    </label>
                    <div className="flex items-center gap-2 rounded-xl border border-border bg-background px-3 shadow-sm transition focus-within:border-foreground focus-within:ring-2 focus-within:ring-ring">
                      <Mail className="size-5 shrink-0 text-muted-foreground" />
                      <input
                        id={emailId}
                        name="email"
                        type="email"
                        placeholder={emailPlaceholder}
                        className="h-10 w-full bg-transparent text-sm text-foreground outline-none placeholder:text-muted-foreground"
                      />
                    </div>

                    {(primaryCta || secondaryCta) && (
                      <div className="mt-2 flex flex-wrap justify-center gap-3 md:justify-start">
                        {primaryCta && (
                          <CtaButton
                            cta={primaryCta}
                            variant="primary"
                            type="submit"
                          />
                        )}
                        {secondaryCta && (
                          <CtaButton cta={secondaryCta} variant="secondary" />
                        )}
                      </div>
                    )}
                  </div>
                </form>
              ) : (
                (primaryCta || secondaryCta) && (
                  <div className="mt-6 flex flex-wrap justify-center gap-3 md:justify-start lg:mt-8">
                    {primaryCta && (
                      <CtaButton
                        cta={primaryCta}
                        variant="primary"
                        type="button"
                      />
                    )}
                    {secondaryCta && (
                      <CtaButton cta={secondaryCta} variant="secondary" />
                    )}
                  </div>
                )
              )}

              {(avatars?.length || socialProof) && (
                <div className="mt-6 flex flex-col items-center gap-y-3 md:flex-row">
                  {avatars && avatars.length > 0 && (
                    <div className="flex items-center">
                      {avatars.map((a, i) => (
                        <span
                          key={i}
                          className={cn(
                            "flex size-7 items-center justify-center overflow-hidden rounded-full border-2 border-background bg-muted text-[10px] font-medium text-muted-foreground",
                            i > 0 && "-ml-2",
                          )}
                        >
                          {a.src ? (
                            // eslint-disable-next-line @next/next/no-img-element
                            <img
                              src={a.src}
                              alt=""
                              className="size-full object-cover"
                            />
                          ) : (
                            a.initials
                          )}
                        </span>
                      ))}
                    </div>
                  )}
                  {socialProof && (
                    <span className="text-sm text-muted-foreground md:ml-3">
                      {socialProof}
                    </span>
                  )}
                </div>
              )}
            </div>
          </div>
        </div>

        {/* ── logo strip ─────────────────────────────────────── */}
        {logos && logos.length > 0 && (
          <div className="border-y border-border">
            <div className="mx-auto max-w-7xl lg:px-7">
              <div className="flex items-center overflow-x-auto [scrollbar-width:none] lg:overflow-visible [&::-webkit-scrollbar]:hidden">
                {logos.map((l, i) => (
                  <div
                    key={`${l.name}-${i}`}
                    className="flex shrink-0 items-center lg:w-full lg:shrink"
                  >
                    <div className="flex w-full items-center justify-center px-6 py-5 lg:px-0 lg:py-7">
                      {l.logo ?? (
                        <span className="whitespace-nowrap text-base font-semibold tracking-tight text-muted-foreground">
                          {l.name}
                        </span>
                      )}
                    </div>
                    {i < logos.length - 1 && (
                      <div
                        aria-hidden
                        className="h-9 w-px shrink-0 bg-border"
                      />
                    )}
                  </div>
                ))}
              </div>
            </div>
          </div>
        )}
      </div>
    </section>
  );
}

export default PreviewSwitchHero;

demo.tsx
import { PreviewSwitchHero } from "@/components/ui/preview-switch-hero";

import {
  Battery,
  Boxes,
  Gem,
  Hexagon,
  Orbit,
  Signal,
  Spline,
  Waypoints,
  Wifi,
} from "lucide-react";

/* ── minimal phone mock (iPhone frame + a single line of copy) ──── */

function PhonePanel({ title, subtitle }: { title: string; subtitle: string }) {
  return (
    // iPhone-style frame with a soft bottom fade so it dissolves into the page.
    <div className="relative mx-auto w-full max-w-[400px] px-2 [mask-image:linear-gradient(to_bottom,black_80%,transparent)]">
      {/* outer bezel */}
      <div className="overflow-hidden rounded-t-[2.5rem] bg-background/75 px-2 pt-2 shadow-md shadow-black/[0.06] ring-1 ring-foreground/10">
        {/* screen — fixed height so switching tabs never resizes the phone */}
        <div className="h-[320px] overflow-hidden rounded-t-[2rem] bg-foreground/[0.03] px-6 ring-1 ring-foreground/10 dark:bg-black">
          {/* status bar */}
          <div className="flex items-center justify-between py-2 text-xs text-foreground">
            <span className="font-semibold">9:41</span>
            <div className="flex items-end gap-1">
              <Signal aria-hidden className="size-4" />
              <Wifi aria-hidden className="size-[18px]" />
              <Battery aria-hidden className="-mb-px size-5" />
            </div>
          </div>

          {/* grabber */}
          <div className="mx-auto mt-3 h-1.5 w-10 rounded-full bg-foreground/15" />

          {/* small text */}
          <div className="px-2 pt-12 text-center">
            <p className="text-2xl font-semibold tracking-tight text-foreground/80">
              {title}
            </p>
            <p className="mt-2 text-sm text-muted-foreground">{subtitle}</p>
          </div>
        </div>
      </div>
    </div>
  );
}

const PANELS = [
  {
    title: "Pick a time",
    subtitle: "Guests book in two taps — no account needed.",
  },
  {
    title: "Always in sync",
    subtitle: "Reads every calendar so you're never double-booked.",
  },
  {
    title: "Zero no-shows",
    subtitle: "Automatic email and SMS nudges before each call.",
  },
  {
    title: "Round-robin",
    subtitle: "Route each booking to whoever's free first.",
  },
];

/* ── client logos ────────────────────────────────────────────────
 * Fictional brands rendered as icon + wordmark. Self-contained (no
 * external assets or real third-party marks) and theme-adaptive — the
 * icon inherits `currentColor`, so it tracks light/dark automatically.
 */

const LOGO_CLS =
  "inline-flex items-center gap-1.5 text-base font-semibold tracking-tight text-muted-foreground";

const CLIENT_LOGOS = [
  { name: "Hexa", Icon: Hexagon },
  { name: "Orbital", Icon: Orbit },
  { name: "Facet", Icon: Gem },
  { name: "Stackline", Icon: Boxes },
  { name: "Wayline", Icon: Waypoints },
  { name: "Curveo", Icon: Spline },
].map(({ name, Icon }) => ({
  name,
  logo: (
    <span className={LOGO_CLS}>
      <Icon aria-hidden className="size-5" />
      {name}
    </span>
  ),
}));

/* ── demo ─────────────────────────────────────────────────────────── */

export default function PreviewSwitchHeroDemo() {
  const tabs = [
    { id: "booking", label: "Booking" },
    { id: "availability", label: "Availability" },
    { id: "reminders", label: "Reminders" },
    { id: "team", label: "Team" },
  ].map((t, i) => ({ ...t, media: <PhonePanel {...PANELS[i]} /> }));

  return (
    <PreviewSwitchHero
      badge={{ tag: "New", label: "Round-robin scheduling for teams" }}
      title="Meetings booked without the back-and-forth"
      description="Share one link, sync every calendar, and let guests pick a time that actually works — no email ping-pong."
      ratings={[
        { source: "ease of use", score: "4.9" },
        { source: "support", score: "4.8" },
        { source: "value", score: "4.9" },
      ]}
      showEmail={false}
      primaryCta={{ label: "Get started", href: "#" }}
      secondaryCta={{ label: "Book a demo", href: "#" }}
      avatars={[
        { initials: "AK" },
        { initials: "MJ" },
        { initials: "RP" },
        { initials: "SL" },
        { initials: "TD" },
        { initials: "EV" },
      ]}
      socialProof="loved by 30,000+ teams"
      tabs={tabs}
      logos={CLIENT_LOGOS}
    />
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
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
