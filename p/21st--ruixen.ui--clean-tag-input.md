<!-- Clean Tag Input · @ruixen.ui · https://21st.dev/@ruixen.ui/components/clean-tag-input
     license: unspecified · category: form
     This is a sleek, black-and-white tag input component where users can add multiple tags by typing and pressing Enter. Each tag is displayed as a rounded pill with subtle borders, and pressing Backspace when the input is empty removes the last tag. The component is fully responsive, works in both light and dark themes, and provides smooth hover and focus interactions for a clean, modern user experience. -->

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
components/ui/clean-tag-input.tsx
"use client";

import * as React from "react";
import { motion, AnimatePresence } from "motion/react";

/* ── sound ── */
let _a: AudioContext, _b: AudioBuffer;
const tick = () => {
  if (typeof window === "undefined") return;
  if (!_a) {
    _a = new AudioContext();
    _b = _a.createBuffer(1, (_a.sampleRate * 0.003) | 0, _a.sampleRate);
    const d = _b.getChannelData(0);
    for (let i = 0; i < d.length; i++)
      d[i] = (Math.random() * 2 - 1) * (1 - i / d.length) ** 4;
  }
  const s = _a.createBufferSource();
  s.buffer = _b;
  const g = _a.createGain();
  g.gain.value = 0.08;
  s.connect(g).connect(_a.destination);
  s.start();
};

/* ── theme ── */
const CSS = `
.ti{
  --ti-glass:linear-gradient(180deg,rgba(255,255,255,0.78),rgba(255,255,255,0.62));
  --ti-border:rgba(0,0,0,0.06);
  --ti-shadow:0 0 1px rgba(0,0,0,0.04),0 2px 8px rgba(0,0,0,0.04),inset 0 1px 0 rgba(255,255,255,0.8);
  --ti-dim:rgba(0,0,0,0.42);
  --ti-mid:rgba(0,0,0,0.55);
  --ti-hi:rgba(0,0,0,0.88);
  --ti-focus:rgba(0,0,0,0.12);
  --ti-tag:rgba(0,0,0,0.04);
  --ti-tag-border:rgba(0,0,0,0.06);
  --ti-tag-h:rgba(0,0,0,0.07);
  --ti-x:rgba(0,0,0,0.3);
  --ti-x-h:rgba(0,0,0,0.6)
}
.dark .ti,[data-theme="dark"] .ti{
  --ti-glass:linear-gradient(180deg,rgba(255,255,255,0.05),rgba(255,255,255,0.02));
  --ti-border:rgba(255,255,255,0.07);
  --ti-shadow:0 1px 3px rgba(0,0,0,0.08),inset 0 1px 0 rgba(255,255,255,0.04);
  --ti-dim:rgba(255,255,255,0.28);
  --ti-mid:rgba(255,255,255,0.5);
  --ti-hi:rgba(255,255,255,0.88);
  --ti-focus:rgba(255,255,255,0.12);
  --ti-tag:rgba(255,255,255,0.04);
  --ti-tag-border:rgba(255,255,255,0.06);
  --ti-tag-h:rgba(255,255,255,0.07);
  --ti-x:rgba(255,255,255,0.3);
  --ti-x-h:rgba(255,255,255,0.6)
}`;

/* ── types ── */
interface Tag {
  id: string;
  text: string;
}

export interface CleanTagInputProps {
  label?: string;
  hint?: string;
  placeholder?: string;
  defaultTags?: string[];
  onChange?: (tags: string[]) => void;
  sound?: boolean;
  style?: React.CSSProperties;
}

