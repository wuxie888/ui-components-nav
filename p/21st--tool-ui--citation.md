<!-- Citation · @tool-ui · https://21st.dev/@tool-ui/components/citation
     license: MIT · category: link
     Displays a source reference card with title, domain, favicon, author, and text snippet for citing external sources, with default, inline, and stacked list variants. -->

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
components/tool-ui/citation/_adapter.tsx
/**
 * Adapter: UI and utility re-exports for copy-standalone portability.
 *
 * When copying this component to another project, update these imports
 * to match your project's paths:
 *
 *   cn      → Your Tailwind merge utility (e.g., "@/lib/utils", "~/lib/cn")
 *   Tooltip → shadcn/ui Tooltip (only needed for variant="inline")
 *   Popover → shadcn/ui Popover (only needed for CitationList)
 */
"use client";

export { cn } from "@/lib/utils";
export {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from "@/components/ui/popover";

components/tool-ui/citation/citation-list.tsx
"use client";

import * as React from "react";
import type { LucideIcon } from "lucide-react";
import {
  FileText,
  Globe,
  Code2,
  Newspaper,
  Database,
  File,
  ExternalLink,
} from "lucide-react";
import { cn, Popover, PopoverContent, PopoverTrigger } from "./_adapter";
import { Citation } from "./citation";
import type {
  SerializableCitation,
  CitationType,
  CitationVariant,
} from "./schema";
import {
  openSafeNavigationHref,
  resolveSafeNavigationHref,
} from "../shared/media";

const TYPE_ICONS: Record<CitationType, LucideIcon> = {
  webpage: Globe,
  document: FileText,
  article: Newspaper,
  api: Database,
  code: Code2,
  other: File,
};

function useHoverPopover(delay = 100) {
  const [open, setOpen] = React.useState(false);
  const timeoutRef = React.useRef<ReturnType<typeof setTimeout> | null>(null);
  const containerRef = React.useRef<HTMLDivElement>(null);

  const handleMouseEnter = React.useCallback(() => {
    if (timeoutRef.current) clearTimeout(timeoutRef.current);
    timeoutRef.current = setTimeout(() => setOpen(true), delay);
  }, [delay]);

  const handleMouseLeave = React.useCallback(() => {
    if (timeoutRef.current) clearTimeout(timeoutRef.current);
    timeoutRef.current = setTimeout(() => setOpen(false), delay);
  }, [delay]);

  const handleFocus = React.useCallback(() => {
    if (timeoutRef.current) clearTimeout(timeoutRef.current);
    setOpen(true);
  }, []);

  const handleBlur = React.useCallback(
    (e: React.FocusEvent) => {
      const relatedTarget = e.relatedTarget as HTMLElement | null;
      if (containerRef.current?.contains(relatedTarget)) {
        return;
      }
      if (relatedTarget?.closest("[data-radix-popper-content-wrapper]")) {
        return;
      }
      if (timeoutRef.current) clearTimeout(timeoutRef.current);
      timeoutRef.current = setTimeout(() => setOpen(false), delay);
    },
    [delay],
  );

  React.useEffect(() => {
    return () => {
      if (timeoutRef.current) clearTimeout(timeoutRef.current);
    };
  }, []);

  return {
    open,
    setOpen,
    containerRef,
    handleMouseEnter,
    handleMouseLeave,
    handleFocus,
    handleBlur,
  };
}

export interface CitationListProps {
  id: string;
  citations: SerializableCitation[];
  variant?: CitationVariant;
  maxVisible?: number;
  className?: string;
  onNavigate?: (href: string, citation: SerializableCitation) => void;
}

export function CitationList(props: CitationListProps) {
  const {
    id,
    citations,
    variant = "default",
    maxVisible,
    className,
    onNavigate,
  } = props;

  const shouldTruncate =
    maxVisible !== undefined && citations.length > maxVisible;
  const visibleCitations = shouldTruncate
    ? citations.slice(0, maxVisible)
    : citations;
  const overflowCitations = shouldTruncate ? citations.slice(maxVisible) : [];
  const overflowCount = overflowCitations.length;

  const wrapperClass =
    variant === "inline"
      ? "flex flex-wrap items-center gap-1.5"
      : "flex flex-col gap-2";

  // Stacked variant: overlapping favicons with popover
  if (variant === "stacked") {
    return (
      <StackedCitations
        id={id}
        citations={citations}
        className={className}
        onNavigate={onNavigate}
      />
    );
  }

  if (variant === "default") {
    return (
      <div
        className={cn("isolate flex flex-col gap-4", className)}
        data-tool-ui-id={id}
        data-slot="citation-list"
      >
        {visibleCitations.map((citation) => (
          <Citation
            key={citation.id}
            {...citation}
            variant="default"
            onNavigate={onNavigate}
          />
        ))}
        {shouldTruncate && (
          <OverflowIndicator
            citations={overflowCitations}
            count={overflowCount}
            variant="default"
            onNavigate={onNavigate}
          />
        )}
      </div>
    );
  }

  return (
    <div
      className={cn("isolate", wrapperClass, className)}
      data-tool-ui-id={id}
      data-slot="citation-list"
    >
      {visibleCitations.map((citation) => (
        <Citation
          key={citation.id}
          {...citation}
          variant={variant}
          onNavigate={onNavigate}
        />
      ))}
      {shouldTruncate && (
        <OverflowIndicator
          citations={overflowCitations}
          count={overflowCount}
          variant={variant}
          onNavigate={onNavigate}
        />
      )}
    </div>
  );
}

interface OverflowIndicatorProps {
  citations: SerializableCitation[];
  count: number;
  variant: CitationVariant;
  onNavigate?: (href: string, citation: SerializableCitation) => void;
}

function OverflowIndicator({
  citations,
  count,
  variant,
  onNavigate,
}: OverflowIndicatorProps) {
  const { open, handleMouseEnter, handleMouseLeave } = useHoverPopover();

  const handleClick = (citation: SerializableCitation) => {
    const href = resolveSafeNavigationHref(citation.href);
    if (!href) return;
    if (onNavigate) {
      onNavigate(href, citation);
    } else {
      openSafeNavigationHref(href);
    }
  };

  const popoverContent = (
    <div className="flex max-h-72 flex-col overflow-y-auto">
      {citations.map((citation) => (
        <OverflowItem
          key={citation.id}
          citation={citation}
          onClick={() => handleClick(citation)}
        />
      ))}
    </div>
  );

  if (variant === "inline") {
    return (
      <Popover open={open}>
        <PopoverTrigger asChild>
          <button
            type="button"
            onMouseEnter={handleMouseEnter}
            onMouseLeave={handleMouseLeave}
            className={cn(
              "inline-flex items-center gap-1 rounded-md px-2 py-1",
              "bg-muted/60 text-sm tabular-nums",
              "transition-colors duration-150",
              "hover:bg-muted",
              "focus-visible:ring-ring focus-visible:ring-2 focus-visible:outline-none",
            )}
          >
            <span className="text-muted-foreground">+{count} more</span>
          </button>
        </PopoverTrigger>
        <PopoverContent
          side="top"
          align="start"
          className="w-80 p-1"
          onMouseEnter={handleMouseEnter}
          onMouseLeave={handleMouseLeave}
          onOpenAutoFocus={(e) => e.preventDefault()}
        >
          {popoverContent}
        </PopoverContent>
      </Popover>
    );
  }

  // Default variant
  return (
    <Popover open={open}>
      <PopoverTrigger asChild>
        <button
          type="button"
          onMouseEnter={handleMouseEnter}
          onMouseLeave={handleMouseLeave}
          className={cn(
            "flex items-center justify-center rounded-xl px-4 py-3",
            "border-border bg-card border border-dashed",
            "transition-colors duration-150",
            "hover:border-foreground/25 hover:bg-muted/50",
            "focus-visible:ring-ring focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:outline-none",
          )}
        >
          <span className="text-muted-foreground text-sm tabular-nums">
            +{count} more sources
          </span>
        </button>
      </PopoverTrigger>
      <PopoverContent
        side="bottom"
        align="start"
        className="w-80 p-1"
        onMouseEnter={handleMouseEnter}
        onMouseLeave={handleMouseLeave}
        onOpenAutoFocus={(e) => e.preventDefault()}
      >
        {popoverContent}
      </PopoverContent>
    </Popover>
  );
}

interface OverflowItemProps {
  citation: SerializableCitation;
  onClick: () => void;
}

function OverflowItem({ citation, onClick }: OverflowItemProps) {
  const TypeIcon = TYPE_ICONS[citation.type ?? "webpage"] ?? Globe;

  return (
    <button
      type="button"
      onClick={onClick}
      className="group hover:bg-muted focus-visible:bg-muted flex w-full cursor-pointer items-center gap-2.5 rounded-md px-2 py-2 text-left transition-colors focus-visible:outline-none"
    >
      {citation.favicon ? (
        <img
          src={citation.favicon}
          alt=""
          aria-hidden="true"
          width={16}
          height={16}
          className="bg-muted size-4 shrink-0 rounded object-cover"
        />
      ) : (
        <TypeIcon
          className="text-muted-foreground size-4 shrink-0"
          aria-hidden="true"
        />
      )}
      <div className="min-w-0 flex-1">
        <p className="group-hover:decoration-foreground/30 truncate text-sm font-medium group-hover:underline group-hover:underline-offset-2">
          {citation.title}
        </p>
        <p className="text-muted-foreground truncate text-xs">
          {citation.domain}
        </p>
      </div>
      <ExternalLink className="text-muted-foreground mt-0.5 size-3.5 shrink-0 self-start opacity-0 transition-opacity group-hover:opacity-100" />
    </button>
  );
}

interface StackedCitationsProps {
  id: string;
  citations: SerializableCitation[];
  className?: string;
  onNavigate?: (href: string, citation: SerializableCitation) => void;
}

function StackedCitations({
  id,
  citations,
  className,
  onNavigate,
}: StackedCitationsProps) {
  const {
    open,
    setOpen,
    containerRef,
    handleMouseEnter,
    handleMouseLeave,
    handleBlur,
  } = useHoverPopover();
  const maxIcons = 4;
  const visibleCitations = citations.slice(0, maxIcons);
  const remainingCount = Math.max(0, citations.length - maxIcons);

  const handleClick = (citation: SerializableCitation) => {
    const href = resolveSafeNavigationHref(citation.href);
    if (!href) return;
    if (onNavigate) {
      onNavigate(href, citation);
    } else {
      openSafeNavigationHref(href);
    }
  };

  return (
    <div ref={containerRef} onBlur={handleBlur} className="inline-flex">
      <Popover open={open}>
        <PopoverTrigger asChild>
          <button
            type="button"
            data-tool-ui-id={id}
            data-slot="citation-list"
            onMouseEnter={handleMouseEnter}
            onMouseLeave={handleMouseLeave}
            onKeyDown={(e) => {
              if (e.key === "Enter" || e.key === " ") {
                e.preventDefault();
                setOpen(true);
              }
            }}
            className={cn(
              "isolate inline-flex cursor-pointer items-center gap-2 rounded-lg px-3 py-2",
              "bg-muted/40 outline-none",
              "transition-colors duration-150",
              "hover:bg-muted/70",
              "focus-visible:ring-ring focus-visible:ring-2",
              className,
            )}
          >
            <div className="flex items-center">
              {visibleCitations.map((citation, index) => {
                const TypeIcon =
                  TYPE_ICONS[citation.type ?? "webpage"] ?? Globe;
                return (
                  <div
                    key={citation.id}
                    className={cn(
                      "border-border bg-background dark:border-foreground/20 relative flex size-6 items-center justify-center rounded-full border shadow-xs",
                      index > 0 && "-ml-2",
                    )}
                    style={{ zIndex: maxIcons - index }}
                  >
                    {citation.favicon ? (
                      <img
                        src={citation.favicon}
                        alt=""
                        aria-hidden="true"
                        width={18}
                        height={18}
                        className="size-4.5 rounded-full object-cover"
                      />
                    ) : (
                      <TypeIcon
                        className="text-muted-foreground size-3"
                        aria-hidden="true"
                      />
                    )}
                  </div>
                );
              })}
              {remainingCount > 0 && (
                <div
                  className="border-border bg-background dark:border-foreground/20 relative -ml-2 flex size-6 items-center justify-center rounded-full border shadow-xs"
                  style={{ zIndex: 0 }}
                >
                  <span className="text-muted-foreground text-[10px] font-medium tracking-tight">
                    •••
                  </span>
                </div>
              )}
            </div>
            <span className="text-muted-foreground text-sm tabular-nums">
              {citations.length} source{citations.length !== 1 && "s"}
            </span>
          </button>
        </PopoverTrigger>
        <PopoverContent
          side="bottom"
          align="start"
          className="w-80 p-1"
          onMouseEnter={handleMouseEnter}
          onMouseLeave={handleMouseLeave}
          onBlur={handleBlur}
          onEscapeKeyDown={() => setOpen(false)}
        >
          <div className="flex max-h-72 flex-col overflow-y-auto">
            {citations.map((citation) => (
              <OverflowItem
                key={citation.id}
                citation={citation}
                onClick={() => handleClick(citation)}
              />
            ))}
          </div>
        </PopoverContent>
      </Popover>
    </div>
  );
}

components/tool-ui/citation/citation.tsx
"use client";

import * as React from "react";
import type { LucideIcon } from "lucide-react";
import {
  FileText,
  Globe,
  Code2,
  Newspaper,
  Database,
  File,
  ExternalLink,
} from "lucide-react";
import { cn, Popover, PopoverContent, PopoverTrigger } from "./_adapter";

import { openSafeNavigationHref, sanitizeHref } from "../shared/media";
import type {
  SerializableCitation,
  CitationType,
  CitationVariant,
} from "./schema";

const FALLBACK_LOCALE = "en-US";

const TYPE_ICONS: Record<CitationType, LucideIcon> = {
  webpage: Globe,
  document: FileText,
  article: Newspaper,
  api: Database,
  code: Code2,
  other: File,
};

function extractDomain(url: string): string | undefined {
  try {
    const urlObj = new URL(url);
    return urlObj.hostname.replace(/^www\./, "");
  } catch {
    return undefined;
  }
}

function formatDate(isoString: string, locale: string): string {
  try {
    const date = new Date(isoString);
    return date.toLocaleDateString(locale, {
      year: "numeric",
      month: "short",
    });
  } catch {
    return isoString;
  }
}

function useHoverPopover(delay = 100) {
  const [open, setOpen] = React.useState(false);
  const timeoutRef = React.useRef<ReturnType<typeof setTimeout> | null>(null);

  const handleMouseEnter = React.useCallback(() => {
    if (timeoutRef.current) clearTimeout(timeoutRef.current);
    timeoutRef.current = setTimeout(() => setOpen(true), delay);
  }, [delay]);

  const handleMouseLeave = React.useCallback(() => {
    if (timeoutRef.current) clearTimeout(timeoutRef.current);
    timeoutRef.current = setTimeout(() => setOpen(false), delay);
  }, [delay]);

  React.useEffect(() => {
    return () => {
      if (timeoutRef.current) clearTimeout(timeoutRef.current);
    };
  }, []);

  return { open, setOpen, handleMouseEnter, handleMouseLeave };
}

export interface CitationProps extends SerializableCitation {
  variant?: CitationVariant;
  className?: string;
  onNavigate?: (href: string, citation: SerializableCitation) => void;
}

export function Citation(props: CitationProps) {
  const { variant = "default", className, onNavigate, ...serializable } = props;

  const {
    id,
    href: rawHref,
    title,
    snippet,
    domain: providedDomain,
    favicon,
    author,
    publishedAt,
    type = "webpage",
    locale: providedLocale,
  } = serializable;

  const locale = providedLocale ?? FALLBACK_LOCALE;
  const sanitizedHref = sanitizeHref(rawHref);
  const domain = providedDomain ?? extractDomain(rawHref);

  const citationData: SerializableCitation = {
    ...serializable,
    href: sanitizedHref ?? rawHref,
    domain,
    locale,
  };

  const TypeIcon = TYPE_ICONS[type] ?? Globe;

  const handleClick = () => {
    if (!sanitizedHref) return;
    if (onNavigate) {
      onNavigate(sanitizedHref, citationData);
    } else {
      openSafeNavigationHref(sanitizedHref);
    }
  };

  const handleKeyDown = (e: React.KeyboardEvent) => {
    if (sanitizedHref && (e.key === "Enter" || e.key === " ")) {
      e.preventDefault();
      handleClick();
    }
  };

  const iconElement = favicon ? (
    <img
      src={favicon}
      alt=""
      aria-hidden="true"
      width={14}
      height={14}
      className="bg-muted size-3.5 shrink-0 rounded object-cover"
    />
  ) : (
    <TypeIcon className="size-3.5 shrink-0 opacity-60" aria-hidden="true" />
  );

  const { open, handleMouseEnter, handleMouseLeave } = useHoverPopover();

  // Inline variant: compact chip with hover popover
  if (variant === "inline") {
    return (
      <Popover open={open}>
        <PopoverTrigger asChild>
          <button
            type="button"
            aria-label={title}
            data-tool-ui-id={id}
            data-slot="citation"
            onClick={handleClick}
            onMouseEnter={handleMouseEnter}
            onMouseLeave={handleMouseLeave}
            className={cn(
              "inline-flex cursor-pointer items-center gap-1.5 rounded-md px-2 py-1",
              "bg-muted/60 text-sm outline-none",
              "transition-colors duration-150",
              "hover:bg-muted",
              "focus-visible:ring-ring focus-visible:ring-2",
              className,
            )}
          >
            {iconElement}
            <span className="text-muted-foreground">{domain}</span>
          </button>
        </PopoverTrigger>
        <PopoverContent
          side="top"
          align="start"
          className="w-72 cursor-pointer p-0"
          onMouseEnter={handleMouseEnter}
          onMouseLeave={handleMouseLeave}
          onOpenAutoFocus={(e) => e.preventDefault()}
          onCloseAutoFocus={(e) => e.preventDefault()}
          onClick={handleClick}
        >
          <div className="hover:bg-muted/50 flex flex-col gap-2 p-3 transition-colors">
            <div className="flex items-start gap-2">
              {iconElement}
              <span className="text-muted-foreground text-xs">{domain}</span>
            </div>
            <p className="text-sm leading-snug font-medium">{title}</p>
            {snippet && (
              <p className="text-muted-foreground line-clamp-2 text-xs leading-relaxed">
                {snippet}
              </p>
            )}
          </div>
        </PopoverContent>
      </Popover>
    );
  }

  // Default variant: full card
  return (
    <article
      className={cn("relative w-full max-w-md min-w-72", className)}
      lang={locale}
      data-tool-ui-id={id}
      data-slot="citation"
    >
      <div
        className={cn(
          "group @container relative isolate flex w-full min-w-0 flex-col overflow-hidden rounded-xl",
          "border-border bg-card border text-sm shadow-xs",
          "transition-colors duration-150",
          sanitizedHref && [
            "cursor-pointer",
            "hover:border-foreground/25",
            "focus-visible:ring-ring focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:outline-none",
          ],
        )}
        onClick={sanitizedHref ? handleClick : undefined}
        role={sanitizedHref ? "link" : undefined}
        tabIndex={sanitizedHref ? 0 : undefined}
        onKeyDown={handleKeyDown}
      >
        <div className="flex flex-col gap-2 p-4">
          <div className="text-muted-foreground flex min-w-0 items-center justify-between gap-1.5 text-xs">
            <div className="flex min-w-0 items-center gap-1.5">
              {iconElement}
              <span className="truncate font-medium">{domain}</span>
              {(author || publishedAt) && (
                <span className="opacity-70">
                  <span className="opacity-60"> — </span>
                  {author}
                  {author && publishedAt && ", "}
                  {publishedAt && (
                    <time dateTime={publishedAt} className="tabular-nums">
                      {formatDate(publishedAt, locale)}
                    </time>
                  )}
                </span>
              )}
            </div>
            {sanitizedHref && (
              <ExternalLink className="size-3.5 shrink-0 opacity-0 transition-opacity group-hover:opacity-100" />
            )}
          </div>

          <h3 className="text-foreground text-[15px] leading-snug font-medium text-pretty">
            <span className="group-hover:decoration-foreground/30 line-clamp-2 group-hover:underline group-hover:underline-offset-2">
              {title}
            </span>
          </h3>

          {snippet && (
            <p className="text-muted-foreground text-[13px] leading-relaxed text-pretty">
              <span className="line-clamp-3">{snippet}</span>
            </p>
          )}
        </div>
      </div>
    </article>
  );
}

components/tool-ui/citation/index.ts
export { Citation } from "./citation";
export type { CitationProps } from "./citation";
export { CitationList } from "./citation-list";
export type { CitationListProps } from "./citation-list";
export type {
  SerializableCitation,
  CitationType,
  CitationVariant,
} from "./schema";

components/tool-ui/citation/README.md
# Citation

Implementation for the "citation" Tool UI surface.

## Files

- public exports: components/tool-ui/citation/index.ts
- serializable schema + parse helpers: components/tool-ui/citation/schema.ts

## Companion assets

- Docs page: app/docs/citation/content.mdx
- Preset payload: lib/presets/citation.ts

## Quick check

Run this after edits:

pnpm test

components/tool-ui/citation/schema.ts
import { z } from "zod";
import { defineToolUiContract } from "../shared/contract";
import {
  ToolUIIdSchema,
  ToolUIReceiptSchema,
  ToolUIRoleSchema,
} from "../shared/schema";

export const CitationTypeSchema = z.enum([
  "webpage",
  "document",
  "article",
  "api",
  "code",
  "other",
]);

export type CitationType = z.infer<typeof CitationTypeSchema>;

export const CitationVariantSchema = z.enum(["default", "inline", "stacked"]);

export type CitationVariant = z.infer<typeof CitationVariantSchema>;

export const SerializableCitationSchema = z.object({
  id: ToolUIIdSchema,
  role: ToolUIRoleSchema.optional(),
  receipt: ToolUIReceiptSchema.optional(),
  href: z.string().url(),
  title: z.string(),
  snippet: z.string().optional(),
  domain: z.string().optional(),
  favicon: z.string().url().optional(),
  author: z.string().optional(),
  publishedAt: z.string().datetime().optional(),
  type: CitationTypeSchema.optional(),
  locale: z.string().optional(),
});

export type SerializableCitation = z.infer<typeof SerializableCitationSchema>;

const SerializableCitationSchemaContract = defineToolUiContract(
  "Citation",
  SerializableCitationSchema,
);

export const parseSerializableCitation: (
  input: unknown,
) => SerializableCitation = SerializableCitationSchemaContract.parse;

export const safeParseSerializableCitation: (
  input: unknown,
) => SerializableCitation | null = SerializableCitationSchemaContract.safeParse;

components/tool-ui/shared/contract.ts
import { z } from "zod";
import { parseWithSchema, safeParseWithSchema } from "./parse";

export interface ToolUiContract<T> {
  schema: z.ZodType<T>;
  parse: (input: unknown) => T;
  safeParse: (input: unknown) => T | null;
}

export function defineToolUiContract<T>(
  componentName: string,
  schema: z.ZodType<T>,
): ToolUiContract<T> {
  return {
    schema,
    parse: (input: unknown) => parseWithSchema(schema, input, componentName),
    safeParse: (input: unknown) => safeParseWithSchema(schema, input),
  };
}

components/tool-ui/shared/media/aspect-ratio.ts
import { z } from "zod";

export const AspectRatioSchema = z
  .enum(["auto", "1:1", "4:3", "16:9", "9:16"])
  .default("auto");

export type AspectRatio = z.infer<typeof AspectRatioSchema>;

export const MediaFitSchema = z.enum(["cover", "contain"]).default("cover");

export type MediaFit = z.infer<typeof MediaFitSchema>;

export const RATIO_CLASS_MAP: Record<AspectRatio, string> = {
  auto: "",
  "1:1": "aspect-square",
  "4:3": "aspect-[4/3]",
  "16:9": "aspect-video",
  "9:16": "aspect-[9/16]",
};

export function getRatioClass(ratio: AspectRatio): string {
  return RATIO_CLASS_MAP[ratio];
}

export function getFitClass(fit: MediaFit): string {
  return fit === "cover" ? "object-cover" : "object-contain";
}

components/tool-ui/shared/media/format-utils.ts
/**
 * Format duration in milliseconds to human-readable string.
 * @example formatDuration(128000) => "2:08"
 * @example formatDuration(3661000) => "1:01:01"
 */
export function formatDuration(durationMs: number): string {
  const totalSeconds = Math.round(durationMs / 1000);
  const hours = Math.floor(totalSeconds / 3600);
  const minutes = Math.floor((totalSeconds % 3600) / 60);
  const seconds = totalSeconds % 60;

  if (hours > 0) {
    return `${hours}:${minutes.toString().padStart(2, "0")}:${seconds
      .toString()
      .padStart(2, "0")}`;
  }
  return `${minutes}:${seconds.toString().padStart(2, "0")}`;
}

/**
 * Format file size in bytes to human-readable string.
 * @example formatFileSize(1024) => "1 KB"
 * @example formatFileSize(1536000) => "1.5 MB"
 */
export function formatFileSize(bytes: number): string {
  if (bytes < 1024) return `${bytes} B`;
  const units = ["KB", "MB", "GB"];
  let size = bytes / 1024;
  let unit = 0;
  while (size >= 1024 && unit < units.length - 1) {
    size /= 1024;
    unit += 1;
  }
  return `${size.toFixed(size >= 10 ? 0 : 1)} ${units[unit]}`;
}

components/tool-ui/shared/media/index.ts
export {
  AspectRatioSchema,
  MediaFitSchema,
  RATIO_CLASS_MAP,
  getRatioClass,
  getFitClass,
  type AspectRatio,
  type MediaFit,
} from "./aspect-ratio";

export { OVERLAY_GRADIENT } from "./overlay-gradient";

export { formatDuration, formatFileSize } from "./format-utils";

export { sanitizeHref } from "./sanitize-href";
export {
  resolveSafeNavigationHref,
  openSafeNavigationHref,
} from "./safe-navigation";

components/tool-ui/shared/media/overlay-gradient.ts
/**
 * Eased gradient for hover overlays on media elements.
 * Creates a smooth fade from opaque black at top to transparent.
 *
 * @see https://larsenwork.com/easing-gradients/
 */
export const OVERLAY_GRADIENT = `linear-gradient(
  to bottom,
  hsl(0, 0%, 0%) 0%,
  hsla(0, 0%, 0%, 0.987) 8.3%,
  hsla(0, 0%, 0%, 0.951) 16.6%,
  hsla(0, 0%, 0%, 0.896) 24.6%,
  hsla(0, 0%, 0%, 0.825) 32.5%,
  hsla(0, 0%, 0%, 0.741) 40.1%,
  hsla(0, 0%, 0%, 0.648) 47.6%,
  hsla(0, 0%, 0%, 0.55) 54.8%,
  hsla(0, 0%, 0%, 0.45) 61.7%,
  hsla(0, 0%, 0%, 0.352) 68.3%,
  hsla(0, 0%, 0%, 0.259) 74.5%,
  hsla(0, 0%, 0%, 0.175) 80.4%,
  hsla(0, 0%, 0%, 0.104) 86%,
  hsla(0, 0%, 0%, 0.049) 91.1%,
  hsla(0, 0%, 0%, 0.013) 95.8%,
  hsla(0, 0%, 0%, 0) 100%
)` as const;

components/tool-ui/shared/media/safe-navigation.ts
import { sanitizeHref } from "./sanitize-href";

export function resolveSafeNavigationHref(
  ...candidates: Array<string | null | undefined>
): string | undefined {
  for (const candidate of candidates) {
    const safeHref = sanitizeHref(candidate ?? undefined);
    if (safeHref) {
      return safeHref;
    }
  }

  return undefined;
}

export function openSafeNavigationHref(href: string | undefined): boolean {
  if (!href || typeof window === "undefined") {
    return false;
  }

  window.open(href, "_blank", "noopener,noreferrer");
  return true;
}

components/tool-ui/shared/media/sanitize-href.ts
/**
 * Sanitize a URL to ensure it's safe for use in href attributes.
 * Allows:
 * - Absolute http(s) URLs
 * - Relative URLs (/path, ./path, ../path, ?query, #hash)
 *
 * @returns The sanitized URL string, or undefined if invalid/unsafe
 */
export function sanitizeHref(href?: string): string | undefined {
  if (!href) return undefined;
  const candidate = href.trim();
  if (!candidate) return undefined;

  if (
    candidate.startsWith("/") ||
    candidate.startsWith("./") ||
    candidate.startsWith("../") ||
    candidate.startsWith("?") ||
    candidate.startsWith("#")
  ) {
    if (candidate.startsWith("//")) return undefined;
    // eslint-disable-next-line no-control-regex -- intentionally matching control characters
    if (/[\u0000-\u001F\u007F]/.test(candidate)) return undefined;
    return candidate;
  }

  try {
    const url = new URL(candidate);
    if (url.protocol === "http:" || url.protocol === "https:") {
      return url.toString();
    }
  } catch {
    return undefined;
  }
  return undefined;
}

components/tool-ui/shared/parse.ts
import { z } from "zod";

function formatZodPath(path: Array<string | number | symbol>): string {
  if (path.length === 0) return "root";
  return path
    .map((segment) =>
      typeof segment === "number" ? `[${segment}]` : String(segment),
    )
    .join(".");
}

/**
 * Format Zod errors into a compact `path: message` string.
 */
export function formatZodError(error: z.ZodError): string {
  const parts = error.issues.map((issue) => {
    const path = formatZodPath(issue.path);
    return `${path}: ${issue.message}`;
  });

  return Array.from(new Set(parts)).join("; ");
}

/**
 * Parse unknown input and throw a readable error.
 */
export function parseWithSchema<T>(
  schema: z.ZodType<T>,
  input: unknown,
  name: string,
): T {
  const res = schema.safeParse(input);
  if (!res.success) {
    throw new Error(`Invalid ${name} payload: ${formatZodError(res.error)}`);
  }
  return res.data;
}

/**
 * Parse unknown input, returning `null` instead of throwing on failure.
 *
 * Use this in assistant-ui `render` functions where `args` stream in
 * incrementally and may be incomplete until the tool call finishes.
 */
export function safeParseWithSchema<T>(
  schema: z.ZodType<T>,
  input: unknown,
): T | null {
  const res = schema.safeParse(input);
  return res.success ? res.data : null;
}

components/tool-ui/shared/schema.ts
import { z } from "zod";
import type { ReactNode } from "react";

/**
 * Tool UI conventions:
 * - Serializable schemas are JSON-safe (no callbacks/ReactNode/`className`).
 * - Schema: `SerializableXSchema`
 * - Parser: `parseSerializableX(input: unknown)` (throws on invalid)
 * - Safe parser: `safeParseSerializableX(input: unknown)` (returns `null` on invalid)
 * - Actions: `LocalActions` for non-receipt actions and `DecisionActions` for consequential actions
 * - Root attrs: `data-tool-ui-id` + `data-slot`
 */

/**
 * Schema for tool UI identity.
 *
 * Every tool UI should have a unique identifier that:
 * - Is stable across re-renders
 * - Is meaningful (not auto-generated)
 * - Is unique within the conversation
 *
 * Format recommendation: `{component-type}-{semantic-identifier}`
 * Examples: "data-table-expenses-q3", "option-list-deploy-target"
 */
export const ToolUIIdSchema = z.string().min(1);

export type ToolUIId = z.infer<typeof ToolUIIdSchema>;

/**
 * Primary role of a Tool UI surface in a chat context.
 */
export const ToolUIRoleSchema = z.enum([
  "information",
  "decision",
  "control",
  "state",
  "composite",
]);

export type ToolUIRole = z.infer<typeof ToolUIRoleSchema>;

export const ToolUIReceiptOutcomeSchema = z.enum([
  "success",
  "partial",
  "failed",
  "cancelled",
]);

export type ToolUIReceiptOutcome = z.infer<typeof ToolUIReceiptOutcomeSchema>;

/**
 * Optional receipt metadata: a durable summary of an outcome.
 */
export const ToolUIReceiptSchema = z.object({
  outcome: ToolUIReceiptOutcomeSchema,
  summary: z.string().min(1),
  identifiers: z.record(z.string(), z.string()).optional(),
  at: z.string().datetime(),
});

export type ToolUIReceipt = z.infer<typeof ToolUIReceiptSchema>;

/**
 * Base schema for Tool UI payloads (id + optional role/receipt).
 */
export const ToolUISurfaceSchema = z.object({
  id: ToolUIIdSchema,
  role: ToolUIRoleSchema.optional(),
  receipt: ToolUIReceiptSchema.optional(),
});

export type ToolUISurface = z.infer<typeof ToolUISurfaceSchema>;

export const ActionSchema = z.object({
  id: z.string().min(1),
  label: z.string().min(1),
  /**
   * Canonical narration the assistant can use after this action is taken.
   *
   * Example: "I exported the table as CSV." / "I opened the link in a new tab."
   */
  sentence: z.string().optional(),
  confirmLabel: z.string().optional(),
  variant: z
    .enum(["default", "destructive", "secondary", "ghost", "outline"])
    .optional(),
  icon: z.custom<ReactNode>().optional(),
  loading: z.boolean().optional(),
  disabled: z.boolean().optional(),
  shortcut: z.string().optional(),
});

export type Action = z.infer<typeof ActionSchema>;
export type LocalAction = Action;
export type DecisionAction = Action;

export const DecisionResultSchema = z.object({
  kind: z.literal("decision"),
  version: z.literal(1),
  decisionId: z.string().min(1),
  actionId: z.string().min(1),
  actionLabel: z.string().min(1),
  at: z.string().datetime(),
  payload: z.record(z.string(), z.unknown()).optional(),
});

export type DecisionResult<
  TPayload extends Record<string, unknown> = Record<string, unknown>,
> = Omit<z.infer<typeof DecisionResultSchema>, "payload"> & {
  payload?: TPayload;
};

export function createDecisionResult<
  TPayload extends Record<string, unknown> = Record<string, unknown>,
>(args: {
  decisionId: string;
  action: { id: string; label: string };
  payload?: TPayload;
}): DecisionResult<TPayload> {
  return {
    kind: "decision",
    version: 1,
    decisionId: args.decisionId,
    actionId: args.action.id,
    actionLabel: args.action.label,
    at: new Date().toISOString(),
    payload: args.payload,
  };
}

export const ActionButtonsPropsSchema = z.object({
  actions: z.array(ActionSchema).min(1),
  align: z.enum(["left", "center", "right"]).optional(),
  confirmTimeout: z.number().positive().optional(),
  className: z.string().optional(),
});

export const SerializableActionSchema = ActionSchema.omit({ icon: true });
export const SerializableActionsSchema = ActionButtonsPropsSchema.extend({
  actions: z.array(SerializableActionSchema),
}).omit({ className: true });

export interface ActionsConfig {
  items: Action[];
  align?: "left" | "center" | "right";
  confirmTimeout?: number;
}

export const SerializableActionsConfigSchema = z.object({
  items: z.array(SerializableActionSchema).min(1),
  align: z.enum(["left", "center", "right"]).optional(),
  confirmTimeout: z.number().positive().optional(),
});

export type SerializableActionsConfig = z.infer<
  typeof SerializableActionsConfigSchema
>;

export type SerializableAction = z.infer<typeof SerializableActionSchema>;

demo.tsx
import { Citation } from "@/components/ui/citation";

export default function CitationDemo() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center p-6">
      <Citation
        id="citation-example"
        href="https://react.dev/reference/react/useState"
        title="useState – React"
        snippet="useState is a React Hook that lets you add a state variable to your component."
        domain="react.dev"
        favicon="https://www.google.com/s2/favicons?domain=react.dev&sz=32"
        author="React Docs"
        publishedAt="2024-04-25T00:00:00.000Z"
        type="document"
      />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react zod
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add aspect-ratio popover
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
