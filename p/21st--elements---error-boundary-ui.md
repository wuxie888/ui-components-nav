<!-- Error Boundary UI · @elements- · https://21st.dev/@elements-/components/error-boundary-ui
     license: no-license · category: accordion
     An error display component for React error boundaries showing the message, parsed stack trace, component stack, with retry and copy-error actions. -->

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
components/ui/error-boundary-ui.tsx
"use client";

import * as React from "react";

import {
  AlertTriangle,
  ChevronDown,
  ChevronUp,
  Copy,
  RefreshCw,
} from "lucide-react";

import { cn } from "@/lib/utils";

interface ErrorBoundaryUiProps {
  error: Error;
  resetError?: () => void;
  componentStack?: string | null;
  isDev?: boolean;
  className?: string;
}

function parseStackTrace(
  stack: string,
): { file: string; line: string; column: string; fn: string }[] {
  const lines = stack.split("\n").slice(1);
  return lines
    .map((line) => {
      const match =
        line.match(/at\s+(.+?)\s+\((.+?):(\d+):(\d+)\)/) ||
        line.match(/at\s+(.+?):(\d+):(\d+)/);
      if (match) {
        if (match.length === 5) {
          return {
            fn: match[1],
            file: match[2],
            line: match[3],
            column: match[4],
          };
        }
        return {
          fn: "anonymous",
          file: match[1],
          line: match[2],
          column: match[3],
        };
      }
      return null;
    })
    .filter((x): x is NonNullable<typeof x> => x !== null);
}