/* ── component ── */
export default function CleanTagInput({
  label = "Tags",
  hint,
  placeholder = "Type and press enter",
  defaultTags = [],
  onChange,
  sound = true,
  style,
}: CleanTagInputProps) {
  const [tags, setTags] = React.useState<Tag[]>(
    defaultTags.map((t, i) => ({ id: `${i}-${t}`, text: t })),
  );
  const [inputValue, setInputValue] = React.useState("");
  const [focused, setFocused] = React.useState(false);
  const inputRef = React.useRef<HTMLInputElement>(null);

  const addTag = (text: string) => {
    const trimmed = text.trim();
    if (!trimmed) return;
    const next = [...tags, { id: `${Date.now()}-${trimmed}`, text: trimmed }];
    setTags(next);
    setInputValue("");
    if (sound) tick();
    onChange?.(next.map((t) => t.text));
  };

  const removeTag = (id: string) => {
    const next = tags.filter((t) => t.id !== id);
    setTags(next);
    if (sound) tick();
    onChange?.(next.map((t) => t.text));
  };

  const handleKeyDown = (e: React.KeyboardEvent<HTMLInputElement>) => {
    if (e.key === "Enter") {
      e.preventDefault();
      addTag(inputValue);
    } else if (e.key === "Backspace" && inputValue === "" && tags.length) {
      e.preventDefault();
      removeTag(tags[tags.length - 1].id);
    }
  };

  return (
    <>
      <style dangerouslySetInnerHTML={{ __html: CSS }} />
      <motion.div
        className="ti"
        initial={{ opacity: 0, y: 8 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.35, ease: [0.25, 0.1, 0.25, 1] }}
        style={{
          display: "inline-flex",
          flexDirection: "column",
          gap: 6,
          ...style,
        }}
      >
        {label && (
          <span
            style={{
              fontSize: 11,
              fontWeight: 500,
              letterSpacing: "0.06em",
              textTransform: "uppercase" as const,
              color: "var(--ti-dim)",
              userSelect: "none" as const,
              paddingLeft: 4,
            }}
          >
            {label}
          </span>
        )}

        {/* glass container */}
        <div
          style={{
            display: "flex",
            flexWrap: "wrap",
            alignItems: "center",
            gap: 6,
            padding: "8px 12px",
            minHeight: 48,
            borderRadius: 12,
            background: "var(--ti-glass)",
            border: `1px solid ${focused ? "var(--ti-focus)" : "var(--ti-border)"}`,
            boxShadow: "var(--ti-shadow)",
            backdropFilter: "blur(24px)",
            WebkitBackdropFilter: "blur(24px)",
            transition: "border-color 0.2s",
            cursor: "text",
          }}
          onClick={() => inputRef.current?.focus()}
        >
          <AnimatePresence mode="popLayout">
            {tags.map((tag) => (
              <motion.div
                key={tag.id}
                layout
                initial={{ opacity: 0, scale: 0.8 }}
                animate={{ opacity: 1, scale: 1 }}
                exit={{ opacity: 0, scale: 0.8 }}
                transition={{ type: "spring", stiffness: 500, damping: 30 }}
                style={{
                  display: "flex",
                  alignItems: "center",
                  gap: 4,
                  padding: "4px 8px 4px 10px",
                  borderRadius: 6,
                  background: "var(--ti-tag)",
                  border: "1px solid var(--ti-tag-border)",
                  fontSize: 12,
                  fontWeight: 500,
                  color: "var(--ti-hi)",
                  userSelect: "none" as const,
                }}
              >
                {tag.text}
                <button
                  onClick={(e) => {
                    e.stopPropagation();
                    removeTag(tag.id);
                  }}
                  style={{
                    display: "flex",
                    alignItems: "center",
                    justifyContent: "center",
                    width: 14,
                    height: 14,
                    borderRadius: 7,
                    border: "none",
                    background: "transparent",
                    cursor: "pointer",
                    color: "var(--ti-x)",
                    transition: "color 0.15s",
                    padding: 0,
                  }}
                  onMouseEnter={(e) => {
                    e.currentTarget.style.color = "var(--ti-x-h)";
                  }}
                  onMouseLeave={(e) => {
                    e.currentTarget.style.color = "var(--ti-x)";
                  }}
                >
                  <svg
                    width={10}
                    height={10}
                    viewBox="0 0 16 16"
                    fill="none"
                    stroke="currentColor"
                    strokeWidth={2}
                    strokeLinecap="round"
                  >
                    <path d="M4 4l8 8M12 4l-8 8" />
                  </svg>
                </button>
              </motion.div>
            ))}
          </AnimatePresence>

          <input
            ref={inputRef}
            value={inputValue}
            onChange={(e) => setInputValue(e.target.value)}
            onKeyDown={handleKeyDown}
            onFocus={() => setFocused(true)}
            onBlur={() => setFocused(false)}
            placeholder={tags.length === 0 ? placeholder : ""}
            style={{
              flex: 1,
              minWidth: 80,
              height: 30,
              background: "transparent",
              border: "none",
              outline: "none",
              fontSize: 13,
              color: "var(--ti-hi)",
            }}
          />
        </div>

        {hint && (
          <span
            style={{
              fontSize: 11,
              color: "var(--ti-dim)",
              paddingLeft: 4,
            }}
          >
            {hint}
          </span>
        )}
      </motion.div>
    </>
  );
}

demo.tsx
import CleanTagInput from "@/components/ui/clean-tag-input";

export default function DemoOne() {
  return <CleanTagInput />;
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add label
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
