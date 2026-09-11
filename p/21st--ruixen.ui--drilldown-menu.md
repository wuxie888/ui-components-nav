<!-- Drilldown Menu · @ruixen.ui · https://21st.dev/@ruixen.ui/components/drilldown-menu
     license: MIT · category: text
     A list that drills into itself. The row you click stays put, fades to a grey breadcrumb with a return arrow, and its children arrive one indent deeper — no sliding panels. -->

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
components/ui/drilldown-menu.tsx
"use client";

import * as React from "react";
import {
  AnimatePresence,
  motion,
  usePresence,
  useReducedMotion,
} from "motion/react";
import { cn } from "@/lib/utils";

/**
 * Drilldown Menu — a list that drills into itself.
 *
 * Clicking a row with children does not push a panel over the top. The row
 * stays exactly where it is, fades to grey, grows a return arrow in the gutter,
 * and its children arrive one indent step further right. Click the breadcrumb
 * to come back out.
 *
 * Movement craft — most of it is about NOT moving:
 *  - Rows are placed absolutely, by index, on a grid of one `ROW_PITCH`. A row
 *    that leaves takes no space with it, so nothing behind it has to reflow.
 *    Left in normal flow, leaving rows hold their space for a frame, arriving
 *    rows lay out around them, and then the whole list has to spring back
 *    together from wherever that put it — well over a hundred pixels of travel
 *    per row for a change that should read as a crossfade.
 *  - Rows are one flat list keyed by id. Trail rows and choices are siblings,
 *    so the row you clicked is the same element before and after and simply
 *    travels; it is never destroyed on one side and rebuilt on the other.
 *  - Nothing moves sideways. Indent is a function of the depth an item lives
 *    at, not of what is open, so becoming a breadcrumb is a pure vertical move
 *    and the label never slides out from under the cursor that clicked it.
 *  - Arriving rows do not travel at all. They are born at their final spot.
 *
 * The text carries the transition instead: each label is set per character, and
 * the characters scale and unblur into place on a short stagger, then reverse
 * out back-to-front. Only the block's height animates, so the stack breathes
 * around its centre as the row count changes.
 */

export interface DrilldownMenuItem {
  /** Stable identifier. Must be unique among its siblings. */
  id: string;
  /** Row text. Also the accessible name. */
  label: string;
  /** Children. A row with children drills in; one without is a leaf. */
  items?: DrilldownMenuItem[];
  /** Fires when a leaf row is chosen. */
  onSelect?: () => void;
}

interface DrilldownMenuProps {
  /** The tree. Content lives with the caller, never in here. */
  items: DrilldownMenuItem[];
  className?: string;
  /** Ids of the branches to open on mount, outermost first. */
  defaultPath?: string[];
  /** Fires when a leaf row is chosen, with the trail that led to it. */
  onSelect?: (item: DrilldownMenuItem, trail: DrilldownMenuItem[]) => void;
}

/**
 * Near-critically damped: rows settle in about 300ms with no overshoot.
 * Overshoot is wrong here — every row is carrying a word someone is reading.
 */
const SPRING = {
  type: "spring" as const,
  stiffness: 520,
  damping: 46,
  mass: 0.9,
};

/** Livelier, because a character travels a few pixels, not a few rows. */
const CHAR_SPRING = {
  type: "spring" as const,
  stiffness: 500,
  damping: 30,
  mass: 1,
};

/** Layout grid, in em, so the whole menu scales off one font size. */
const ROW_PITCH = 1.85;
const ROW_HEIGHT = 1.53;
const INDENT = 0.9;

/** Per-character cadence. Out is quicker than in, and runs back to front. */
const STAGGER_IN = 0.015;
const STAGGER_OUT = 0.008;

const CHAR_VARIANTS = {
  hidden: {
    opacity: 0,
    scale: 0,
    filter: "blur(4px)",
    // A tween out, not a spring: a spring's tail keeps the row mounted long
    // after it is invisible, and leaving should feel quicker than arriving.
    transition: { duration: 0.16, ease: [0.4, 0, 1, 1] as const },
  },
  visible: {
    opacity: 1,
    scale: 1,
    filter: "blur(0px)",
    transition: CHAR_SPRING,
  },
};

/** Return arrow: points back the way you came, tail curling away below. */
function ReturnArrow() {
  return (
    <svg
      aria-hidden="true"
      className="h-[0.9em] w-[0.9em]"
      fill="none"
      stroke="currentColor"
      strokeLinecap="round"
      strokeLinejoin="round"
      strokeWidth={2}
      viewBox="0 0 24 24"
    >
      <path d="M9 5 4 10l5 5" />
      <path d="M4 10h9a6 6 0 0 1 6 6v2" />
    </svg>
  );
}

