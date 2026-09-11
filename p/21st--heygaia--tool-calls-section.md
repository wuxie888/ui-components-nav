<!-- Tool Calls Section · @heygaia · https://21st.dev/@heygaia/components/tool-calls-section
     license: no-license · category: timeline
     An expandable section that shows AI agent tool usage as a collapsible timeline with stacked category icons and per-call inputs and outputs. -->

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
components/ui/tool-calls-section.tsx
"use client";

import type { ReactNode } from "react";
import { useMemo, useState } from "react";

import { HugeiconsIcon, ArrowDown01Icon, ToolsIcon } from "@/components/icons";
import { cn } from "@/lib/utils";
import { formatToolName, getToolCategoryIcon } from "@/lib/utils/tool-icons";
import { CompactMarkdown } from "@/registry/new-york/ui/compact-markdown";

// ============================================================================
// Types
// ============================================================================

export interface ToolCallEntry {
	/** Name of the tool that was called */
	tool_name: string;
	/** Category/integration the tool belongs to (e.g., "gmail", "search", "memory") */
	tool_category: string;
	/** Human-readable message describing what the tool did */
	message?: string;
	/** Whether to show the category label (default: true) */
	show_category?: boolean;
	/** Unique ID for this tool call */
	tool_call_id?: string;
	/** Input parameters passed to the tool */
	inputs?: Record<string, unknown>;
	/** Output/result from the tool */
	output?: string;
	/** URL to custom icon for integrations */
	icon_url?: string;
	/** Friendly name for the integration (e.g., "Linear", "Slack") */
	integration_name?: string;
}

export interface IntegrationInfo {
	iconUrl?: string;
	name?: string;
}

export interface ToolCallsSectionProps {
	/** Array of tool call entries to display */
	toolCalls: ToolCallEntry[];
	/** Optional map of integration IDs to their info for icon/name lookup */
	integrations?: Map<string, IntegrationInfo>;
	/** Maximum number of icons to show in the stacked display (default: 10) */
	maxIconsToShow?: number;
	/** Whether to start with the accordion expanded (default: false) */
	defaultExpanded?: boolean;
	/** Custom class name for the container */
	className?: string;
	/** Custom icon size (default: 21) */
	iconSize?: number;
	/** Custom icon renderer override */
	renderIcon?: (call: ToolCallEntry, size: number) => ReactNode;
	/** Custom content renderer override for inputs/outputs */
	renderContent?: (content: unknown) => ReactNode;
}

// ============================================================================
// Helper Components
// ============================================================================

interface ChevronIconProps {
	isExpanded: boolean;
	size?: number;
	className?: string;
}

function ChevronIcon({ isExpanded, size = 18, className = "" }: ChevronIconProps) {
	return (
		<HugeiconsIcon
			icon={ArrowDown01Icon}
			size={size}
			className={cn(
				"transition-transform duration-200",
				isExpanded && "rotate-180",
				className,
			)}
		/>
	);
}

// ============================================================================
// Main Component
// ============================================================================

