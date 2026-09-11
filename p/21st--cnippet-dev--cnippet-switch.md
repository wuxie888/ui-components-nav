<!-- Switch · @cnippet-dev · https://21st.dev/@cnippet-dev/components/cnippet-switch
     license: MIT · category: form
     An accessible toggle switch built on Base UI's Switch primitive. Animated thumb with a springy press/scale interaction, two sizes (default and sm), controlled or uncontrolled usage via checked / defaultChecked, disabled state, and easy accent recoloring through the data-checked:* utilities. Re-exports SwitchPrimitive for advanced composition. -->

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
components/ui/switch.tsx
"use client";

import { Switch as SwitchPrimitive } from "@base-ui/react/switch";
import type React from "react";
import { cn } from "@/registry/default/lib/utils";

export function Switch({
  className,
  size = "default",
  ...props
}: SwitchPrimitive.Root.Props & {
  size?: "default" | "sm";
}): React.ReactElement {
  return (
    <SwitchPrimitive.Root
      className={cn(
        "inline-flex h-[calc(var(--thumb-size)+2px)] w-[calc(var(--thumb-size)*2-2px)] shrink-0 items-center rounded-full p-px outline-none transition-[background-color,box-shadow] duration-200 focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-1 focus-visible:ring-offset-background data-disabled:cursor-not-allowed data-checked:bg-primary data-unchecked:bg-input data-disabled:opacity-64",
        size === "sm"
          ? "[--thumb-size:--spacing(4)]"
          : "[--thumb-size:--spacing(5)] sm:[--thumb-size:--spacing(4)]",
        className,
      )}
      data-slot="switch"
      {...props}
    >
      <SwitchPrimitive.Thumb
        className={cn(
          "pointer-events-none block aspect-square h-full origin-left in-[[role=switch]:active,[data-slot=label]:active,[data-slot=field-label]:active]:not-data-disabled:scale-x-110 in-[[role=switch]:active,[data-slot=label]:active,[data-slot=field-label]:active]:rounded-[var(--thumb-size)/calc(var(--thumb-size)*1.1)] rounded-(--thumb-size) bg-background shadow-sm/5 will-change-transform [transition:translate_.15s,border-radius_.15s,scale_.1s_.1s,transform-origin_.15s] data-checked:origin-[var(--thumb-size)_50%] data-checked:translate-x-[calc(var(--thumb-size)-4px)]",
        )}
        data-slot="switch-thumb"
      />
    </SwitchPrimitive.Root>
  );
}

export { SwitchPrimitive };

demo.tsx
"use client";

import { BellOffIcon, MoonIcon } from "lucide-react";
import { useState } from "react";
import { Switch } from "@/components/ui/cnippet-switch";

const scheduleOptions = ["1 hour", "2 hours", "4 hours", "Until tomorrow"];

export default function SwitchDoNotDisturb() {
  const [dnd, setDnd] = useState(false);
  const [selected, setSelected] = useState("1 hour");

  return (
    <div className="w-full max-w-sm space-y-4 rounded-xl border p-5">
      <div className="flex items-center justify-between">
        <div className="flex items-center gap-2">
          <div
            className={`flex size-9 items-center justify-center rounded-lg transition-colors ${dnd ? "bg-primary/10 text-primary" : "bg-muted text-muted-foreground"}`}
          >
            {dnd ? (
              <BellOffIcon aria-hidden="true" className="size-4" />
            ) : (
              <MoonIcon aria-hidden="true" className="size-4" />
            )}
          </div>
          <div>
            <p className="font-medium text-sm">Do Not Disturb</p>
            <p className="text-muted-foreground text-xs">
              {dnd ? `Active for ${selected}` : "All notifications enabled"}
            </p>
          </div>
        </div>
        <Switch checked={dnd} onCheckedChange={setDnd} />
      </div>

      {dnd && (
        <div className="space-y-2">
          <p className="font-medium text-muted-foreground text-xs uppercase tracking-wide">
            Duration
          </p>
          <div className="grid grid-cols-2 gap-2">
            {scheduleOptions.map((opt) => (
              <button
                className={`rounded-lg border px-3 py-2 font-medium text-xs transition-colors ${
                  selected === opt
                    ? "border-primary bg-primary/5 text-primary"
                    : "hover:bg-muted"
                }`}
                key={opt}
                onClick={() => setSelected(opt)}
                type="button"
              >
                {opt}
              </button>
            ))}
          </div>
        </div>
      )}
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @base-ui-components/react @base-ui/react
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
