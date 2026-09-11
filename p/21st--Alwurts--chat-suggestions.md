<!-- Chat Suggestions · @Alwurts · https://21st.dev/@Alwurts/components/chat-suggestions
     license: MIT · category: onboarding
     A composable set of components for displaying chat prompt suggestions as clickable buttons, ideal for onboarding or empty chat states. -->

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
components/ui/chat-suggestions.tsx
"use client";

import type { ComponentProps } from "react";
import { Button } from "@/components/ui/button";
import { cn } from "@/lib/utils";

function ChatSuggestions({ className, ...props }: ComponentProps<"div">) {
	return (
		<div
			className={cn("flex flex-col gap-2.5 px-2 py-2", className)}
			{...props}
		/>
	);
}

function ChatSuggestionsHeader({ className, ...props }: ComponentProps<"div">) {
	return <div className={cn("flex flex-col gap-1", className)} {...props} />;
}

function ChatSuggestionsTitle({ className, ...props }: ComponentProps<"p">) {
	return (
		<p
			className={cn(
				"text-sm font-medium text-muted-foreground",
				className,
			)}
			{...props}
		/>
	);
}

function ChatSuggestionsDescription({
	className,
	...props
}: ComponentProps<"p">) {
	return (
		<p
			className={cn("text-xs text-muted-foreground/80", className)}
			{...props}
		/>
	);
}

function ChatSuggestionsContent({
	className,
	...props
}: ComponentProps<"div">) {
	return <div className={cn("flex flex-wrap gap-2", className)} {...props} />;
}

function ChatSuggestion({
	className,
	...props
}: ComponentProps<typeof Button>) {
	return (
		<Button
			variant="outline"
			size="default"
			className={cn(
				"h-auto py-2 px-3 text-sm font-normal whitespace-normal text-left justify-start",
				"hover:bg-accent/50 hover:text-accent-foreground",
				"transition-colors",
				className,
			)}
			{...props}
		/>
	);
}

export {
	ChatSuggestions,
	ChatSuggestionsHeader,
	ChatSuggestionsTitle,
	ChatSuggestionsDescription,
	ChatSuggestionsContent,
	ChatSuggestion,
};

demo.tsx
"use client";

import { ChatSuggestion, ChatSuggestions, ChatSuggestionsContent, ChatSuggestionsDescription, ChatSuggestionsHeader, ChatSuggestionsTitle } from "@/components/ui/chat-suggestions";
import { useState } from "react";

export default function ChatSuggestionsDemo() {
	const [selectedSuggestion, setSelectedSuggestion] = useState<string>("");

	const suggestions = [
		"Tell me about your features",
		"How do I get started?",
		"Show me an example",
		"What are the pricing options?",
	];

	return (
		<div className="flex flex-col gap-4 p-4">
			<ChatSuggestions>
				<ChatSuggestionsHeader>
					<ChatSuggestionsTitle>
						Try these prompts:
					</ChatSuggestionsTitle>
					<ChatSuggestionsDescription>
						Click a suggestion to get started
					</ChatSuggestionsDescription>
				</ChatSuggestionsHeader>
				<ChatSuggestionsContent>
					{suggestions.map((suggestion) => (
						<ChatSuggestion
							key={suggestion}
							onClick={() => setSelectedSuggestion(suggestion)}
						>
							{suggestion}
						</ChatSuggestion>
					))}
				</ChatSuggestionsContent>
			</ChatSuggestions>

			{selectedSuggestion && (
				<div className="p-3 bg-muted rounded-md text-sm">
					<span className="font-medium">Selected:</span>{" "}
					{selectedSuggestion}
				</div>
			)}
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