export function ToolCallsSection({
	toolCalls,
	integrations,
	maxIconsToShow = 10,
	defaultExpanded = false,
	className,
	iconSize = 21,
	renderIcon,
	renderContent,
}: ToolCallsSectionProps) {
	const [isExpanded, setIsExpanded] = useState(defaultExpanded);
	const [expandedCalls, setExpandedCalls] = useState<Set<number>>(new Set());

	// Create a lookup map for custom integrations by id
	const integrationLookup = useMemo(() => {
		if (integrations) return integrations;
		return new Map<string, IntegrationInfo>();
	}, [integrations]);

	// Helper to get icon_url with fallback to integrations lookup
	const getIconUrl = (call: ToolCallEntry): string | undefined => {
		if (call.icon_url) return call.icon_url;
		const integration = integrationLookup.get(call.tool_category);
		return integration?.iconUrl;
	};

	// Helper to get integration_name with fallback to integrations lookup
	const getIntegrationName = (call: ToolCallEntry): string | undefined => {
		if (call.integration_name) return call.integration_name;
		const integration = integrationLookup.get(call.tool_category);
		return integration?.name;
	};

	const toggleCallExpansion = (index: number) => {
		setExpandedCalls((prev) => {
			const next = new Set(prev);
			if (next.has(index)) next.delete(index);
			else next.add(index);
			return next;
		});
	};

	if (toolCalls.length === 0) return null;

	// Default icon renderer
	const defaultRenderIcon = (call: ToolCallEntry, size: number) => {
		const icon = getToolCategoryIcon(
			call.tool_category || "general",
			{ width: size, height: size },
			getIconUrl(call),
		);
		return icon || (
			<div className="p-1 min-w-8 min-h-8 bg-zinc-200 dark:bg-zinc-800 rounded-lg text-zinc-600 dark:text-zinc-400 backdrop-blur">
				<HugeiconsIcon icon={ToolsIcon} size={size} />
			</div>
		);
	};

	const iconRenderer = renderIcon || defaultRenderIcon;

	// Default content renderer
	const defaultRenderContent = (content: unknown) => (
		<CompactMarkdown content={content} />
	);

	const contentRenderer = renderContent || defaultRenderContent;

	// Render stacked rotated icons (deduplicated by category for cleaner display)
	const renderStackedIcons = () => {
		const seenCategories = new Set<string>();
		const uniqueIcons = toolCalls.filter((call) => {
			const category = call.tool_category || "general";
			if (seenCategories.has(category)) return false;
			seenCategories.add(category);
			return true;
		});
		const displayIcons = uniqueIcons.slice(0, maxIconsToShow);

		return (
			<div className="flex min-h-8 items-center -space-x-2">
				{displayIcons.map((call, index) => (
					<div
						key={`${call.tool_name}-${index}`}
						className="relative flex min-w-8 items-center justify-center"
						style={{
							rotate:
								displayIcons.length > 1
									? index % 2 === 0
										? "8deg"
										: "-8deg"
									: "0deg",
							zIndex: index,
						}}
					>
						{iconRenderer(call, iconSize)}
					</div>
				))}
				{uniqueIcons.length > maxIconsToShow && (
					<div className="z-0 flex size-7 min-h-7 min-w-7 items-center justify-center rounded-lg bg-zinc-200 dark:bg-zinc-700/60 text-xs text-zinc-600 dark:text-zinc-500 font-normal">
						+{uniqueIcons.length - maxIconsToShow}
					</div>
				)}
			</div>
		);
	};

	return (
		<div className={cn("w-fit max-w-[35rem]", className)}>
			{/* Collapsible Header */}
			<button
				type="button"
				onClick={() => setIsExpanded(!isExpanded)}
				className="flex items-center gap-2 hover:text-zinc-900 dark:hover:text-white text-zinc-500 cursor-pointer py-2"
			>
				{renderStackedIcons()}
				<span className="text-xs font-medium transition-all duration-200">
					Used {toolCalls.length} tool
					{toolCalls.length > 1 ? "s" : ""}
				</span>
				<ChevronIcon isExpanded={isExpanded} />
			</button>

			{/* Collapsible Content */}
			<div
				className={cn(
					"overflow-hidden transition-all duration-200",
					isExpanded ? "max-h-[2000px] opacity-100" : "max-h-0 opacity-0",
				)}
			>
				<div className="space-y-0 pt-1">
					{toolCalls.map((call, index) => {
						const hasCategoryText =
							call.show_category !== false &&
							call.tool_category &&
							call.tool_category !== "unknown";
						const hasDetails = call.inputs || call.output;
						const isCallExpanded = expandedCalls.has(index);

						return (
							<div
								key={`${call.tool_name}-step-${index}`}
								className="flex items-stretch gap-2"
							>
								{/* Icon column with connector line */}
								<div className="flex flex-col items-center self-stretch">
									<div className="min-h-8 min-w-8 flex items-center justify-center shrink-0">
										{iconRenderer(call, iconSize)}
									</div>
									{index < toolCalls.length - 1 && (
										<div className="w-px flex-1 bg-zinc-300 dark:bg-zinc-700 min-h-4" />
									)}
								</div>

								{/* Content column */}
								<div className="flex-1 min-w-0">
									<button
										type="button"
										className={cn(
											"flex items-center gap-1 group/parent",
											hasDetails ? "cursor-pointer" : "",
											!hasCategoryText ? "pt-2" : "",
										)}
										onClick={() => hasDetails && toggleCallExpansion(index)}
									>
										<p
											className={cn(
												"text-xs text-zinc-600 dark:text-zinc-400 font-medium",
												hasDetails && "group-hover/parent:text-zinc-900 dark:group-hover/parent:text-white",
											)}
										>
											{call.message || formatToolName(call.tool_name)}
										</p>
										{hasDetails && (
											<ChevronIcon isExpanded={isCallExpanded} size={14} />
										)}
									</button>

									{hasCategoryText && (
										<p className="text-[11px] text-zinc-400 dark:text-zinc-500 capitalize">
											{getIntegrationName(call) ||
												call.tool_category
													.replace(/_/g, " ")
													.split(" ")
													.map(
														(word) =>
															word.charAt(0).toUpperCase() +
															word.slice(1).toLowerCase(),
													)
													.join(" ")}
										</p>
									)}

									{isCallExpanded && hasDetails && (
										<div className="mt-2 space-y-2 text-[11px] bg-zinc-100 dark:bg-zinc-800/50 rounded-xl p-3 mb-3 w-fit">
											{call.inputs && Object.keys(call.inputs).length > 0 && (
												<div className="flex flex-col">
													<span className="text-zinc-400 dark:text-zinc-500 font-medium mb-1">
														Input
													</span>
													{contentRenderer(call.inputs)}
												</div>
											)}
											{call.output && (
												<div className="flex flex-col">
													<span className="text-zinc-400 dark:text-zinc-500 font-medium mb-1">
														Output
													</span>
													{contentRenderer(call.output)}
												</div>
											)}
										</div>
									)}
								</div>
							</div>
						);
					})}
				</div>
			</div>
		</div>
	);
}

