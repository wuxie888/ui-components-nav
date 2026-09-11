<!-- Color Slider · @intentui · https://21st.dev/@intentui/components/color-slider
     license: MIT · category: slider
     A horizontal or vertical slider for adjusting a single color channel such as hue, saturation, or brightness, built on React Aria Components. -->

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
components/ui/color-slider.tsx
'use client'
import type {
  ColorSliderProps,
  SliderOutputProps,
  SliderTrackProps,
} from 'react-aria-components/ColorSlider'
import {
  ColorSlider as PrimitiveColorSlider,
  SliderOutput,
  SliderTrack,
} from 'react-aria-components/ColorSlider'
import { fieldStyles } from '@/components/ui/field'
import { cx } from '@/lib/primitive'

export function ColorSlider({ className, ...props }: ColorSliderProps) {
  return (
    <PrimitiveColorSlider
      data-slot="control"
      className={cx(
        'orientation-vertical:flex orientation-horizontal:grid orientation-horizontal:w-full grid-cols-[1fr_auto] flex-col items-center gap-2',
        fieldStyles(),
        className
      )}
      {...props}
    />
  )
}

export function ColorSliderOutput({ className, ...props }: SliderOutputProps) {
  return (
    <SliderOutput
      className={cx('orientation-vertical:hidden font-medium text-base/6 sm:text-sm/6', className)}
      {...props}
    />
  )
}

export function ColorSliderTrack({ className, ...props }: SliderTrackProps) {
  return (
    <SliderTrack
      className={cx(
        'group col-span-2 orientation-horizontal:h-6 rounded-lg',
        'orientation-horizontal:h-6 orientation-horizontal:w-full',
        'orientation-vertical:ms-[50%] orientation-vertical:h-56 orientation-vertical:w-6 orientation-vertical:-translate-x-[50%]',
        'disabled:bg-muted-fg disabled:opacity-50 forced-colors:bg-[GrayText]',
        className
      )}
      {...props}
      style={({ defaultStyle, isDisabled }) => ({
        ...defaultStyle,
        background: isDisabled
          ? undefined
          : `${defaultStyle.background}, repeating-conic-gradient(#CCC 0% 25%, white 0% 50%) 50% / 16px 16px`,
      })}
    />
  )
}

demo.tsx
"use client";

import {
  ColorSlider,
  ColorSliderOutput,
  ColorSliderTrack,
} from "@/components/ui/color-slider";
import { ColorThumb } from "@/components/ui/color-slider-utils/color-thumb";
import { Description } from "@/components/ui/field";

export default function ColorSliderDemo() {
  return (
    <div className="flex w-full justify-center p-6">
      <ColorSlider
        channel="hue"
        defaultValue="hsl(0, 100%, 50%)"
        className="w-full max-w-xs"
      >
        <ColorSliderOutput />
        <ColorSliderTrack>
          <ColorThumb />
        </ColorSliderTrack>
        <Description>
          This color slider is using the{" "}
          <strong className="font-medium text-fg">hue</strong> channel.
        </Description>
      </ColorSlider>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install react-aria-components
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add field field?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068&publisher_install_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdXJwb3NlIjoicHVibGlzaGVyLXJlZ2lzdHJ5LWluc3RhbGwiLCJpYXQiOjE3ODcxMTkxNTcsImV4cCI6MTc4NzExOTc1N30.gDPkDVeVG8VArfAdDCjWMciDvyEf-3PthQx9s5JhUJk primitive
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
