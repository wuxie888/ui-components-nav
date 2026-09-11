<!-- Url Input · @ruixen.ui · https://21st.dev/@ruixen.ui/components/url-input
     license: unspecified · category: globe
     This component is a smart URL input field that enhances the user experience by automatically displaying the website’s favicon next to the entered address. As the user types or pastes a URL, the component detects the domain, fetches its favicon, and shows it inline inside the input box. If the favicon isn’t available, it falls back to a clean Lucide Globe icon, ensuring a polished look. By combining shadcn/ui inputs with dynamic favicon fetching, this component makes forms feel more interactive and visually informative. -->

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
components/ui/url-input.tsx
"use client";

import * as React from "react";
import { motion } from "motion/react";

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
.ul{
  --ul-glass:linear-gradient(180deg,rgba(255,255,255,0.78),rgba(255,255,255,0.62));
  --ul-border:rgba(0,0,0,0.06);
  --ul-shadow:0 0 1px rgba(0,0,0,0.04),0 2px 8px rgba(0,0,0,0.04),inset 0 1px 0 rgba(255,255,255,0.8);
  --ul-dim:rgba(0,0,0,0.42);
  --ul-mid:rgba(0,0,0,0.55);
  --ul-hi:rgba(0,0,0,0.88);
  --ul-focus:rgba(0,0,0,0.12);
  --ul-icon-bg:rgba(0,0,0,0.04);
  --ul-icon-border:rgba(0,0,0,0.06)
}
.dark .ul,[data-theme="dark"] .ul{
  --ul-glass:linear-gradient(180deg,rgba(255,255,255,0.05),rgba(255,255,255,0.02));
  --ul-border:rgba(255,255,255,0.07);
  --ul-shadow:0 1px 3px rgba(0,0,0,0.08),inset 0 1px 0 rgba(255,255,255,0.04);
  --ul-dim:rgba(255,255,255,0.28);
  --ul-mid:rgba(255,255,255,0.5);
  --ul-hi:rgba(255,255,255,0.88);
  --ul-focus:rgba(255,255,255,0.12);
  --ul-icon-bg:rgba(255,255,255,0.04);
  --ul-icon-border:rgba(255,255,255,0.06)
}`;

/* ── types ── */
interface UrlInputProps {
  label?: string;
  placeholder?: string;
  defaultValue?: string;
  hint?: string;
  onChange?: (url: string) => void;
  sound?: boolean;
  style?: React.CSSProperties;
}

/* ── component ── */
export default function UrlInput({
  label = "Website URL",
  placeholder = "example.com",
  defaultValue = "",
  hint,
  onChange,
  sound = true,
  style,
}: UrlInputProps) {
  const [url, setUrl] = React.useState(defaultValue);
  const [favicon, setFavicon] = React.useState<string | null>(null);
  const [faviconOk, setFaviconOk] = React.useState(false);
  const [focused, setFocused] = React.useState(false);
  const inputRef = React.useRef<HTMLInputElement>(null);

  React.useEffect(() => {
    if (!url) {
      setFavicon(null);
      setFaviconOk(false);
      return;
    }
    try {
      const parsed = new URL(url.startsWith("http") ? url : `https://${url}`);
      setFavicon(`${parsed.origin}/favicon.ico`);
      setFaviconOk(false);
    } catch {
      setFavicon(null);
      setFaviconOk(false);
    }
  }, [url]);

  const handleFocus = () => {
    setFocused(true);
    if (sound) tick();
  };

  return (
    <>
      <style dangerouslySetInnerHTML={{ __html: CSS }} />
      <motion.div
        className="ul"
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
              color: "var(--ul-dim)",
              userSelect: "none" as const,
              paddingLeft: 4,
            }}
          >
            {label}
          </span>
        )}

        {/* glass field */}
        <div
          style={{
            display: "flex",
            alignItems: "center",
            gap: 12,
            padding: "0 14px",
            borderRadius: 12,
            background: "var(--ul-glass)",
            border: `1px solid ${focused ? "var(--ul-focus)" : "var(--ul-border)"}`,
            boxShadow: "var(--ul-shadow)",
            backdropFilter: "blur(24px)",
            WebkitBackdropFilter: "blur(24px)",
            transition: "border-color 0.2s",
            cursor: "text",
          }}
          onClick={() => inputRef.current?.focus()}
        >
          {/* favicon / globe */}
          <div
            style={{
              width: 26,
              height: 26,
              borderRadius: 6,
              background: "var(--ul-icon-bg)",
              border: "1px solid var(--ul-icon-border)",
              overflow: "hidden",
              flexShrink: 0,
              display: "flex",
              alignItems: "center",
              justifyContent: "center",
            }}
          >
            {favicon ? (
              <img
                src={favicon}
                alt=""
                width={18}
                height={18}
                style={{
                  width: 18,
                  height: 18,
                  borderRadius: 3,
                  opacity: faviconOk ? 1 : 0,
                  transition: "opacity 0.3s",
                }}
                onLoad={() => setFaviconOk(true)}
                onError={() => {
                  setFavicon(null);
                  setFaviconOk(false);
                }}
              />
            ) : null}
            {(!favicon || !faviconOk) && (
              <svg
                width={14}
                height={14}
                viewBox="0 0 16 16"
                fill="none"
                stroke="var(--ul-dim)"
                strokeWidth={1.5}
                strokeLinecap="round"
                strokeLinejoin="round"
              >
                <circle cx="8" cy="8" r="6" />
                <path d="M2 8h12" />
                <path d="M8 2c2 2 3 4 3 6s-1 4-3 6" />
                <path d="M8 2c-2 2-3 4-3 6s1 4 3 6" />
              </svg>
            )}
          </div>

          {/* protocol hint */}
          <span
            style={{
              fontSize: 13,
              color: "var(--ul-dim)",
              userSelect: "none" as const,
              flexShrink: 0,
            }}
          >
            https://
          </span>

          {/* input */}
          <input
            ref={inputRef}
            type="text"
            value={url}
            onChange={(e) => {
              setUrl(e.target.value);
              onChange?.(e.target.value);
            }}
            onFocus={handleFocus}
            onBlur={() => setFocused(false)}
            placeholder={placeholder}
            style={{
              flex: 1,
              height: 48,
              background: "transparent",
              border: "none",
              outline: "none",
              fontSize: 14,
              color: "var(--ul-hi)",
              minWidth: 0,
            }}
          />
        </div>

        {hint && (
          <span
            style={{
              fontSize: 11,
              color: "var(--ul-dim)",
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
import UrlInput from "@/components/ui/url-input";

export default function DemoOne() {
  return <UrlInput />;
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add input label
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
