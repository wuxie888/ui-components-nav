<!-- Inline Validation · @ddoemonn · https://21st.dev/@ddoemonn/components/inline-validation
     license: MIT · category: form
     A controlled text input that validates as you type, crossfading between a hint, an inline error message, and valid/invalid status icons. -->

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
components/ui/inline-validation.tsx
"use client";

import { useCallback, useEffect, useId, useRef, useState } from "react";
import { motion, useReducedMotion } from "motion/react";

const CROSSFADE = { type: "spring", stiffness: 260, damping: 34, mass: 0.8 } as const;
const INSTANT = { duration: 0 } as const;

const LINE = 16;

export type ValidationStatus = "idle" | "pending" | "valid" | "invalid";

export type Validator = (value: string) => string | null;

export type UseInlineValidationOptions = {
  value: string;
  validate: Validator;
  debounce?: number;
};

export type UseInlineValidationReturn = {
  status: ValidationStatus;
  error: string | null;
  message: string;
  touched: boolean;
  commit: () => void;
  reset: () => void;
  fieldProps: {
    onBlur: () => void;
    "aria-invalid": boolean;
  };
};

type Settled = {
  status: ValidationStatus;
  error: string | null;
  message: string;
};

const CLEAN: Settled = { status: "idle", error: null, message: "" };

export function useInlineValidation({
  value,
  validate,
  debounce = 400,
}: UseInlineValidationOptions): UseInlineValidationReturn {
  const [touched, setTouched] = useState(false);
  const [settled, setSettled] = useState<Settled>(CLEAN);

  const check = useRef(validate);
  const latest = useRef(value);

  useEffect(() => {
    check.current = validate;
    latest.current = value;
  });

  useEffect(() => {
    if (!touched) return;

    const next = check.current(value);
    const resolved: ValidationStatus = value.length > 0 ? "valid" : "idle";

    if (next === null) {
      setSettled((prev) =>
        prev.status === resolved && prev.error === null
          ? prev
          : { status: resolved, error: null, message: prev.message },
      );
      return;
    }

    setSettled((prev) =>
      prev.status === "invalid"
        ? prev
        : { status: "pending", error: null, message: prev.message },
    );

    const t = setTimeout(() => {
      setSettled((prev) =>
        prev.error === next ? prev : { status: "invalid", error: next, message: next },
      );
    }, debounce);

    return () => clearTimeout(t);
  }, [value, touched, debounce]);

  const commit = useCallback(() => {
    setTouched(true);
    const v = latest.current;
    const next = check.current(v);
    setSettled((prev) =>
      next === null
        ? { status: v.length > 0 ? "valid" : "idle", error: null, message: prev.message }
        : { status: "invalid", error: next, message: next },
    );
  }, []);

  const reset = useCallback(() => {
    setTouched(false);
    setSettled(CLEAN);
  }, []);

  return {
    status: settled.status,
    error: settled.error,
    message: settled.message,
    touched,
    commit,
    reset,
    fieldProps: { onBlur: commit, "aria-invalid": settled.status === "invalid" },
  };
}

export type InlineValidationProps = {
  label: string;
  value: string;
  onChange: (value: string) => void;
  validate: Validator;
  hint?: string;
  id?: string;
  name?: string;
  type?: "text" | "email" | "password" | "tel" | "url" | "search";
  placeholder?: string;
  autoComplete?: string;
  inputMode?: React.ComponentProps<"input">["inputMode"];
  debounce?: number;
  reserveLines?: number;
  disabled?: boolean;
  required?: boolean;
  className?: string;
};