export default ToolCallsSection;

components/ui/compact-markdown.tsx
"use client";

import type { ReactNode } from "react";

export interface CompactMarkdownProps {
	content: unknown;
	className?: string;
}

/**
 * Helper to check if a value looks like structured data (object/array/JSON)
 */
const isStructuredData = (value: unknown): boolean => {
	if (typeof value === "object" && value !== null) return true;
	if (typeof value === "string") {
		const trimmed = value.trim();
		return (
			(trimmed.startsWith("{") && trimmed.endsWith("}")) ||
			(trimmed.startsWith("[") && trimmed.endsWith("]"))
		);
	}
	return false;
};

/**
 * Try to parse and format JSON-like strings
 */
const formatJsonLikeString = (str: string): string => {
	try {
		const parsed = JSON.parse(str);
		return JSON.stringify(parsed, null, 2);
	} catch {
		// If it looks like truncated JSON, try to format it anyway
		return str;
	}
};

/**
 * Normalize content for display
 */
const normalizeValue = (content: unknown): { data: unknown; isStructured: boolean } => {
	if (typeof content === "object" && content !== null) {
		return { data: content, isStructured: true };
	}
	if (typeof content === "string") {
		const trimmed = content.trim();
		const looksLikeJson =
			(trimmed.startsWith("{") && (trimmed.endsWith("}") || trimmed.includes("}"))) ||
			(trimmed.startsWith("[") && (trimmed.endsWith("]") || trimmed.includes("]")));
		if (looksLikeJson) {
			return { data: content, isStructured: true };
		}
		return { data: content, isStructured: false };
	}
	return { data: String(content), isStructured: false };
};

/**
 * Compact display for structured data and markdown content.
 * Accepts any value and automatically formats it appropriately:
 * - Objects/Arrays: Pretty-printed JSON
 * - Strings that look like JSON: Formatted with indentation (even if truncated)
 * - Other strings: Simple text rendering or markdown if react-markdown available
 */
export function CompactMarkdown({ content, className = "" }: CompactMarkdownProps) {
	const { data, isStructured } = normalizeValue(content);

	const baseClasses = "bg-zinc-900/50 rounded-xl p-3 max-h-60 overflow-y-auto text-xs text-zinc-400 w-fit max-w-[32rem]";

	// For structured data, render as preformatted text
	if (isStructured) {
		const displayText =
			typeof data === "string"
				? formatJsonLikeString(data)
				: JSON.stringify(data, null, 2);

		return (
			<pre className={`${baseClasses} whitespace-pre-wrap break-words ${className}`}>
				{displayText}
			</pre>
		);
	}

	// For text content, render with basic formatting
	const textContent = String(data);

	return (
		<div className={`${baseClasses} leading-relaxed ${className}`}>
			{textContent}
		</div>
	);
}

export default CompactMarkdown;

lib/utils/tool-icons.tsx
import type { IconSvgElement } from "@hugeicons/react";
import Image from "next/image";
import {
	Brain02Icon,
	CheckListIcon,
	AlarmClockIcon,
	ComputerProgramming01Icon,
	File02Icon,
	HugeiconsIcon,
	Image02Icon,
	InformationCircleIcon,
	Link01Icon,
	Notification03Icon,
	PackageOpenIcon,
	SourceCodeCircleIcon,
	SquareArrowUpRight02Icon,
	Target02Icon,
	ToolsIcon,
} from "@/components/icons";