/** Walk `defaultPath` down the tree, stopping at the first id that misses. */
function resolvePath(
  items: DrilldownMenuItem[],
  ids: string[],
): DrilldownMenuItem[] {
  const trail: DrilldownMenuItem[] = [];
  let level = items;
  for (const id of ids) {
    const next = level.find((item) => item.id === id);
    if (!next?.items?.length) break;
    trail.push(next);
    level = next.items;
  }
  return trail;
}

export function DrilldownMenu({
  items,
  className,
  defaultPath,
  onSelect,
}: DrilldownMenuProps) {
  const [trail, setTrail] = React.useState<DrilldownMenuItem[]>(() =>
    defaultPath ? resolvePath(items, defaultPath) : [],
  );
  const reduceMotion = useReducedMotion();

  const level = trail.length ? (trail[trail.length - 1].items ?? []) : items;

  // ONE list, not a trail list plus a choice list. Two sibling arrays under the
  // same AnimatePresence scope keys per array, so the row you click gets torn
  // down on one side and rebuilt on the other: it fades and reappears instead
  // of travelling, and anything mid-flight inside it is stranded.
  const rows = [
    ...trail.map((item, depth) => ({ item, depth, isTrail: true })),
    ...level.map((item) => ({ item, depth: trail.length, isTrail: false })),
  ];

  const handleItem = (item: DrilldownMenuItem) => {
    if (item.items?.length) {
      setTrail((current) => [...current, item]);
      return;
    }
    item.onSelect?.();
    onSelect?.(item, trail);
  };

  return (
    <div
      className={cn(
        "flex min-h-[16rem] flex-col justify-center text-2xl sm:min-h-[20rem] sm:text-3xl",
        className,
      )}
    >
      {/* Height is a CSS transition rather than an animated value: it is the
          one property here expressed in `em`, and CSS interpolates units
          natively where an animation library has to re-read them in px. */}
      <div
        className={cn(
          "relative",
          !reduceMotion && "transition-[height] duration-300 ease-out",
        )}
        style={{
          height: `${(rows.length - 1) * ROW_PITCH + ROW_HEIGHT}em`,
        }}
      >
        <AnimatePresence initial={false}>
          {rows.map(({ item, depth, isTrail }, index) => (
            <Row
              depth={depth}
              index={index}
              isTrail={isTrail}
              item={item}
              key={item.id}
              onActivate={() =>
                isTrail
                  ? setTrail((current) => current.slice(0, depth))
                  : handleItem(item)
              }
              reduceMotion={!!reduceMotion}
            />
          ))}
        </AnimatePresence>
      </div>
    </div>
  );
}

interface RowProps {
  item: DrilldownMenuItem;
  depth: number;
  index: number;
  isTrail: boolean;
  onActivate: () => void;
  reduceMotion: boolean;
}

/**
 * Position and presence are deliberately on two different elements.
 *
 * The outer one owns x/y as a plain `animate` object, because that is the only
 * form Motion re-resolves when the values change under an unchanged animation.
 * Express the same thing as a variant target and a row that stays `visible`
 * while its index changes never moves — it silently keeps the slot it had, and
 * lands on top of whatever is there now.
 *
 * The inner one owns presence, so it can orchestrate the characters in and out
 * on a stagger. It drives its own removal through `usePresence`, which is what
 * lets the exit run per character instead of collapsing the row in one step.
 */