export function InlineValidation({
  label,
  value,
  onChange,
  validate,
  hint,
  id,
  name,
  type = "text",
  placeholder,
  autoComplete,
  inputMode,
  debounce = 400,
  reserveLines = 1,
  disabled = false,
  required = false,
  className = "",
}: InlineValidationProps) {
  const reduced = useReducedMotion();
  const fade = reduced ? INSTANT : CROSSFADE;

  const auto = useId();
  const fieldId = id ?? `${auto}-field`;
  const hintId = `${auto}-hint`;
  const errorId = `${auto}-error`;

  const { status, error, message, fieldProps } = useInlineValidation({
    value,
    validate,
    debounce,
  });

  const invalid = status === "invalid";
  const valid = status === "valid";

  const described = [hint ? hintId : null, invalid ? errorId : null]
    .filter(Boolean)
    .join(" ");

  const clamp = {
    display: "-webkit-box" as const,
    WebkitBoxOrient: "vertical" as const,
    WebkitLineClamp: reserveLines,
    overflow: "hidden" as const,
  };

  return (
    <div className={`w-full ${className}`}>
      <label
        htmlFor={fieldId}
        className="block text-[13px] font-medium text-stone-700 dark:text-stone-200"
      >
        {label}
      </label>

      <div className="relative mt-1.5">
        <input
          id={fieldId}
          name={name}
          type={type}
          value={value}
          placeholder={placeholder}
          autoComplete={autoComplete}
          inputMode={inputMode}
          disabled={disabled}
          required={required}
          aria-required={required || undefined}
          aria-describedby={described || undefined}
          onChange={(e) => onChange(e.target.value)}
          {...fieldProps}
          className={`h-10 w-full rounded-[10px] border-2 pl-3 pr-9 text-[13px] text-stone-700 outline-none transition-[background-color,border-color,box-shadow] duration-150 placeholder:text-stone-400 focus-visible:outline-none disabled:opacity-50 dark:text-stone-200 dark:placeholder:text-stone-500 ${
            invalid
              ? "border-red-500 bg-white dark:border-red-400 dark:bg-[#1D1D1A]"
              : "border-stone-200 bg-stone-100/70 shadow-[inset_0_1px_2px_rgba(28,25,23,0.07)] focus:border-[#4568FF] focus:bg-white focus:shadow-none dark:border-white/[0.08] dark:bg-[#1D1D1A] dark:shadow-[inset_0_1px_2px_rgba(0,0,0,0.45)] dark:focus:border-[#93B0FF] dark:focus:bg-[#252522]"
          }`}
        />

        <span
          className="pointer-events-none absolute right-3 top-1/2 grid size-3.5 -translate-y-1/2 place-items-center"
          aria-hidden
        >
          <motion.svg
            viewBox="0 0 12 12"
            width="14"
            height="14"
            fill="none"
            className="col-start-1 row-start-1 text-stone-500 dark:text-stone-400"
            initial={false}
            animate={{ opacity: valid ? 1 : 0, scale: valid ? 1 : 0.7 }}
            transition={fade}
          >
            <path
              d="M2 6.3 4.7 9 10 3.2"
              stroke="currentColor"
              strokeWidth="1.6"
              strokeLinecap="round"
              strokeLinejoin="round"
            />
          </motion.svg>
          <motion.svg
            viewBox="0 0 12 12"
            width="14"
            height="14"
            fill="none"
            className="col-start-1 row-start-1 text-red-700 dark:text-red-400"
            initial={false}
            animate={{ opacity: invalid ? 1 : 0, scale: invalid ? 1 : 0.7 }}
            transition={fade}
          >
            <path d="M6 2v4.4" stroke="currentColor" strokeWidth="1.7" strokeLinecap="round" />
            <rect x="5.15" y="8.4" width="1.7" height="1.7" rx="0.5" fill="currentColor" />
          </motion.svg>
        </span>
      </div>

      <div className="relative mt-1.5 grid" style={{ height: reserveLines * LINE }}>
        {hint ? (
          <motion.p
            aria-hidden
            style={clamp}
            className="col-start-1 row-start-1 text-[11.5px] leading-[16px] text-stone-500 dark:text-stone-400"
            initial={false}
            animate={{ opacity: invalid ? 0 : 1, y: invalid ? 3 : 0 }}
            transition={fade}
          >
            {hint}
          </motion.p>
        ) : null}

        <motion.p
          aria-hidden
          style={clamp}
          className="col-start-1 row-start-1 text-[11.5px] leading-[16px] text-red-700 dark:text-red-400"
          initial={false}
          animate={{ opacity: invalid ? 1 : 0, y: invalid ? 0 : -3 }}
          transition={fade}
        >
          {error ?? message}
        </motion.p>

        {hint ? (
          <span id={hintId} className="sr-only">
            {hint}
          </span>
        ) : null}

        <span
          id={errorId}
          role="status"
          aria-live="polite"
          aria-atomic="true"
          className="sr-only"
        >
          {error ?? ""}
        </span>
      </div>
    </div>
  );
}

demo.tsx
"use client";

import { InlineValidation } from "@/components/ui/inline-validation";
import { useState } from "react";

const checkEmail = (v: string) => {
  if (v.trim() === "") return "A work email is required.";
  if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(v))
    return "That is not a complete email address.";
  return null;
};

export default function InviteFormDemo() {
  const [email, setEmail] = useState("");

  return (
    <div className="flex min-h-[280px] w-full items-center justify-center p-6">
      <form
        className="w-full max-w-sm"
        onSubmit={(e) => {
          e.preventDefault();
          if (checkEmail(email)) return;
          alert(`Invite sent to ${email}`);
        }}
      >
        <InlineValidation
          label="Work email"
          type="email"
          value={email}
          onChange={setEmail}
          validate={checkEmail}
          hint="Only used to send the invite."
          autoComplete="email"
          required
        />

        <button
          type="submit"
          className="mt-3 h-9 rounded-[9px] bg-stone-800 px-3 text-[13px] font-medium text-white dark:bg-stone-100 dark:text-stone-900"
        >
          Send invite
        </button>
      </form>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install motion
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
