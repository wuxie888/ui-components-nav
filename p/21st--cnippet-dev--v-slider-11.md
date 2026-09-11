<!-- Volume Control Slider · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-slider-11
     license: no-license · category: slider
     A volume slider with a mute toggle button and a live value readout that switches icons based on level. -->

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
components/ui/v-slider-11.tsx
"use client";

import { Volume1Icon, Volume2Icon, VolumeXIcon } from "lucide-react";
import { useState } from "react";
import { Slider } from "@/registry/default/ui/slider";

export default function Particle() {
  const [volume, setVolume] = useState(60);

  const Icon =
    volume === 0 ? VolumeXIcon : volume < 50 ? Volume1Icon : Volume2Icon;

  return (
    <div className="flex w-full max-w-sm items-center gap-3">
      <button
        aria-label={volume === 0 ? "Unmute" : "Mute"}
        className="text-muted-foreground transition-colors hover:text-foreground"
        onClick={() => setVolume(volume === 0 ? 60 : 0)}
        type="button"
      >
        <Icon aria-hidden="true" className="size-5" />
      </button>
      <Slider
        aria-label="Volume"
        className="flex-1"
        max={100}
        min={0}
        onValueChange={(v) => setVolume(Array.isArray(v) ? (v[0] ?? 60) : v)}
        value={[volume]}
      />
      <span className="w-8 text-right text-muted-foreground text-sm tabular-nums">
        {volume}
      </span>
    </div>
  );
}

demo.tsx
import Component from "@/components/ui/v-slider-11";

export default function Default() {
  return (
    <div className="flex min-h-52 w-full items-center justify-center p-6">
      <Component />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add slider
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