export function ErrorBoundaryUi({
  error,
  resetError,
  componentStack,
  isDev = process.env.NODE_ENV === "development",
  className,
}: ErrorBoundaryUiProps) {
  const [showStack, setShowStack] = React.useState(isDev);
  const [showComponentStack, setShowComponentStack] = React.useState(false);
  const [copied, setCopied] = React.useState(false);

  const stackFrames = React.useMemo(
    () => (error.stack ? parseStackTrace(error.stack) : []),
    [error.stack],
  );

  const handleCopy = React.useCallback(async () => {
    const errorText = [
      `Error: ${error.message}`,
      "",
      "Stack Trace:",
      error.stack,
      componentStack ? `\nComponent Stack:\n${componentStack}` : "",
    ].join("\n");

    await navigator.clipboard.writeText(errorText);
    setCopied(true);
    setTimeout(() => setCopied(false), 1500);
  }, [error.message, error.stack, componentStack]);

  const handleToggleStack = React.useCallback(() => {
    setShowStack((prev) => !prev);
  }, []);

  const handleToggleComponentStack = React.useCallback(() => {
    setShowComponentStack((prev) => !prev);
  }, []);

  return (
    <div
      data-slot="error-boundary-ui"
      role="alert"
      aria-live="assertive"
      aria-atomic="true"
      className={cn(
        "rounded-lg border border-red-200 dark:border-red-900 bg-red-50 dark:bg-red-950/30 overflow-hidden",
        className,
      )}
    >
      <div className="flex items-start gap-3 p-4">
        <div className="shrink-0 mt-0.5">
          <AlertTriangle className="w-5 h-5 text-red-500" />
        </div>
        <div className="flex-1 min-w-0">
          <h3 className="font-semibold text-red-700 dark:text-red-300">
            {isDev ? error.name || "Error" : "Something went wrong"}
          </h3>
          <p className="mt-1 text-sm text-red-600 dark:text-red-400 break-words">
            {isDev
              ? error.message
              : "An unexpected error occurred. Please try again."}
          </p>
        </div>
      </div>

      <div className="flex items-center gap-2 px-4 pb-4">
        {resetError && (
          <button
            type="button"
            onClick={resetError}
            aria-label="Try again"
            className="flex items-center gap-1.5 px-3 py-1.5 text-sm font-medium bg-red-100 dark:bg-red-900/50 text-red-700 dark:text-red-300 rounded hover:bg-red-200 dark:hover:bg-red-900 transition-colors"
          >
            <RefreshCw className="w-3.5 h-3.5" />
            Try again
          </button>
        )}
        <button
          type="button"
          onClick={handleCopy}
          aria-label={copied ? "Copied to clipboard" : "Copy error details"}
          className="flex items-center gap-1.5 px-3 py-1.5 text-sm font-medium bg-red-100 dark:bg-red-900/50 text-red-700 dark:text-red-300 rounded hover:bg-red-200 dark:hover:bg-red-900 transition-colors"
        >
          <Copy className="w-3.5 h-3.5" />
          {copied ? "Copied!" : "Copy error"}
        </button>
      </div>

      {isDev && error.stack && (
        <div className="border-t border-red-200 dark:border-red-900">
          <button
            type="button"
            onClick={handleToggleStack}
            aria-expanded={showStack}
            aria-controls="stack-trace-content"
            aria-label="Toggle stack trace"
            className="w-full flex items-center justify-between px-4 py-2 text-sm text-red-600 dark:text-red-400 hover:bg-red-100 dark:hover:bg-red-900/30 transition-colors"
          >
            <span className="font-medium">Stack Trace</span>
            {showStack ? (
              <ChevronUp className="w-4 h-4" />
            ) : (
              <ChevronDown className="w-4 h-4" />
            )}
          </button>
          {showStack && (
            <div
              id="stack-trace-content"
              className="px-4 pb-4 overflow-auto"
              aria-label="Error stack trace"
            >
              <div className="font-mono text-xs space-y-1">
                {stackFrames.map((frame, idx) => (
                  <div
                    key={idx}
                    className="flex gap-2 text-red-600 dark:text-red-400"
                  >
                    <span className="text-red-400 dark:text-red-600 shrink-0">
                      at
                    </span>
                    <span className="text-red-700 dark:text-red-300">
                      {frame.fn}
                    </span>
                    <span className="text-red-500 truncate">
                      ({frame.file}:{frame.line}:{frame.column})
                    </span>
                  </div>
                ))}
              </div>
            </div>
          )}
        </div>
      )}

      {isDev && componentStack && (
        <div className="border-t border-red-200 dark:border-red-900">
          <button
            type="button"
            onClick={handleToggleComponentStack}
            aria-expanded={showComponentStack}
            aria-controls="component-stack-content"
            aria-label="Toggle component stack"
            className="w-full flex items-center justify-between px-4 py-2 text-sm text-red-600 dark:text-red-400 hover:bg-red-100 dark:hover:bg-red-900/30 transition-colors"
          >
            <span className="font-medium">Component Stack</span>
            {showComponentStack ? (
              <ChevronUp className="w-4 h-4" />
            ) : (
              <ChevronDown className="w-4 h-4" />
            )}
          </button>
          {showComponentStack && (
            <div
              id="component-stack-content"
              className="px-4 pb-4 overflow-auto"
              aria-label="Component stack trace"
            >
              <pre className="font-mono text-xs text-red-600 dark:text-red-400 whitespace-pre-wrap">
                {componentStack}
              </pre>
            </div>
          )}
        </div>
      )}
    </div>
  );
}

export type { ErrorBoundaryUiProps };

demo.tsx
"use client";

import { ErrorBoundaryUi } from "@/components/ui/error-boundary-ui";

function makeSampleError() {
  const error = new Error("Cannot read properties of undefined (reading 'map')");
  error.name = "TypeError";
  error.stack = `TypeError: Cannot read properties of undefined (reading 'map')
    at ProductList (webpack-internal:///./src/components/ProductList.tsx:24:18)
    at renderWithHooks (webpack-internal:///./node_modules/react-dom/cjs/react-dom.development.js:16305:18)
    at mountIndeterminateComponent (webpack-internal:///./node_modules/react-dom/cjs/react-dom.development.js:20074:13)`;
  return error;
}

export default function Default() {
  const error = makeSampleError();
  const componentStack = `
    at ProductList (src/components/ProductList.tsx:24:18)
    at Page (src/app/products/page.tsx:12:9)`;

  return (
    <div className="w-full max-w-xl mx-auto p-6">
      <ErrorBoundaryUi
        error={error}
        isDev
        componentStack={componentStack}
        resetError={() => window.location.reload()}
      />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
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
