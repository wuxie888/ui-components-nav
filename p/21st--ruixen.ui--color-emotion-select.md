<!-- ColorEmotionSelect · @ruixen.ui · https://21st.dev/@ruixen.ui/components/color-emotion-select
     license: unspecified · category: form
     The Color-Emotion Semantic Select is a reusable UI component built with shadcn UI’s Select primitives that lets you visually map options to meaningful colors and emotions. Instead of showing plain text choices, each option carries a color indicator—such as red for “High Priority,” yellow for “Warning,” or blue for “Calm”—and can also include an emoji or icon to express mood or intensity. This makes the dropdown more intuitive, accessible, and quick to scan, especially in dashboards, analytics tools, or design systems where colors already communicate status or priority. -->

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
components/ui/color-emotion-select.tsx
"use client";

import * as React from "react";
import {
  Select,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectLabel,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select";
import { cn } from "@/lib/utils";

export interface ColorEmotionOption {
  value: string;
  label: string;
  color: string; // Tailwind color or hex
  emoji?: string; // Optional emoji for visual cue
}

interface ColorEmotionSelectProps {
  options: ColorEmotionOption[];
  label?: string; // Optional label for the whole group
  placeholder?: string; // Placeholder text
  onChange?: (value: string) => void;
  defaultValue?: string;
}

export const ColorEmotionSelect: React.FC<ColorEmotionSelectProps> = ({
  options,
  label,
  placeholder = "Select...",
  onChange,
  defaultValue,
}) => {
  return (
    <Select defaultValue={defaultValue} onValueChange={onChange}>
      <SelectTrigger className="w-[250px]">
        <SelectValue placeholder={placeholder} />
      </SelectTrigger>
      <SelectContent>
        <SelectGroup>
          {label && <SelectLabel>{label}</SelectLabel>}
          {options.map((option) => (
            <SelectItem
              key={option.value}
              value={option.value}
              className={cn("flex items-center gap-2")}
            >
              <span
                className="w-3 h-3 rounded-full"
                style={{ backgroundColor: option.color }}
              />
              {option.emoji && <span>{option.emoji}</span>}
              {option.label}
            </SelectItem>
          ))}
        </SelectGroup>
      </SelectContent>
    </Select>
  );
};

demo.tsx
"use client";

import * as React from "react";
import { ColorEmotionSelect, ColorEmotionOption } from "@/components/ui/color-emotion-select";

const moodOptions: ColorEmotionOption[] = [
  { value: "happy", label: "Happy", color: "#FFD700", emoji: "😄" },
  { value: "sad", label: "Sad", color: "#1E3A8A", emoji: "😢" },
  { value: "angry", label: "Angry", color: "#DC2626", emoji: "😡" },
  { value: "neutral", label: "Neutral", color: "#9CA3AF", emoji: "😐" },
];

export default function DemoColorEmotionSelect() {
  const [selectedMood, setSelectedMood] = React.useState<string | undefined>();

  return (
    <div className="p-4 flex flex-col gap-4">
      <ColorEmotionSelect
        options={moodOptions}
        label="Select Your Mood"
        placeholder="Choose mood"
        onChange={setSelectedMood}
        defaultValue={selectedMood}
      />
      {selectedMood && (
        <p className="text-sm text-gray-700">
          You selected: <span className="font-semibold">{selectedMood}</span>
        </p>
      )}
    </div>
  );
};
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add command input popover select
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
