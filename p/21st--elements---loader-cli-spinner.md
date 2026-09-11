<!-- CLI Spinner Loader · @elements- · https://21st.dev/@elements-/components/loader-cli-spinner
     license: MIT · category: spinner
     A terminal-style text spinner with 90 selectable ASCII/braille animation variants for loading states. -->

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
components/ui/loader-cli-spinner.tsx
"use client";

import { useEffect, useRef, useState } from "react";

import { cn } from "@/lib/utils";

import { CLI_SPINNERS, type CliSpinnerVariant } from "./cli-spinner-data";

export type LoaderCliSpinnerProps = {
  variant?: CliSpinnerVariant;
  speed?: number;
  size?: number | string;
  paused?: boolean;
  ariaLabel?: string;
  className?: string;
};

export function LoaderCliSpinner({
  variant = "braille-spin",
  speed = 1,
  size = "1em",
  paused = false,
  ariaLabel = "Loading",
  className,
}: LoaderCliSpinnerProps) {
  const spinner = CLI_SPINNERS[variant] ?? CLI_SPINNERS["braille-spin"];
  const [index, setIndex] = useState(0);
  const indexRef = useRef(0);

  useEffect(() => {
    indexRef.current = 0;
    setIndex(0);
    if (paused || speed <= 0) return;
    const delay = Math.max(16, spinner.interval / speed);
    const id = window.setInterval(() => {
      indexRef.current = (indexRef.current + 1) % spinner.frames.length;
      setIndex(indexRef.current);
    }, delay);
    return () => window.clearInterval(id);
  }, [spinner, speed, paused]);

  const fontSize = typeof size === "number" ? `${size}px` : size;

  return (
    <output
      aria-label={ariaLabel}
      aria-live="polite"
      className={cn(
        "inline-flex items-center justify-center font-mono leading-none tabular-nums",
        className,
      )}
      style={{ fontSize, minWidth: "1ch" }}
    >
      <span aria-hidden="true">{spinner.frames[index]}</span>
    </output>
  );
}

export type { CliSpinnerVariant } from "./cli-spinner-data";
export { CLI_SPINNER_VARIANTS } from "./cli-spinner-data";

components/ui/cli-spinner-data.ts
// Frame data adapted from sindresorhus/cli-spinners (MIT)
// Renamed, reorganized, and re-exported for React UI.

export type CliSpinnerVariant =
  | "aesthetic"
  | "arc-spin"
  | "arrow-compass"
  | "arrow-move"
  | "arrow-spin"
  | "ball-bar"
  | "balloon-large"
  | "balloon-small"
  | "bar-bounce"
  | "beta-wave"
  | "bit-stream"
  | "blue-pulse"
  | "bouncer"
  | "box-bounce"
  | "box-bounce-thick"
  | "braille-bounce"
  | "braille-cascade"
  | "braille-circle"
  | "braille-corner"
  | "braille-drift"
  | "braille-fold"
  | "braille-matrix"
  | "braille-orbit"
  | "braille-pulse"
  | "braille-shift"
  | "braille-spin"
  | "braille-sweep"
  | "braille-tick"
  | "braille-wave"
  | "circle-pulse"
  | "clockwork"
  | "dot-circle"
  | "duo-pulse"
  | "dwarf-fortress"
  | "earth"
  | "ellipsis"
  | "ellipsis-scroll"
  | "fingers"
  | "fish"
  | "fist-bump"
  | "flipper"
  | "grenade"
  | "grow-h"
  | "grow-v"
  | "half-orbit"
  | "hearts"
  | "layer"
  | "letter-spin"
  | "line-dash"
  | "line-roll"
  | "line-slash"
  | "material-bar"
  | "mindblown"
  | "monkey"
  | "moon-phase"
  | "orange-pulse"
  | "pipe-turn"
  | "plus-pulse"
  | "point"
  | "pong"
  | "quarter-orbit"
  | "retro-braille"
  | "runner"
  | "sandglass"
  | "shark"
  | "smiley-spin"
  | "soccer"
  | "speaker"
  | "square-corners"
  | "squisher"
  | "stacker"
  | "static-noise"
  | "time-travel"
  | "toggle-1"
  | "toggle-10"
  | "toggle-11"
  | "toggle-12"
  | "toggle-13"
  | "toggle-2"
  | "toggle-3"
  | "toggle-4"
  | "toggle-5"
  | "toggle-6"
  | "toggle-7"
  | "toggle-8"
  | "toggle-9"
  | "tree"
  | "triangle-spin"
  | "twinkle"
  | "weather";

export type SpinnerData = {
  interval: number;
  frames: readonly string[];
};