function Row({
  item,
  depth,
  index,
  isTrail,
  onActivate,
  reduceMotion,
}: RowProps) {
  const [isPresent, safeToRemove] = usePresence();

  // Backstop: if the exit animation never reports completion, the row would
  // stay mounted forever — invisible, and still in the tab order.
  React.useEffect(() => {
    if (isPresent) return;
    const timer = window.setTimeout(() => safeToRemove?.(), 900);
    return () => window.clearTimeout(timer);
  }, [isPresent, safeToRemove]);

  return (
    <motion.div
      // Placed by CSS, moved by `layout`. Animating x/y directly cannot work
      // here: an em offset of zero is written as `transform: none`, and Motion
      // then has nothing but px to interpolate from when the row's slot
      // changes — so a row returning from the trail to the middle of a list
      // silently keeps the slot it had. `layout` measures boxes instead, so it
      // is indifferent to what units the position was authored in.
      className={cn("absolute", !isPresent && "pointer-events-none")}
      layout={reduceMotion ? false : "position"}
      style={{
        left: `${depth * INDENT}em`,
        top: `${index * ROW_PITCH}em`,
      }}
      transition={reduceMotion ? { duration: 0 } : SPRING}
    >
      <motion.button
        animate={isPresent ? "visible" : "hidden"}
        aria-label={isTrail ? `Back to ${item.label}` : undefined}
        initial="hidden"
        onAnimationComplete={() => {
          if (!isPresent) safeToRemove?.();
        }}
        onClick={onActivate}
        transition={
          reduceMotion
            ? { duration: 0 }
            : {
                staggerChildren: isPresent ? STAGGER_IN : STAGGER_OUT,
                staggerDirection: isPresent ? 1 : -1,
              }
        }
        type="button"
        className={cn(
          "rounded-[0.25em] px-[0.22em] py-[0.14em] text-left font-medium leading-[1.25] outline-none transition-colors",
          "hover:bg-muted focus-visible:ring-2 focus-visible:ring-ring",
          // Weight is deliberately identical either way: a breadcrumb is the
          // same word at the same size, greyed. Drop its weight too and the
          // label reflows its own width the moment you click it.
          isTrail ? "text-muted-foreground" : "text-foreground",
        )}
      >
        <span className="relative inline-block whitespace-pre">
          {/* CSS, not Motion. The arrow's state depends on a prop that flips
              while the row stays mounted and stays "visible" — exactly the case
              Motion does not re-resolve, whether it is expressed as a value
              inside a variant or as a label on a child of a variant tree. A
              class list is recomputed on every render and cannot get stranded. */}
          <span
            aria-hidden="true"
            className={cn(
              "pointer-events-none absolute inset-y-0 right-full mr-[0.6em] flex items-center",
              !reduceMotion && "transition-all duration-200 ease-out",
              isTrail && isPresent
                ? "translate-x-0 opacity-100"
                : "-translate-x-1 opacity-0",
            )}
          >
            <ReturnArrow />
          </span>
          {item.label.split("").map((char, charIndex) => (
            <motion.span
              className="inline-block"
              key={charIndex}
              variants={CHAR_VARIANTS}
            >
              {char}
            </motion.span>
          ))}
        </span>
      </motion.button>
    </motion.div>
  );
}

export default DrilldownMenu;

demo.tsx
// This is a file with a demo for your component
// That's what users will see in the preview
// Create new files in this directory to add more demos
"use client";
 
import * as React from "react";
import { 
  DrilldownMenu,
  type DrilldownMenuItem,
 } from "@/components/ui/drilldown-menu";

const SETTINGS: DrilldownMenuItem[] = [
  {
    id: "account",
    label: "Account",
    items: [
      { id: "profile", label: "Profile" },
      { id: "emails", label: "Email Addresses" },
      { id: "password", label: "Password" },
      { id: "sessions", label: "Active Sessions" },
      { id: "close", label: "Close Account" },
    ],
  },
  {
    id: "workspace",
    label: "Workspace",
    items: [
      { id: "general", label: "General" },
      {
        id: "members",
        label: "Members",
        items: [
          { id: "invite", label: "Invite People" },
          { id: "roles", label: "Roles" },
          { id: "pending", label: "Pending Invites" },
          { id: "guests", label: "Guest Access" },
          { id: "requests", label: "Access Requests" },
        ],
      },
      { id: "billing", label: "Billing" },
      { id: "limits", label: "Usage Limits" },
      { id: "integrations", label: "Integrations" },
    ],
  },
  {
    id: "notifications",
    label: "Notifications",
    items: [
      { id: "digest", label: "Email Digest" },
      {
        id: "push",
        label: "Push",
        items: [
          { id: "dms", label: "Direct Messages" },
          { id: "mentions", label: "Mentions" },
          { id: "replies", label: "Comment Replies" },
          { id: "assignments", label: "Task Assignments" },
          { id: "summary", label: "Weekly Summary" },
        ],
      },
      { id: "in-app", label: "In-App" },
      { id: "quiet", label: "Quiet Hours" },
    ],
  },
  {
    id: "appearance",
    label: "Appearance",
    items: [
      { id: "theme", label: "Theme" },
      { id: "density", label: "Density" },
      { id: "accent", label: "Accent Color" },
      { id: "typeface", label: "Typeface" },
    ],
  },
  {
    id: "advanced",
    label: "Advanced",
    items: [
      { id: "keys", label: "API Keys" },
      { id: "webhooks", label: "Webhooks" },
      { id: "audit", label: "Audit Log" },
      { id: "export", label: "Data Export" },
    ],
  },
];

// ONLY DEFAULT EXPORT WILL BE TREATED AS A DEMO
export default function DemoOne() {
  const [chosen, setChosen] = React.useState<string | null>(null);
 
  return (
    <div className="flex w-full flex-col items-center gap-4 px-6 py-10">
      <DrilldownMenu
        className="w-full max-w-sm"
        items={SETTINGS}
        onSelect={(item, trail) =>
          setChosen([...trail, item].map((step) => step.label).join(" / "))
        }
      />
      <p className="h-5 text-sm text-muted-foreground">{chosen}</p>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion motion
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
