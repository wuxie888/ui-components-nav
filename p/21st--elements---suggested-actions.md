<!-- AI Suggested Actions · @elements- · https://21st.dev/@elements-/components/suggested-actions
     license: MIT · category: grid
     Responsive grid of suggestion buttons that display quick chat prompts users can click to start or steer a conversation. -->

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
components/ui/ai-suggested-actions.tsx
"use client";

import * as React from "react";

import { cn } from "@/lib/utils";

interface Suggestion {
  label: string;
  prompt: string;
}

interface AiSuggestedActionsProps {
  suggestions: Suggestion[];
  onSelect?: (prompt: string) => void;
  className?: string;
}

function AiSuggestedActions({
  suggestions,
  onSelect,
  className,
}: AiSuggestedActionsProps) {
  return (
    <div
      data-slot="ai-suggested-actions"
      className={cn("grid gap-2 font-mono sm:grid-cols-2", className)}
    >
      {suggestions.map((suggestion, index) => (
        <button
          type="button"
          key={suggestion.prompt}
          onClick={() => onSelect?.(suggestion.prompt)}
          className="border bg-background p-3 text-left text-xs transition-colors hover:bg-muted"
          style={{
            animationDelay: `${index * 50}ms`,
          }}
        >
          {suggestion.label}
        </button>
      ))}
    </div>
  );
}

export { AiSuggestedActions };
export type { AiSuggestedActionsProps, Suggestion };

demo.tsx
"use client";

import * as React from "react";

import { AiSuggestedActions } from "@/components/ui/suggested-actions";

const suggestions = [
  {
    label: "Explain this code",
    prompt: "Can you explain how this code works?",
  },
  { label: "Fix the bug", prompt: "There's a bug in my code. Can you help?" },
  { label: "Write tests", prompt: "Can you write unit tests for this?" },
  { label: "Optimize performance", prompt: "How can I optimize this code?" },
];

export default function Demo() {
  const [selected, setSelected] = React.useState<string | null>(null);

  return (
    <div className="mx-auto w-full max-w-md rounded-xl border bg-card p-5 text-card-foreground shadow-sm">
      <div className="mb-4 space-y-1">
        <h3 className="text-sm font-medium">How can I help you today?</h3>
        <p className="text-xs text-muted-foreground">
          Pick a suggestion to get started.
        </p>
      </div>

      <AiSuggestedActions suggestions={suggestions} onSelect={setSelected} />

      <p className="mt-4 min-h-5 text-xs text-muted-foreground">
        {selected ? (
          <>
            Sending: <span className="text-foreground">{selected}</span>
          </>
        ) : (
          "No prompt selected yet."
        )}
      </p>
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