export const CLI_SPINNERS: Record<CliSpinnerVariant, SpinnerData> = {
  aesthetic: {
    interval: 80,
    frames: [
      "▰▱▱▱▱▱▱",
      "▰▰▱▱▱▱▱",
      "▰▰▰▱▱▱▱",
      "▰▰▰▰▱▱▱",
      "▰▰▰▰▰▱▱",
      "▰▰▰▰▰▰▱",
      "▰▰▰▰▰▰▰",
      "▰▱▱▱▱▱▱",
    ],
  },
  "arc-spin": {
    interval: 100,
    frames: ["◜", "◠", "◝", "◞", "◡", "◟"],
  },
  "arrow-compass": {
    interval: 80,
    frames: ["⬆️ ", "↗️ ", "➡️ ", "↘️ ", "⬇️ ", "↙️ ", "⬅️ ", "↖️ "],
  },
  "arrow-move": {
    interval: 120,
    frames: ["▹▹▹▹▹", "▸▹▹▹▹", "▹▸▹▹▹", "▹▹▸▹▹", "▹▹▹▸▹", "▹▹▹▹▸"],
  },
  "arrow-spin": {
    interval: 100,
    frames: ["←", "↖", "↑", "↗", "→", "↘", "↓", "↙"],
  },
  "ball-bar": {
    interval: 80,
    frames: [
      "( ●    )",
      "(  ●   )",
      "(   ●  )",
      "(    ● )",
      "(     ●)",
      "(    ● )",
      "(   ●  )",
      "(  ●   )",
      "( ●    )",
      "(●     )",
    ],
  },
  "balloon-large": {
    interval: 120,
    frames: [".", "o", "O", "°", "O", "o", "."],
  },
  "balloon-small": {
    interval: 140,
    frames: [" ", ".", "o", "O", "@", "*", " "],
  },
  "bar-bounce": {
    interval: 80,
    frames: [
      "[    ]",
      "[=   ]",
      "[==  ]",
      "[=== ]",
      "[====]",
      "[ ===]",
      "[  ==]",
      "[   =]",
      "[    ]",
      "[   =]",
      "[  ==]",
      "[ ===]",
      "[====]",
      "[=== ]",
      "[==  ]",
      "[=   ]",
    ],
  },
  "beta-wave": {
    interval: 80,
    frames: [
      "ρββββββ",
      "βρβββββ",
      "ββρββββ",
      "βββρβββ",
      "ββββρββ",
      "βββββρβ",
      "ββββββρ",
    ],
  },
  "bit-stream": {
    interval: 80,
    frames: [
      "010010",
      "001100",
      "100101",
      "111010",
      "111101",
      "010111",
      "101011",
      "111000",
      "110011",
      "110101",
    ],
  },
  "blue-pulse": {
    interval: 100,
    frames: ["🔹 ", "🔷 ", "🔵 ", "🔵 ", "🔷 "],
  },
  bouncer: {
    interval: 120,
    frames: ["⠁", "⠂", "⠄", "⠂"],
  },
  "box-bounce": {
    interval: 120,
    frames: ["▖", "▘", "▝", "▗"],
  },
  "box-bounce-thick": {
    interval: 100,
    frames: ["▌", "▀", "▐", "▄"],
  },
  "braille-bounce": {
    interval: 80,
    frames: ["⠋", "⠙", "⠚", "⠞", "⠖", "⠦", "⠴", "⠲", "⠳", "⠓"],
  },
  "braille-cascade": {
    interval: 80,
    frames: [
      "⢀⠀",
      "⡀⠀",
      "⠄⠀",
      "⢂⠀",
      "⡂⠀",
      "⠅⠀",
      "⢃⠀",
      "⡃⠀",
      "⠍⠀",
      "⢋⠀",
      "⡋⠀",
      "⠍⠁",
      "⢋⠁",
      "⡋⠁",
      "⠍⠉",
      "⠋⠉",
      "⠋⠉",
      "⠉⠙",
      "⠉⠙",
      "⠉⠩",
      "⠈⢙",
      "⠈⡙",
      "⢈⠩",
      "⡀⢙",
      "⠄⡙",
      "⢂⠩",
      "⡂⢘",
      "⠅⡘",
      "⢃⠨",
      "⡃⢐",
      "⠍⡐",
      "⢋⠠",
      "⡋⢀",
      "⠍⡁",
      "⢋⠁",
      "⡋⠁",
      "⠍⠉",
      "⠋⠉",
      "⠋⠉",
      "⠉⠙",
      "⠉⠙",
      "⠉⠩",
      "⠈⢙",
      "⠈⡙",
      "⠈⠩",
      "⠀⢙",
      "⠀⡙",
      "⠀⠩",
      "⠀⢘",
      "⠀⡘",
      "⠀⠨",
      "⠀⢐",
      "⠀⡐",
      "⠀⠠",
      "⠀⢀",
      "⠀⡀",
    ],
  },
  "braille-circle": {
    interval: 80,
    frames: ["⣼", "⣹", "⢻", "⠿", "⡟", "⣏", "⣧", "⣶"],
  },
  "braille-corner": {
    interval: 80,
    frames: ["⣾", "⣽", "⣻", "⢿", "⡿", "⣟", "⣯", "⣷"],
  },
  "braille-drift": {
    interval: 80,
    frames: [
      "⠁",
      "⠁",
      "⠉",
      "⠙",
      "⠚",
      "⠒",
      "⠂",
      "⠂",
      "⠒",
      "⠲",
      "⠴",
      "⠤",
      "⠄",
      "⠄",
      "⠤",
      "⠠",
      "⠠",
      "⠤",
      "⠦",
      "⠖",
      "⠒",
      "⠐",
      "⠐",
      "⠒",
      "⠓",
      "⠋",
      "⠉",
      "⠈",
      "⠈",
    ],
  },
  "braille-fold": {
    interval: 80,
    frames: ["⢹", "⢺", "⢼", "⣸", "⣇", "⡧", "⡗", "⡏"],
  },
  "braille-matrix": {
    interval: 80,
    frames: [
      "⠉⠉",
      "⠈⠙",
      "⠀⠹",
      "⠀⢸",
      "⠀⣰",
      "⢀⣠",
      "⣀⣀",
      "⣄⡀",
      "⣆⠀",
      "⡇⠀",
      "⠏⠀",
      "⠋⠁",
    ],
  },
  "braille-orbit": {
    interval: 80,
    frames: [
      "⠁",
      "⠉",
      "⠙",
      "⠚",
      "⠒",
      "⠂",
      "⠂",
      "⠒",
      "⠲",
      "⠴",
      "⠤",
      "⠄",
      "⠄",
      "⠤",
      "⠴",
      "⠲",
      "⠒",
      "⠂",
      "⠂",
      "⠒",
      "⠚",
      "⠙",
      "⠉",
      "⠁",
    ],
  },
  "braille-pulse": {
    interval: 80,
    frames: [
      "⠄",
      "⠆",
      "⠇",
      "⠋",
      "⠙",
      "⠸",
      "⠰",
      "⠠",
      "⠰",
      "⠸",
      "⠙",
      "⠋",
      "⠇",
      "⠆",
    ],
  },
  "braille-shift": {
    interval: 80,
    frames: ["⢄", "⢂", "⢁", "⡁", "⡈", "⡐", "⡠"],
  },
  "braille-spin": {
    interval: 80,
    frames: ["⠋", "⠙", "⠹", "⠸", "⠼", "⠴", "⠦", "⠧", "⠇", "⠏"],
  },
  "braille-sweep": {
    interval: 80,
    frames: [
      "⠈",
      "⠉",
      "⠋",
      "⠓",
      "⠒",
      "⠐",
      "⠐",
      "⠒",
      "⠖",
      "⠦",
      "⠤",
      "⠠",
      "⠠",
      "⠤",
      "⠦",
      "⠖",
      "⠒",
      "⠐",
      "⠐",
      "⠒",
      "⠓",
      "⠋",
      "⠉",
      "⠈",
    ],
  },
  "braille-tick": {
    interval: 100,
    frames: ["⠁", "⠂", "⠄", "⡀", "⢀", "⠠", "⠐", "⠈"],
  },
  "braille-wave": {
    interval: 80,
    frames: [
      "⠋",
      "⠙",
      "⠚",
      "⠒",
      "⠂",
      "⠂",
      "⠒",
      "⠲",
      "⠴",
      "⠦",
      "⠖",
      "⠒",
      "⠐",
      "⠐",
      "⠒",
      "⠓",
      "⠋",
    ],
  },
  "circle-pulse": {
    interval: 120,
    frames: ["◡", "⊙", "◠"],
  },
  clockwork: {
    interval: 100,
    frames: [
      "🕛 ",
      "🕐 ",
      "🕑 ",
      "🕒 ",
      "🕓 ",
      "🕔 ",
      "🕕 ",
      "🕖 ",
      "🕗 ",
      "🕘 ",
      "🕙 ",
      "🕚 ",
    ],
  },
  "dot-circle": {
    interval: 80,
    frames: ["⢎ ", "⠎⠁", "⠊⠑", "⠈⠱", " ⡱", "⢀⡰", "⢄⡠", "⢆⡀"],
  },
  "duo-pulse": {
    interval: 100,
    frames: [
      "🔸 ",
      "🔶 ",
      "🟠 ",
      "🟠 ",
      "🔶 ",
      "🔹 ",
      "🔷 ",
      "🔵 ",
      "🔵 ",
      "🔷 ",
    ],
  },
  "dwarf-fortress": {
    interval: 80,
    frames: [
      " ██████£££  ",
      "☺██████£££  ",
      "☺██████£££  ",
      "☺▓█████£££  ",
      "☺▓█████£££  ",
      "☺▒█████£££  ",
      "☺▒█████£££  ",
      "☺░█████£££  ",
      "☺░█████£££  ",
      "☺ █████£££  ",
      " ☺█████£££  ",
      " ☺█████£££  ",
      " ☺▓████£££  ",
      " ☺▓████£££  ",
      " ☺▒████£££  ",
      " ☺▒████£££  ",
      " ☺░████£££  ",
      " ☺░████£££  ",
      " ☺ ████£££  ",
      "  ☺████£££  ",
      "  ☺████£££  ",
      "  ☺▓███£££  ",
      "  ☺▓███£££  ",
      "  ☺▒███£££  ",
      "  ☺▒███£££  ",
      "  ☺░███£££  ",
      "  ☺░███£££  ",
      "  ☺ ███£££  ",
      "   ☺███£££  ",
      "   ☺███£££  ",
      "   ☺▓██£££  ",
      "   ☺▓██£££  ",
      "   ☺▒██£££  ",
      "   ☺▒██£££  ",
      "   ☺░██£££  ",
      "   ☺░██£££  ",
      "   ☺ ██£££  ",
      "    ☺██£££  ",
      "    ☺██£££  ",
      "    ☺▓█£££  ",
      "    ☺▓█£££  ",
      "    ☺▒█£££  ",
      "    ☺▒█£££  ",
      "    ☺░█£££  ",
      "    ☺░█£££  ",
      "    ☺ █£££  ",
      "     ☺█£££  ",
      "     ☺█£££  ",
      "     ☺▓£££  ",
      "     ☺▓£££  ",
      "     ☺▒£££  ",
      "     ☺▒£££  ",
      "     ☺░£££  ",
      "     ☺░£££  ",
      "     ☺ £££  ",
      "      ☺£££  ",
      "      ☺£££  ",
      "      ☺▓££  ",
      "      ☺▓££  ",
      "      ☺▒££  ",
      "      ☺▒££  ",
      "      ☺░££  ",
      "      ☺░££  ",
      "      ☺ ££  ",
      "       ☺££  ",
      "       ☺££  ",
      "       ☺▓£  ",
      "       ☺▓£  ",
      "       ☺▒£  ",
      "       ☺▒£  ",
      "       ☺░£  ",
      "       ☺░£  ",
      "       ☺ £  ",
      "        ☺£  ",
      "        ☺£  ",
      "        ☺▓  ",
      "        ☺▓  ",
      "        ☺▒  ",
      "        ☺▒  ",
      "        ☺░  ",
      "        ☺░  ",
      "        ☺   ",
      "        ☺  &",
      "        ☺ ☼&",
      "       ☺ ☼ &",
      "       ☺☼  &",
      "      ☺☼  & ",
      "      ‼   & ",
      "     ☺   &  ",
      "    ‼    &  ",
      "   ☺    &   ",
      "  ‼     &   ",
      " ☺     &    ",
      "‼      &    ",
      "      &     ",
      "      &     ",
      "     &   ░  ",
      "     &   ▒  ",
      "    &    ▓  ",
      "    &    £  ",
      "   &    ░£  ",
      "   &    ▒£  ",
      "  &     ▓£  ",
      "  &     ££  ",
      " &     ░££  ",
      " &     ▒££  ",
      "&      ▓££  ",
      "&      £££  ",
      "      ░£££  ",
      "      ▒£££  ",
      "      ▓£££  ",
      "      █£££  ",
      "     ░█£££  ",
      "     ▒█£££  ",
      "     ▓█£££  ",
      "     ██£££  ",
      "    ░██£££  ",
      "    ▒██£££  ",
      "    ▓██£££  ",
      "    ███£££  ",
      "   ░███£££  ",
      "   ▒███£££  ",
      "   ▓███£££  ",
      "   ████£££  ",
      "  ░████£££  ",
      "  ▒████£££  ",
      "  ▓████£££  ",
      "  █████£££  ",
      " ░█████£££  ",
      " ▒█████£££  ",
      " ▓█████£££  ",
      " ██████£££  ",
      " ██████£££  ",
    ],
  },
  earth: {
    interval: 180,
    frames: ["🌍 ", "🌎 ", "🌏 "],
  },
  ellipsis: {
    interval: 400,
    frames: [".  ", ".. ", "...", "   "],
  },
  "ellipsis-scroll": {
    interval: 200,
    frames: [".  ", ".. ", "...", " ..", "  .", "   "],
  },
  fingers: {
    interval: 160,
    frames: ["🤘 ", "🤟 ", "🖖 ", "✋ ", "🤚 ", "👆 "],
  },
  fish: {
    interval: 80,
    frames: [
      "~~~~~~~~~~~~~~~~~~~~",
      "> ~~~~~~~~~~~~~~~~~~",
      "º> ~~~~~~~~~~~~~~~~~",
      "(º> ~~~~~~~~~~~~~~~~",
      "((º> ~~~~~~~~~~~~~~~",
      "<((º> ~~~~~~~~~~~~~~",
      "><((º> ~~~~~~~~~~~~~",
      " ><((º> ~~~~~~~~~~~~",
      "~ ><((º> ~~~~~~~~~~~",
      "~~ <>((º> ~~~~~~~~~~",
      "~~~ ><((º> ~~~~~~~~~",
      "~~~~ <>((º> ~~~~~~~~",
      "~~~~~ ><((º> ~~~~~~~",
      "~~~~~~ <>((º> ~~~~~~",
      "~~~~~~~ ><((º> ~~~~~",
      "~~~~~~~~ <>((º> ~~~~",
      "~~~~~~~~~ ><((º> ~~~",
      "~~~~~~~~~~ <>((º> ~~",
      "~~~~~~~~~~~ ><((º> ~",
      "~~~~~~~~~~~~ <>((º> ",
      "~~~~~~~~~~~~~ ><((º>",
      "~~~~~~~~~~~~~~ <>((º",
      "~~~~~~~~~~~~~~~ ><((",
      "~~~~~~~~~~~~~~~~ <>(",
      "~~~~~~~~~~~~~~~~~ ><",
      "~~~~~~~~~~~~~~~~~~ <",
      "~~~~~~~~~~~~~~~~~~~~",
    ],
  },
  "fist-bump": {
    interval: 80,
    frames: [
      "🤜　　　　🤛 ",
      "🤜　　　　🤛 ",
      "🤜　　　　🤛 ",
      "　🤜　　🤛　 ",
      "　　🤜🤛　　 ",
      "　🤜✨🤛　　 ",
      "🤜　✨　🤛　 ",
    ],
  },
  flipper: {
    interval: 70,
    frames: ["_", "_", "_", "-", "`", "`", "'", "´", "-", "_", "_", "_"],
  },
  grenade: {
    interval: 80,
    frames: [
      "،  ",
      "′  ",
      " ´ ",
      " ‾ ",
      "  ⸌",
      "  ⸊",
      "  |",
      "  ⁎",
      "  ⁕",
      " ෴ ",
      "  ⁓",
      "   ",
      "   ",
      "   ",
    ],
  },
  "grow-h": {
    interval: 120,
    frames: ["▏", "▎", "▍", "▌", "▋", "▊", "▉", "▊", "▋", "▌", "▍", "▎"],
  },
  "grow-v": {
    interval: 120,
    frames: ["▁", "▃", "▄", "▅", "▆", "▇", "▆", "▅", "▄", "▃"],
  },
  "half-orbit": {
    interval: 50,
    frames: ["◐", "◓", "◑", "◒"],
  },
  hearts: {
    interval: 100,
    frames: ["💛 ", "💙 ", "💜 ", "💚 ", "💗 "],
  },
  layer: {
    interval: 150,
    frames: ["-", "=", "≡"],
  },
  "letter-spin": {
    interval: 100,
    frames: ["d", "q", "p", "b"],
  },
  "line-dash": {
    interval: 130,
    frames: ["-", "\\", "|", "/"],
  },
  "line-roll": {
    interval: 80,
    frames: ["/  ", " - ", " \\ ", "  |", "  |", " \\ ", " - ", "/  "],
  },
  "line-slash": {
    interval: 100,
    frames: ["⠂", "-", "–", "—", "–", "-"],
  },
  "material-bar": {
    interval: 17,
    frames: [
      "█▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁",
      "██▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁",
      "███▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁",
      "████▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁",
      "██████▁▁▁▁▁▁▁▁▁▁▁▁▁▁",
      "██████▁▁▁▁▁▁▁▁▁▁▁▁▁▁",
      "███████▁▁▁▁▁▁▁▁▁▁▁▁▁",
      "████████▁▁▁▁▁▁▁▁▁▁▁▁",
      "█████████▁▁▁▁▁▁▁▁▁▁▁",
      "█████████▁▁▁▁▁▁▁▁▁▁▁",
      "██████████▁▁▁▁▁▁▁▁▁▁",
      "███████████▁▁▁▁▁▁▁▁▁",
      "█████████████▁▁▁▁▁▁▁",
      "██████████████▁▁▁▁▁▁",
      "██████████████▁▁▁▁▁▁",
      "▁██████████████▁▁▁▁▁",
      "▁██████████████▁▁▁▁▁",
      "▁██████████████▁▁▁▁▁",
      "▁▁██████████████▁▁▁▁",
      "▁▁▁██████████████▁▁▁",
      "▁▁▁▁█████████████▁▁▁",
      "▁▁▁▁██████████████▁▁",
      "▁▁▁▁██████████████▁▁",
      "▁▁▁▁▁██████████████▁",
      "▁▁▁▁▁██████████████▁",
      "▁▁▁▁▁██████████████▁",
      "▁▁▁▁▁▁██████████████",
      "▁▁▁▁▁▁██████████████",
      "▁▁▁▁▁▁▁█████████████",
      "▁▁▁▁▁▁▁█████████████",
      "▁▁▁▁▁▁▁▁████████████",
      "▁▁▁▁▁▁▁▁████████████",
      "▁▁▁▁▁▁▁▁▁███████████",
      "▁▁▁▁▁▁▁▁▁███████████",
      "▁▁▁▁▁▁▁▁▁▁██████████",
      "▁▁▁▁▁▁▁▁▁▁██████████",
      "▁▁▁▁▁▁▁▁▁▁▁▁████████",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁███████",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁██████",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁█████",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁█████",
      "█▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁████",
      "██▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁███",
      "██▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁███",
      "███▁▁▁▁▁▁▁▁▁▁▁▁▁▁███",
      "████▁▁▁▁▁▁▁▁▁▁▁▁▁▁██",
      "█████▁▁▁▁▁▁▁▁▁▁▁▁▁▁█",
      "█████▁▁▁▁▁▁▁▁▁▁▁▁▁▁█",
      "██████▁▁▁▁▁▁▁▁▁▁▁▁▁█",
      "████████▁▁▁▁▁▁▁▁▁▁▁▁",
      "█████████▁▁▁▁▁▁▁▁▁▁▁",
      "█████████▁▁▁▁▁▁▁▁▁▁▁",
      "█████████▁▁▁▁▁▁▁▁▁▁▁",
      "█████████▁▁▁▁▁▁▁▁▁▁▁",
      "███████████▁▁▁▁▁▁▁▁▁",
      "████████████▁▁▁▁▁▁▁▁",
      "████████████▁▁▁▁▁▁▁▁",
      "██████████████▁▁▁▁▁▁",
      "██████████████▁▁▁▁▁▁",
      "▁██████████████▁▁▁▁▁",
      "▁██████████████▁▁▁▁▁",
      "▁▁▁█████████████▁▁▁▁",
      "▁▁▁▁▁████████████▁▁▁",
      "▁▁▁▁▁████████████▁▁▁",
      "▁▁▁▁▁▁███████████▁▁▁",
      "▁▁▁▁▁▁▁▁█████████▁▁▁",
      "▁▁▁▁▁▁▁▁█████████▁▁▁",
      "▁▁▁▁▁▁▁▁▁█████████▁▁",
      "▁▁▁▁▁▁▁▁▁█████████▁▁",
      "▁▁▁▁▁▁▁▁▁▁█████████▁",
      "▁▁▁▁▁▁▁▁▁▁▁████████▁",
      "▁▁▁▁▁▁▁▁▁▁▁████████▁",
      "▁▁▁▁▁▁▁▁▁▁▁▁███████▁",
      "▁▁▁▁▁▁▁▁▁▁▁▁███████▁",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁███████",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁███████",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁█████",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁████",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁████",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁████",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁███",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁███",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁██",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁██",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁██",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁█",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁█",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁█",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁",
      "▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁▁",
    ],
  },
  mindblown: {
    interval: 160,
    frames: [
      "😐 ",
      "😐 ",
      "😮 ",
      "😮 ",
      "😦 ",
      "😦 ",
      "😧 ",
      "😧 ",
      "🤯 ",
      "💥 ",
      "✨ ",
      "　 ",
      "　 ",
      "　 ",
    ],
  },
  monkey: {
    interval: 300,
    frames: ["🙈 ", "🙈 ", "🙉 ", "🙊 "],
  },
  "moon-phase": {
    interval: 80,
    frames: ["🌑 ", "🌒 ", "🌓 ", "🌔 ", "🌕 ", "🌖 ", "🌗 ", "🌘 "],
  },
  "orange-pulse": {
    interval: 100,
    frames: ["🔸 ", "🔶 ", "🟠 ", "🟠 ", "🔶 "],
  },
  "pipe-turn": {
    interval: 100,
    frames: ["┤", "┘", "┴", "└", "├", "┌", "┬", "┐"],
  },
  "plus-pulse": {
    interval: 80,
    frames: ["+", "x", "*"],
  },
  point: {
    interval: 125,
    frames: ["∙∙∙", "●∙∙", "∙●∙", "∙∙●", "∙∙∙"],
  },
  pong: {
    interval: 80,
    frames: [
      "▐⠂       ▌",
      "▐⠈       ▌",
      "▐ ⠂      ▌",
      "▐ ⠠      ▌",
      "▐  ⡀     ▌",
      "▐  ⠠     ▌",
      "▐   ⠂    ▌",
      "▐   ⠈    ▌",
      "▐    ⠂   ▌",
      "▐    ⠠   ▌",
      "▐     ⡀  ▌",
      "▐     ⠠  ▌",
      "▐      ⠂ ▌",
      "▐      ⠈ ▌",
      "▐       ⠂▌",
      "▐       ⠠▌",
      "▐       ⡀▌",
      "▐      ⠠ ▌",
      "▐      ⠂ ▌",
      "▐     ⠈  ▌",
      "▐     ⠂  ▌",
      "▐    ⠠   ▌",
      "▐    ⡀   ▌",
      "▐   ⠠    ▌",
      "▐   ⠂    ▌",
      "▐  ⠈     ▌",
      "▐  ⠂     ▌",
      "▐ ⠠      ▌",
      "▐ ⡀      ▌",
      "▐⠠       ▌",
    ],
  },
  "quarter-orbit": {
    interval: 120,
    frames: ["◴", "◷", "◶", "◵"],
  },
  "retro-braille": {
    interval: 80,
    frames: [
      "⠀",
      "⠁",
      "⠂",
      "⠃",
      "⠄",
      "⠅",
      "⠆",
      "⠇",
      "⡀",
      "⡁",
      "⡂",
      "⡃",
      "⡄",
      "⡅",
      "⡆",
      "⡇",
      "⠈",
      "⠉",
      "⠊",
      "⠋",
      "⠌",
      "⠍",
      "⠎",
      "⠏",
      "⡈",
      "⡉",
      "⡊",
      "⡋",
      "⡌",
      "⡍",
      "⡎",
      "⡏",
      "⠐",
      "⠑",
      "⠒",
      "⠓",
      "⠔",
      "⠕",
      "⠖",
      "⠗",
      "⡐",
      "⡑",
      "⡒",
      "⡓",
      "⡔",
      "⡕",
      "⡖",
      "⡗",
      "⠘",
      "⠙",
      "⠚",
      "⠛",
      "⠜",
      "⠝",
      "⠞",
      "⠟",
      "⡘",
      "⡙",
      "⡚",
      "⡛",
      "⡜",
      "⡝",
      "⡞",
      "⡟",
      "⠠",
      "⠡",
      "⠢",
      "⠣",
      "⠤",
      "⠥",
      "⠦",
      "⠧",
      "⡠",
      "⡡",
      "⡢",
      "⡣",
      "⡤",
      "⡥",
      "⡦",
      "⡧",
      "⠨",
      "⠩",
      "⠪",
      "⠫",
      "⠬",
      "⠭",
      "⠮",
      "⠯",
      "⡨",
      "⡩",
      "⡪",
      "⡫",
      "⡬",
      "⡭",
      "⡮",
      "⡯",
      "⠰",
      "⠱",
      "⠲",
      "⠳",
      "⠴",
      "⠵",
      "⠶",
      "⠷",
      "⡰",
      "⡱",
      "⡲",
      "⡳",
      "⡴",
      "⡵",
      "⡶",
      "⡷",
      "⠸",
      "⠹",
      "⠺",
      "⠻",
      "⠼",
      "⠽",
      "⠾",
      "⠿",
      "⡸",
      "⡹",
      "⡺",
      "⡻",
      "⡼",
      "⡽",
      "⡾",
      "⡿",
      "⢀",
      "⢁",
      "⢂",
      "⢃",
      "⢄",
      "⢅",
      "⢆",
      "⢇",
      "⣀",
      "⣁",
      "⣂",
      "⣃",
      "⣄",
      "⣅",
      "⣆",
      "⣇",
      "⢈",
      "⢉",
      "⢊",
      "⢋",
      "⢌",
      "⢍",
      "⢎",
      "⢏",
      "⣈",
      "⣉",
      "⣊",
      "⣋",
      "⣌",
      "⣍",
      "⣎",
      "⣏",
      "⢐",
      "⢑",
      "⢒",
      "⢓",
      "⢔",
      "⢕",
      "⢖",
      "⢗",
      "⣐",
      "⣑",
      "⣒",
      "⣓",
      "⣔",
      "⣕",
      "⣖",
      "⣗",
      "⢘",
      "⢙",
      "⢚",
      "⢛",
      "⢜",
      "⢝",
      "⢞",
      "⢟",
      "⣘",
      "⣙",
      "⣚",
      "⣛",
      "⣜",
      "⣝",
      "⣞",
      "⣟",
      "⢠",
      "⢡",
      "⢢",
      "⢣",
      "⢤",
      "⢥",
      "⢦",
      "⢧",
      "⣠",
      "⣡",
      "⣢",
      "⣣",
      "⣤",
      "⣥",
      "⣦",
      "⣧",
      "⢨",
      "⢩",
      "⢪",
      "⢫",
      "⢬",
      "⢭",
      "⢮",
      "⢯",
      "⣨",
      "⣩",
      "⣪",
      "⣫",
      "⣬",
      "⣭",
      "⣮",
      "⣯",
      "⢰",
      "⢱",
      "⢲",
      "⢳",
      "⢴",
      "⢵",
      "⢶",
      "⢷",
      "⣰",
      "⣱",
      "⣲",
      "⣳",
      "⣴",
      "⣵",
      "⣶",
      "⣷",
      "⢸",
      "⢹",
      "⢺",
      "⢻",
      "⢼",
      "⢽",
      "⢾",
      "⢿",
      "⣸",
      "⣹",
      "⣺",
      "⣻",
      "⣼",
      "⣽",
      "⣾",
      "⣿",
    ],
  },
  runner: {
    interval: 140,
    frames: ["🚶 ", "🏃 "],
  },
  sandglass: {
    interval: 80,
    frames: [
      "⠁",
      "⠂",
      "⠄",
      "⡀",
      "⡈",
      "⡐",
      "⡠",
      "⣀",
      "⣁",
      "⣂",
      "⣄",
      "⣌",
      "⣔",
      "⣤",
      "⣥",
      "⣦",
      "⣮",
      "⣶",
      "⣷",
      "⣿",
      "⡿",
      "⠿",
      "⢟",
      "⠟",
      "⡛",
      "⠛",
      "⠫",
      "⢋",
      "⠋",
      "⠍",
      "⡉",
      "⠉",
      "⠑",
      "⠡",
      "⢁",
    ],
  },
  shark: {
    interval: 120,
    frames: [
      "▐|\\____________▌",
      "▐_|\\___________▌",
      "▐__|\\__________▌",
      "▐___|\\_________▌",
      "▐____|\\________▌",
      "▐_____|\\_______▌",
      "▐______|\\______▌",
      "▐_______|\\_____▌",
      "▐________|\\____▌",
      "▐_________|\\___▌",
      "▐__________|\\__▌",
      "▐___________|\\_▌",
      "▐____________|\\▌",
      "▐____________/|▌",
      "▐___________/|_▌",
      "▐__________/|__▌",
      "▐_________/|___▌",
      "▐________/|____▌",
      "▐_______/|_____▌",
      "▐______/|______▌",
      "▐_____/|_______▌",
      "▐____/|________▌",
      "▐___/|_________▌",
      "▐__/|__________▌",
      "▐_/|___________▌",
      "▐/|____________▌",
    ],
  },
  "smiley-spin": {
    interval: 200,
    frames: ["😄 ", "😝 "],
  },
  soccer: {
    interval: 80,
    frames: [
      " 🧑⚽️       🧑 ",
      "🧑  ⚽️      🧑 ",
      "🧑   ⚽️     🧑 ",
      "🧑    ⚽️    🧑 ",
      "🧑     ⚽️   🧑 ",
      "🧑      ⚽️  🧑 ",
      "🧑       ⚽️🧑  ",
      "🧑      ⚽️  🧑 ",
      "🧑     ⚽️   🧑 ",
      "🧑    ⚽️    🧑 ",
      "🧑   ⚽️     🧑 ",
      "🧑  ⚽️      🧑 ",
    ],
  },
  speaker: {
    interval: 160,
    frames: ["🔈 ", "🔉 ", "🔊 ", "🔉 "],
  },
  "square-corners": {
    interval: 180,
    frames: ["◰", "◳", "◲", "◱"],
  },
  squisher: {
    interval: 100,
    frames: ["╫", "╪"],
  },
  stacker: {
    interval: 100,
    frames: ["☱", "☲", "☴"],
  },
  "static-noise": {
    interval: 100,
    frames: ["▓", "▒", "░"],
  },
  "time-travel": {
    interval: 100,
    frames: [
      "🕛 ",
      "🕚 ",
      "🕙 ",
      "🕘 ",
      "🕗 ",
      "🕖 ",
      "🕕 ",
      "🕔 ",
      "🕓 ",
      "🕒 ",
      "🕑 ",
      "🕐 ",
    ],
  },
  "toggle-1": {
    interval: 250,
    frames: ["⊶", "⊷"],
  },
  "toggle-10": {
    interval: 100,
    frames: ["㊂", "㊀", "㊁"],
  },
  "toggle-11": {
    interval: 50,
    frames: ["⧇", "⧆"],
  },
  "toggle-12": {
    interval: 120,
    frames: ["☗", "☖"],
  },
  "toggle-13": {
    interval: 80,
    frames: ["=", "*", "-"],
  },
  "toggle-2": {
    interval: 80,
    frames: ["▫", "▪"],
  },
  "toggle-3": {
    interval: 120,
    frames: ["□", "■"],
  },
  "toggle-4": {
    interval: 100,
    frames: ["■", "□", "▪", "▫"],
  },
  "toggle-5": {
    interval: 100,
    frames: ["▮", "▯"],
  },
  "toggle-6": {
    interval: 300,
    frames: ["ဝ", "၀"],
  },
  "toggle-7": {
    interval: 80,
    frames: ["⦾", "⦿"],
  },
  "toggle-8": {
    interval: 100,
    frames: ["◍", "◌"],
  },
  "toggle-9": {
    interval: 100,
    frames: ["◉", "◎"],
  },
  tree: {
    interval: 400,
    frames: ["🌲", "🎄"],
  },
  "triangle-spin": {
    interval: 50,
    frames: ["◢", "◣", "◤", "◥"],
  },
  twinkle: {
    interval: 70,
    frames: ["✶", "✸", "✹", "✺", "✹", "✷"],
  },
  weather: {
    interval: 100,
    frames: [
      "☀️ ",
      "☀️ ",
      "☀️ ",
      "🌤 ",
      "⛅️ ",
      "🌥 ",
      "☁️ ",
      "🌧 ",
      "🌨 ",
      "🌧 ",
      "🌨 ",
      "🌧 ",
      "🌨 ",
      "⛈ ",
      "🌨 ",
      "🌧 ",
      "🌨 ",
      "☁️ ",
      "🌥 ",
      "⛅️ ",
      "🌤 ",
      "☀️ ",
      "☀️ ",
    ],
  },
};

