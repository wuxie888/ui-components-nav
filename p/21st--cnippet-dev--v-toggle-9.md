<!-- Video Call Controls Bar · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-toggle-9
     license: MIT · category: toggle
     A video call control bar with mic, camera, and screen-share toggles that swap icons and turn red when disabled, plus a leave-call button and an animated in-call status indicator. -->

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
components/ui/v-toggle-9.tsx
"use client";

import {
  MicIcon,
  MicOffIcon,
  MonitorIcon,
  MonitorOffIcon,
  PhoneOffIcon,
  VideoIcon,
  VideoOffIcon,
} from "lucide-react";
import { useState } from "react";

import { Toggle } from "@/registry/default/ui/toggle";

export function Pattern() {
  const [micOn, setMicOn] = useState(true);
  const [camOn, setCamOn] = useState(true);
  const [screenOn, setScreenOn] = useState(false);

  return (
    <div className="flex flex-col items-center gap-6">
      <div className="flex w-full max-w-xs flex-col items-center gap-2 rounded-2xl border bg-card px-6 py-5 shadow-sm">
        <div className="mb-1 flex items-center gap-1.5">
          <span className="relative flex size-2">
            <span className="absolute inline-flex h-full w-full animate-ping rounded-full bg-green-400 opacity-75" />
            <span className="relative inline-flex size-2 rounded-full bg-green-500" />
          </span>
          <span className="text-muted-foreground text-xs">In call · 12:34</span>
        </div>

        <div className="flex items-end gap-3">
          <div className="flex flex-col items-center gap-1.5">
            <Toggle
              aria-label="Toggle microphone"
              className={
                !micOn
                  ? "border-destructive/20 bg-destructive/10 text-destructive hover:bg-destructive/15 data-pressed:bg-destructive/10"
                  : ""
              }
              onPressedChange={(p) => setMicOn(!p)}
              pressed={!micOn}
              size="lg"
              variant="outline"
            >
              {micOn ? <MicIcon /> : <MicOffIcon />}
            </Toggle>
            <span className="text-muted-foreground text-xs">
              {micOn ? "Mute" : "Unmute"}
            </span>
          </div>

          <div className="flex flex-col items-center gap-1.5">
            <Toggle
              aria-label="Toggle camera"
              className={
                !camOn
                  ? "border-destructive/20 bg-destructive/10 text-destructive hover:bg-destructive/15 data-pressed:bg-destructive/10"
                  : ""
              }
              onPressedChange={(p) => setCamOn(!p)}
              pressed={!camOn}
              size="lg"
              variant="outline"
            >
              {camOn ? <VideoIcon /> : <VideoOffIcon />}
            </Toggle>
            <span className="text-muted-foreground text-xs">
              {camOn ? "Stop video" : "Start video"}
            </span>
          </div>

          <div className="flex flex-col items-center gap-1.5">
            <Toggle
              aria-label="Toggle screen share"
              onPressedChange={setScreenOn}
              pressed={screenOn}
              size="lg"
              variant="outline"
            >
              {screenOn ? <MonitorOffIcon /> : <MonitorIcon />}
            </Toggle>
            <span className="text-muted-foreground text-xs">
              {screenOn ? "Stop share" : "Share screen"}
            </span>
          </div>

          <div className="flex flex-col items-center gap-1.5">
            <button
              aria-label="Leave call"
              className="inline-flex size-10 items-center justify-center rounded-lg bg-destructive text-white shadow-xs transition-opacity hover:opacity-90 sm:size-9"
              type="button"
            >
              <PhoneOffIcon className="size-4" />
            </button>
            <span className="text-muted-foreground text-xs">Leave</span>
          </div>
        </div>
      </div>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-toggle-9";

export default function Default() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center bg-background p-6">
      <Pattern />
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
npx shadcn@latest add toggle
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