export interface IconProps {
	size?: number;
	width?: number;
	height?: number;
	strokeWidth?: number;
	className?: string;
	color?: string;
}

// Category-specific icons with colors
export interface IconConfig {
	icon: IconSvgElement | string;
	bgColor: string;
	bgColorLight?: string; // Light mode background
	iconColor: string;
	isImage?: boolean;
}

/**
 * Normalize a category/integration name for icon lookup
 */
const normalizeCategoryName = (name: string): string => {
	if (!name) return "general";
	return name
		.toLowerCase()
		.trim()
		.replace(/[\s-]+/g, "_")
		.replace(/_+/g, "_")
		.replace(/^_|_$/g, "");
};

// Alias mapping for backwards compatibility
const iconAliases: Record<string, string> = {
	calendar: "google_calendar",
};

// Tool category icon configs - matches gaia repo pattern
const iconConfigs: Record<string, IconConfig> = {
	// Integration icons (use images)
	gmail: {
		icon: "/images/icons/gmail.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	google_calendar: {
		icon: "/images/icons/googlecalendar.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	github: {
		icon: "/images/icons/github.png",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	linear: {
		icon: "/images/icons/linear.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	slack: {
		icon: "/images/icons/slack.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	google_docs: {
		icon: "/images/icons/google_docs.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	googlesheets: {
		icon: "/images/icons/googlesheets.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	search: {
		icon: "/images/icons/google.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	weather: {
		icon: "/images/icons/weather.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	notion: {
		icon: "/images/icons/notion.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	twitter: {
		icon: "/images/icons/twitter.webp",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	linkedin: {
		icon: "/images/icons/linkedin.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	reddit: {
		icon: "/images/icons/reddit.svg",
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
		isImage: true,
	},
	figma: {
		icon: "/images/icons/figma.svg",
		bgColor: "bg-zinc-800",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-white",
		isImage: true,
	},
	
	// Category icons (use HugeIcons components)
	todos: {
		icon: CheckListIcon,
		bgColor: "bg-emerald-500/20 backdrop-blur",
		bgColorLight: "bg-emerald-500/20",
		iconColor: "text-emerald-400",
	},
	reminders: {
		icon: AlarmClockIcon,
		bgColor: "bg-sky-500/20 backdrop-blur",
		bgColorLight: "bg-sky-500/20",
		iconColor: "text-blue-400",
	},
	documents: {
		icon: File02Icon,
		bgColor: "bg-orange-500/20 backdrop-blur",
		bgColorLight: "bg-orange-500/20",
		iconColor: "text-orange-400",
	},
	development: {
		icon: SourceCodeCircleIcon,
		bgColor: "bg-sky-500/20 backdrop-blur",
		bgColorLight: "bg-sky-500/20",
		iconColor: "text-cyan-400",
	},
	memory: {
		icon: Brain02Icon,
		bgColor: "bg-indigo-500/20 backdrop-blur",
		bgColorLight: "bg-indigo-500/20",
		iconColor: "text-indigo-400",
	},
	creative: {
		icon: Image02Icon,
		bgColor: "bg-pink-500/20 backdrop-blur",
		bgColorLight: "bg-pink-500/20",
		iconColor: "text-pink-400",
	},
	goal_tracking: {
		icon: Target02Icon,
		bgColor: "bg-emerald-500/20 backdrop-blur",
		bgColorLight: "bg-emerald-500/20",
		iconColor: "text-emerald-400",
	},
	notifications: {
		icon: Notification03Icon,
		bgColor: "bg-yellow-500/20 backdrop-blur",
		bgColorLight: "bg-yellow-500/20",
		iconColor: "text-yellow-400",
	},
	support: {
		icon: InformationCircleIcon,
		bgColor: "bg-sky-500/20 backdrop-blur",
		bgColorLight: "bg-sky-500/20",
		iconColor: "text-blue-400",
	},
	general: {
		icon: InformationCircleIcon,
		bgColor: "bg-gray-500/20 backdrop-blur",
		bgColorLight: "bg-gray-500/20",
		iconColor: "text-gray-400",
	},
	integrations: {
		icon: Link01Icon,
		bgColor: "bg-zinc-700",
		bgColorLight: "bg-zinc-200",
		iconColor: "text-zinc-200",
	},
	
	// Agent tool call categories
	handoff: {
		icon: SquareArrowUpRight02Icon,
		bgColor: "bg-sky-500/20 backdrop-blur",
		bgColorLight: "bg-sky-500/20",
		iconColor: "text-sky-400",
	},
	retrieve_tools: {
		icon: PackageOpenIcon,
		bgColor: "bg-indigo-500/20 backdrop-blur",
		bgColorLight: "bg-indigo-500/20",
		iconColor: "text-indigo-400",
	},
	executor: {
		icon: ComputerProgramming01Icon,
		bgColor: "bg-teal-500/20 backdrop-blur",
		bgColorLight: "bg-teal-500/20",
		iconColor: "text-teal-400",
	},
	unknown: {
		icon: ToolsIcon,
		bgColor: "bg-zinc-500/20 backdrop-blur",
		bgColorLight: "bg-zinc-500/20",
		iconColor: "text-zinc-400",
	},
};

/**
 * Get icon for a tool category with optional URL-based icon fallback.
 * Supports built-in categories and custom integration icons via iconUrl.
 */
export const getToolCategoryIcon = (
	category: string,
	iconProps: Partial<IconProps> & { showBackground?: boolean } = {},
	iconUrl?: string | null,
) => {
	const { showBackground = true, ...restProps } = iconProps;

	const defaultProps = {
		size: restProps.size || 16,
		width: restProps.width || 20,
		height: restProps.height || 20,
		strokeWidth: restProps.strokeWidth || 2,
		className: restProps.className,
	};

	// Normalize
	const normalizedCategory = normalizeCategoryName(category);

	// Resolve aliases
	const aliasedCategory =
		iconAliases[normalizedCategory] ||
		iconAliases[category] ||
		normalizedCategory;

	const finalCategory = normalizeCategoryName(aliasedCategory);

	let config = iconConfigs[finalCategory];

	// Fallback search
	if (!config) {
		const normalizedConfigs = Object.entries(iconConfigs);
		const matchingConfig = normalizedConfigs.find(
			([key]) => normalizeCategoryName(key) === finalCategory,
		);
		if (matchingConfig) {
			config = matchingConfig[1];
		}
	}

	// If no predefined config found, try iconUrl fallback for custom integrations
	if (!config) {
		if (iconUrl) {
			const iconElement = (
				<Image
					alt={`${category} Icon`}
					width={defaultProps.width}
					height={defaultProps.height}
					className={`${restProps.className || ""} aspect-square object-contain`}
					src={iconUrl}
				/>
			);
			return showBackground ? (
				<div className="rounded-lg p-1 bg-zinc-700 dark:bg-zinc-700">{iconElement}</div>
			) : (
				iconElement
			);
		}
		return null;
	}

	// Render image or component icon
	const iconElement = config.isImage ? (
		<Image
			alt={`${category} Icon`}
			width={defaultProps.width}
			height={defaultProps.height}
			className={`${restProps.className || ""} aspect-square object-contain`}
			src={config.icon as string}
		/>
	) : (
		<HugeiconsIcon
			icon={config.icon as IconSvgElement}
			size={defaultProps.size}
			className={restProps.className || config.iconColor}
		/>
	);

	// Return with or without background based on showBackground prop
	// Using dark: prefix for proper light/dark mode support
	return showBackground ? (
		<div className={`rounded-lg p-1 ${config.bgColorLight || config.bgColor} dark:${config.bgColor}`}>
			{iconElement}
		</div>
	) : (
		iconElement
	);
};

// Format tool names from snake_case to Title Case
export const formatToolName = (name: string): string => {
	return name
		.split("_")
		.map((word) => word.charAt(0).toUpperCase() + word.slice(1).toLowerCase())
		.join(" ");
};

demo.tsx
"use client";

import { ToolCallsSection } from "@/components/ui/tool-calls-section";

const toolCalls = [
  {
    tool_name: "send_email",
    tool_category: "gmail",
    integration_name: "Gmail",
    message: "Sent email to the team",
    inputs: { to: "team@company.com", subject: "Weekly sync notes" },
    output: "Email delivered successfully to 4 recipients.",
  },
  {
    tool_name: "create_event",
    tool_category: "google_calendar",
    integration_name: "Google Calendar",
    message: "Created meeting for tomorrow at 2 PM",
    inputs: { title: "Product review", start: "2:00 PM", duration: "45m" },
  },
  {
    tool_name: "code_analysis",
    tool_category: "executor",
    message: "Executed code analysis task",
    output: "Analyzed 128 files, found 3 potential improvements.",
  },
  {
    tool_name: "scheduling_handoff",
    tool_category: "handoff",
    message: "Delegated to scheduling assistant",
  },
];

export default function Default() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center bg-background p-6">
      <ToolCallsSection toolCalls={toolCalls} defaultExpanded />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add icons
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