export const CLI_SPINNER_VARIANTS = Object.keys(
  CLI_SPINNERS,
) as CliSpinnerVariant[];

demo.tsx
import { LoaderCliSpinner } from "@/components/ui/loader-cli-spinner";

export default function Default() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center bg-background p-8">
      <div className="w-full max-w-md overflow-hidden rounded-lg border border-border bg-card font-mono text-sm text-card-foreground shadow-sm">
        <div className="flex items-center gap-1.5 border-b border-border px-4 py-2.5">
          <span className="size-3 rounded-full bg-red-400" />
          <span className="size-3 rounded-full bg-yellow-400" />
          <span className="size-3 rounded-full bg-green-400" />
          <span className="ml-2 text-xs text-muted-foreground">bash</span>
        </div>
        <div className="space-y-2 px-4 py-4">
          <p className="text-muted-foreground">
            <span className="text-foreground">$</span> npm install
          </p>
          <p className="flex items-center gap-2 text-foreground">
            <LoaderCliSpinner variant="braille-spin" />
            <span>Resolving dependencies</span>
          </p>
          <p className="flex items-center gap-2 text-foreground">
            <LoaderCliSpinner variant="aesthetic" />
            <span>Building modules</span>
          </p>
          <p className="flex items-center gap-2 text-foreground">
            <LoaderCliSpinner variant="line-dash" />
            <span>Compiling</span>
          </p>
        </div>
      </div>
    </div>
  );
}
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
