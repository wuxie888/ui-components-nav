<!-- Message Draft · @tool-ui · https://21st.dev/@tool-ui/components/message-draft
     license: MIT · category: ai-chat
     A review card that shows a drafted email or Slack message with Send/Cancel actions and an undo grace period before it sends. -->

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
components/tool-ui/message-draft/_adapter.tsx
/**
 * Adapter: UI and utility re-exports for copy-standalone portability.
 *
 * When copying this component to another project, update these imports
 * to match your project's paths:
 *
 *   cn        → Your Tailwind merge utility (e.g., "@/lib/utils", "~/lib/cn")
 *   Button    → shadcn/ui Button
 */

export { cn } from "@/lib/utils";
export { Button } from "@/components/ui/button";

components/tool-ui/message-draft/index.tsx
export { MessageDraft } from "./message-draft";
export {
  type SerializableMessageDraft,
  type SerializableEmailDraft,
  type SerializableSlackDraft,
  type MessageDraftChannel,
  type MessageDraftOutcome,
  type SlackTarget,
  type MessageDraftProps,
} from "./schema";

components/tool-ui/message-draft/message-draft.tsx
"use client";

import * as React from "react";
import { cn, Button } from "./_adapter";
import type {
  MessageDraftProps,
  SerializableEmailDraft,
  SerializableSlackDraft,
} from "./schema";
import { ActionButtons } from "../shared/action-buttons";
import type { Action } from "../shared/schema";
import { Check, ChevronDown } from "lucide-react";

type DraftState = "review" | "sending" | "sent" | "cancelled";
type DraftOutcome = MessageDraftProps["outcome"];

const DEFAULT_GRACE_PERIOD = 5000;
const COLLAPSED_BODY_HEIGHT = 280;

interface RecipientRowProps {
  label: string;
  recipients: string[];
  maxVisible?: number;
  muted?: boolean;
}

function RecipientRow({
  label,
  recipients,
  maxVisible = 3,
  muted = false,
}: RecipientRowProps) {
  const visibleRecipients = recipients.slice(0, maxVisible);
  const overflowCount = recipients.length - maxVisible;

  return (
    <tr className="text-sm">
      <td className="text-muted-foreground w-0 pr-4 pb-1 text-right align-top font-medium whitespace-nowrap">
        {label}
      </td>
      <td className={cn("pb-1 align-top", muted && "text-muted-foreground")}>
        {visibleRecipients.join(", ")}
        {overflowCount > 0 && (
          <span className="text-muted-foreground"> +{overflowCount} more</span>
        )}
      </td>
    </tr>
  );
}

interface SingleFieldRowProps {
  label: string;
  value: string;
}

function SingleFieldRow({ label, value }: SingleFieldRowProps) {
  return (
    <tr className="text-sm">
      <td className="text-muted-foreground w-0 pr-4 pb-1 text-right align-top font-medium whitespace-nowrap">
        {label}
      </td>
      <td className="pb-1 align-top">{value}</td>
    </tr>
  );
}

interface ExpandableBodyProps {
  body: string;
  isExpanded: boolean;
  onNeedsExpansionChange?: (needsExpansion: boolean) => void;
}

function ExpandableBody({
  body,
  isExpanded,
  onNeedsExpansionChange,
}: ExpandableBodyProps) {
  const [needsExpansion, setNeedsExpansion] = React.useState<boolean | null>(
    null,
  );
  const contentRef = React.useRef<HTMLDivElement>(null);

  React.useLayoutEffect(() => {
    if (contentRef.current) {
      const needs = contentRef.current.scrollHeight > COLLAPSED_BODY_HEIGHT;
      setNeedsExpansion(needs);
      onNeedsExpansionChange?.(needs);
    }
  }, [body, onNeedsExpansionChange]);

  return (
    <div className="relative">
      <div
        ref={contentRef}
        className={cn(
          "overflow-hidden text-sm leading-relaxed",
          needsExpansion !== null &&
            "transition-[max-height] duration-300 ease-in-out",
        )}
        style={{
          maxHeight:
            needsExpansion === null
              ? `${COLLAPSED_BODY_HEIGHT}px`
              : isExpanded || !needsExpansion
                ? `${contentRef.current?.scrollHeight ?? 1000}px`
                : `${COLLAPSED_BODY_HEIGHT}px`,
        }}
      >
        <p className="pt-1 whitespace-pre-wrap">{body}</p>
      </div>
      {needsExpansion && (
        <div
          className={cn(
            "from-card pointer-events-none absolute inset-x-0 bottom-0 bg-gradient-to-t to-transparent transition-[height] duration-300 ease-in-out",
            isExpanded ? "h-0" : "h-12",
          )}
        />
      )}
    </div>
  );
}

interface EmailDraftContentProps {
  draft: SerializableEmailDraft;
  titleId: string;
  isExpanded: boolean;
  onNeedsExpansionChange?: (needsExpansion: boolean) => void;
}

function EmailDraftContent({
  draft,
  titleId,
  isExpanded,
  onNeedsExpansionChange,
}: EmailDraftContentProps) {
  return (
    <>
      <h2 id={titleId} className="pt-2 text-base leading-tight font-semibold">
        {draft.subject}
      </h2>

      <table className="w-full">
        <tbody>
          {draft.from && <SingleFieldRow label="From" value={draft.from} />}
          <RecipientRow label="To" recipients={draft.to} />
          {draft.cc && draft.cc.length > 0 && (
            <RecipientRow label="Cc" recipients={draft.cc} />
          )}
          {draft.bcc && draft.bcc.length > 0 && (
            <RecipientRow label="Bcc" recipients={draft.bcc} muted />
          )}
        </tbody>
      </table>

      <div className="bg-border -mx-5 h-px" role="separator" />

      <ExpandableBody
        body={draft.body}
        isExpanded={isExpanded}
        onNeedsExpansionChange={onNeedsExpansionChange}
      />
    </>
  );
}

interface SlackDraftContentProps {
  draft: SerializableSlackDraft;
  titleId: string;
  isExpanded: boolean;
  onNeedsExpansionChange?: (needsExpansion: boolean) => void;
}

function SlackLogo({ className }: { className?: string }) {
  return (
    <svg className={className} viewBox="0 0 24 24" aria-hidden="true">
      <path
        fill="#E01E5A"
        d="M5.042 15.165a2.528 2.528 0 0 1-2.52 2.523A2.528 2.528 0 0 1 0 15.165a2.527 2.527 0 0 1 2.522-2.52h2.52v2.52zm1.271 0a2.527 2.527 0 0 1 2.521-2.52 2.527 2.527 0 0 1 2.521 2.52v6.313A2.528 2.528 0 0 1 8.834 24a2.528 2.528 0 0 1-2.521-2.522v-6.313z"
      />
      <path
        fill="#36C5F0"
        d="M8.834 5.042a2.528 2.528 0 0 1-2.521-2.52A2.528 2.528 0 0 1 8.834 0a2.528 2.528 0 0 1 2.521 2.522v2.52H8.834zm0 1.271a2.528 2.528 0 0 1 2.521 2.521 2.528 2.528 0 0 1-2.521 2.521H2.522A2.528 2.528 0 0 1 0 8.834a2.528 2.528 0 0 1 2.522-2.521h6.312z"
      />
      <path
        fill="#2EB67D"
        d="M18.958 8.834a2.528 2.528 0 0 1 2.522-2.521A2.528 2.528 0 0 1 24 8.834a2.528 2.528 0 0 1-2.52 2.521h-2.522V8.834zm-1.271 0a2.528 2.528 0 0 1-2.521 2.521 2.528 2.528 0 0 1-2.521-2.521V2.522A2.528 2.528 0 0 1 15.165 0a2.528 2.528 0 0 1 2.522 2.522v6.312z"
      />
      <path
        fill="#ECB22E"
        d="M15.165 18.958a2.528 2.528 0 0 1 2.522 2.522A2.528 2.528 0 0 1 15.165 24a2.527 2.527 0 0 1-2.521-2.52v-2.522h2.521zm0-1.271a2.527 2.527 0 0 1-2.521-2.521 2.526 2.526 0 0 1 2.521-2.521h6.313A2.527 2.527 0 0 1 24 15.165a2.528 2.528 0 0 1-2.522 2.521h-6.313z"
      />
    </svg>
  );
}

function SlackDraftContent({
  draft,
  titleId,
  isExpanded,
  onNeedsExpansionChange,
}: SlackDraftContentProps) {
  const { target } = draft;
  const isChannel = target.type === "channel";
  const targetDisplay = isChannel
    ? `#${target.name}`
    : `Message to @${target.name}`;
  const memberCount = isChannel ? target.memberCount : undefined;

  return (
    <>
      <div
        id={titleId}
        className="flex items-center gap-1.5 text-sm font-medium"
      >
        <SlackLogo className="size-4" />
        <span>{targetDisplay}</span>
        {memberCount !== undefined && (
          <span className="text-muted-foreground ml-auto text-sm font-normal">
            {memberCount.toLocaleString()} members
          </span>
        )}
      </div>

      <div className="bg-border -mx-5 h-px" role="separator" />

      <ExpandableBody
        body={draft.body}
        isExpanded={isExpanded}
        onNeedsExpansionChange={onNeedsExpansionChange}
      />
    </>
  );
}

function formatSentTime(date: Date): string {
  return date.toLocaleTimeString(undefined, {
    hour: "numeric",
    minute: "2-digit",
  });
}

export function resolveStateFromOutcome(outcome: DraftOutcome): DraftState {
  if (outcome === "sent") return "sent";
  if (outcome === "cancelled") return "cancelled";
  return "review";
}

export function resolveOutcomeTransition(
  previousOutcome: DraftOutcome,
  nextOutcome: DraftOutcome,
): DraftState | null {
  if (previousOutcome === nextOutcome) {
    return null;
  }

  return resolveStateFromOutcome(nextOutcome);
}

interface SentConfirmationProps {
  sentAt: Date;
}

function SentConfirmation({ sentAt }: SentConfirmationProps) {
  return (
    <div
      className="flex items-center justify-end gap-2 text-sm"
      role="status"
      aria-label="Message sent"
    >
      <span className="text-muted-foreground">
        Sent at {formatSentTime(sentAt)}
      </span>
      <span className="bg-primary/10 text-primary flex size-6 shrink-0 items-center justify-center rounded-full">
        <Check className="size-3.5" />
      </span>
    </div>
  );
}

export function MessageDraft(props: MessageDraftProps) {
  const {
    id,
    className,
    outcome,
    undoGracePeriod = DEFAULT_GRACE_PERIOD,
    onSend,
    onUndo,
    onCancel,
  } = props;

  const [state, setState] = React.useState<DraftState>(() =>
    resolveStateFromOutcome(outcome),
  );
  const [countdown, setCountdown] = React.useState(
    Math.ceil(undoGracePeriod / 1000),
  );
  const [sentAt, setSentAt] = React.useState<Date | null>(() =>
    outcome === "sent" ? new Date() : null,
  );
  const [isExpanded, setIsExpanded] = React.useState(false);
  const [needsExpansion, setNeedsExpansion] = React.useState(false);
  const undoButtonRef = React.useRef<HTMLButtonElement>(null);
  const timerRef = React.useRef<ReturnType<typeof setTimeout> | null>(null);
  const countdownRef = React.useRef<ReturnType<typeof setInterval> | null>(
    null,
  );
  const previousOutcomeRef = React.useRef<DraftOutcome>(outcome);

  const clearTimers = React.useCallback(() => {
    if (timerRef.current) {
      clearTimeout(timerRef.current);
      timerRef.current = null;
    }
    if (countdownRef.current) {
      clearInterval(countdownRef.current);
      countdownRef.current = null;
    }
  }, []);

  React.useEffect(() => {
    return clearTimers;
  }, [clearTimers]);

  React.useEffect(() => {
    const nextState = resolveOutcomeTransition(
      previousOutcomeRef.current,
      outcome,
    );

    previousOutcomeRef.current = outcome;

    if (nextState === null) {
      return;
    }

    clearTimers();
    setState(nextState);
    setCountdown(Math.ceil(undoGracePeriod / 1000));
    setSentAt(nextState === "sent" ? new Date() : null);
  }, [outcome, undoGracePeriod, clearTimers]);

  React.useEffect(() => {
    if (state === "sending") {
      undoButtonRef.current?.focus();

      setCountdown(Math.ceil(undoGracePeriod / 1000));

      countdownRef.current = setInterval(() => {
        setCountdown((prev) => {
          if (prev <= 1) {
            if (countdownRef.current) {
              clearInterval(countdownRef.current);
              countdownRef.current = null;
            }
            return 0;
          }
          return prev - 1;
        });
      }, 1000);

      timerRef.current = setTimeout(async () => {
        clearTimers();
        await onSend?.();
        setSentAt(new Date());
        setState("sent");
      }, undoGracePeriod);
    }
  }, [state, undoGracePeriod, onSend, clearTimers]);

  const handleSend = React.useCallback(() => {
    setState("sending");
  }, []);

  const handleUndo = React.useCallback(() => {
    clearTimers();
    setState("review");
    onUndo?.();
  }, [clearTimers, onUndo]);

  const handleCancel = React.useCallback(() => {
    clearTimers();
    setState("cancelled");
    onCancel?.();
  }, [clearTimers, onCancel]);

  const handleKeyDown = React.useCallback(
    (event: React.KeyboardEvent) => {
      if (event.key === "Escape" && state === "review") {
        event.preventDefault();
        handleCancel();
      }
    },
    [state, handleCancel],
  );

  const handleNeedsExpansionChange = React.useCallback((needs: boolean) => {
    setNeedsExpansion(needs);
  }, []);

  const handleToggleExpand = React.useCallback(() => {
    setIsExpanded((prev) => !prev);
  }, []);

  const handleAction = React.useCallback(
    async (actionId: string) => {
      if (actionId === "send") {
        handleSend();
      } else if (actionId === "cancel") {
        handleCancel();
      }
    },
    [handleSend, handleCancel],
  );

  const actions: Action[] = [
    {
      id: "cancel",
      label: "Cancel",
      variant: "ghost",
    },
    {
      id: "send",
      label: "Send",
      variant: "default",
    },
  ];

  const expandButton = needsExpansion ? (
    <Button
      variant="ghost"
      size="sm"
      onClick={handleToggleExpand}
      className="h-7 gap-1 px-2 text-sm"
    >
      {isExpanded ? "Show less" : "Read more"}
      <ChevronDown className={cn("size-3", isExpanded && "rotate-180")} />
    </Button>
  ) : null;

  const renderActions = () => {
    switch (state) {
      case "sending":
        return (
          <div
            className="flex items-center justify-end gap-3"
            aria-live="polite"
          >
            <span className="text-muted-foreground text-sm">
              Sending in {countdown}s
            </span>
            <Button
              ref={undoButtonRef}
              variant="outline"
              size="sm"
              onClick={handleUndo}
              className="rounded-full"
            >
              Undo
            </Button>
          </div>
        );
      case "sent":
        return <SentConfirmation sentAt={sentAt ?? new Date()} />;
      case "cancelled":
        return null;
      default:
        return <ActionButtons actions={actions} onAction={handleAction} />;
    }
  };

  if (state === "cancelled") {
    return null;
  }

  return (
    <article
      className={cn(
        "flex w-full max-w-lg min-w-64 flex-col gap-3",
        "text-foreground",
        className,
      )}
      data-slot="message-draft"
      data-tool-ui-id={id}
      data-state={state}
      aria-labelledby={`${id}-title`}
      onKeyDown={handleKeyDown}
    >
      <div className="bg-card flex w-full flex-col gap-3 rounded-2xl border px-5 pt-3 pb-5 shadow-xs transition-none">
        {props.channel === "email" ? (
          <EmailDraftContent
            draft={props}
            titleId={`${id}-title`}
            isExpanded={isExpanded}
            onNeedsExpansionChange={handleNeedsExpansionChange}
          />
        ) : (
          <SlackDraftContent
            draft={props}
            titleId={`${id}-title`}
            isExpanded={isExpanded}
            onNeedsExpansionChange={handleNeedsExpansionChange}
          />
        )}

        {expandButton}
      </div>

      <div className="@container/actions">{renderActions()}</div>
    </article>
  );
}

components/tool-ui/message-draft/README.md
# Message Draft

Implementation for the "message-draft" Tool UI surface.

## Files

- public exports: components/tool-ui/message-draft/index.tsx
- serializable schema + parse helpers: components/tool-ui/message-draft/schema.ts

## Companion assets

- Docs page: app/docs/message-draft/content.mdx
- Preset payload: lib/presets/message-draft.ts

## Quick check

Run this after edits:

pnpm test

components/tool-ui/message-draft/schema.ts
import { z } from "zod";
import { ToolUIIdSchema, ToolUIRoleSchema } from "../shared/schema";
import { defineToolUiContract } from "../shared/contract";

export const MessageDraftChannelSchema = z.enum(["email", "slack"]);

export type MessageDraftChannel = z.infer<typeof MessageDraftChannelSchema>;

export const MessageDraftOutcomeSchema = z.enum(["sent", "cancelled"]);

export type MessageDraftOutcome = z.infer<typeof MessageDraftOutcomeSchema>;

const SlackTargetSchema = z.discriminatedUnion("type", [
  z.object({
    type: z.literal("channel"),
    name: z.string().min(1),
    memberCount: z.number().optional(),
  }),
  z.object({ type: z.literal("dm"), name: z.string().min(1) }),
]);

export type SlackTarget = z.infer<typeof SlackTargetSchema>;

export const SerializableEmailDraftSchema = z.object({
  id: ToolUIIdSchema,
  role: ToolUIRoleSchema.optional(),
  body: z.string().min(1),
  outcome: MessageDraftOutcomeSchema.optional(),
  channel: z.literal("email"),
  subject: z.string().min(1),
  from: z.string().optional(),
  to: z.array(z.string()).min(1),
  cc: z.array(z.string()).optional(),
  bcc: z.array(z.string()).optional(),
});

export const SerializableSlackDraftSchema = z.object({
  id: ToolUIIdSchema,
  role: ToolUIRoleSchema.optional(),
  body: z.string().min(1),
  outcome: MessageDraftOutcomeSchema.optional(),
  channel: z.literal("slack"),
  target: SlackTargetSchema,
});

export const SerializableMessageDraftSchema = z.discriminatedUnion("channel", [
  SerializableEmailDraftSchema,
  SerializableSlackDraftSchema,
]);

export type SerializableMessageDraft = z.infer<
  typeof SerializableMessageDraftSchema
>;

export type SerializableEmailDraft = z.infer<
  typeof SerializableEmailDraftSchema
>;

export type SerializableSlackDraft = z.infer<
  typeof SerializableSlackDraftSchema
>;

const SerializableMessageDraftSchemaContract = defineToolUiContract(
  "MessageDraft",
  SerializableMessageDraftSchema,
);

export const parseSerializableMessageDraft: (
  input: unknown,
) => SerializableMessageDraft = SerializableMessageDraftSchemaContract.parse;

export const safeParseSerializableMessageDraft: (
  input: unknown,
) => SerializableMessageDraft | null =
  SerializableMessageDraftSchemaContract.safeParse;

export type MessageDraftProps = SerializableMessageDraft & {
  className?: string;
  undoGracePeriod?: number;
  onSend?: () => void | Promise<void>;
  onUndo?: () => void;
  onCancel?: () => void;
};

components/tool-ui/shared/_adapter.tsx
/**
 * Adapter: UI and utility re-exports for copy-standalone portability.
 *
 * When copying this component to another project, update these imports
 * to match your project's paths:
 *
 *   cn     → Your Tailwind merge utility (e.g., "@/lib/utils", "~/lib/cn")
 *   Button → shadcn/ui Button
 */

export { cn } from "@/lib/utils";
export { Button } from "@/components/ui/button";

components/tool-ui/shared/action-buttons.tsx
"use client";

import type { Action } from "./schema";
import { cn, Button } from "./_adapter";
import { useActionButtons } from "./use-action-buttons";

export interface ActionButtonsProps {
  actions: Action[];
  onAction: (actionId: string) => void | Promise<void>;
  onBeforeAction?: (actionId: string) => boolean | Promise<boolean>;
  confirmTimeout?: number;
  align?: "left" | "center" | "right";
  className?: string;
}

export function ActionButtons({
  actions,
  onAction,
  onBeforeAction,
  confirmTimeout = 3000,
  align = "right",
  className,
}: ActionButtonsProps) {
  const { actions: resolvedActions, runAction } = useActionButtons({
    actions,
    onAction,
    onBeforeAction,
    confirmTimeout,
  });

  return (
    <div
      className={cn(
        "flex flex-col gap-3",
        "@sm/actions:flex-row @sm/actions:flex-wrap @sm/actions:items-center @sm/actions:gap-2",
        align === "left" && "@sm/actions:justify-start",
        align === "center" && "@sm/actions:justify-center",
        align === "right" && "@sm/actions:justify-end",
        className,
      )}
    >
      {resolvedActions.map((action) => {
        const label = action.currentLabel;
        const variant = action.variant || "default";

        return (
          <Button
            key={action.id}
            variant={variant}
            onClick={() => runAction(action.id)}
            disabled={action.isDisabled}
            className={cn(
              "rounded-full px-4!",
              "justify-center",
              "min-h-11 w-full text-base",
              "@sm/actions:min-h-0 @sm/actions:w-auto @sm/actions:px-3 @sm/actions:py-2 @sm/actions:text-sm",
              action.isConfirming &&
                "ring-destructive ring-2 ring-offset-2 motion-safe:animate-pulse",
            )}
            aria-label={
              action.shortcut ? `${label} (${action.shortcut})` : label
            }
          >
            {action.isLoading && (
              <svg
                className="mr-2 h-4 w-4 motion-safe:animate-spin"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
              >
                <circle
                  className="opacity-25"
                  cx="12"
                  cy="12"
                  r="10"
                  stroke="currentColor"
                  strokeWidth="4"
                />
                <path
                  className="opacity-75"
                  fill="currentColor"
                  d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
                />
              </svg>
            )}
            {action.icon && !action.isLoading && (
              <span className="mr-2">{action.icon}</span>
            )}
            {label}
            {action.shortcut && !action.isLoading && (
              <kbd className="border-border bg-muted ml-2.5 hidden rounded-lg border px-2 py-0.5 font-mono text-xs font-medium sm:inline-block">
                {action.shortcut}
              </kbd>
            )}
          </Button>
        );
      })}
    </div>
  );
}

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

components/tool-ui/shared/use-action-buttons.tsx
"use client";

import { useCallback, useEffect, useMemo, useRef, useState } from "react";
import type { Action } from "./schema";

export type UseActionButtonsOptions = {
  actions: Action[];
  onAction: (actionId: string) => void | Promise<void>;
  onBeforeAction?: (actionId: string) => boolean | Promise<boolean>;
  confirmTimeout?: number;
};

export type UseActionButtonsResult = {
  actions: Array<
    Action & {
      currentLabel: string;
      isConfirming: boolean;
      isExecuting: boolean;
      isDisabled: boolean;
      isLoading: boolean;
    }
  >;
  runAction: (actionId: string) => Promise<void>;
  confirmingActionId: string | null;
  executingActionId: string | null;
};

type ActionExecutionLock = {
  tryAcquire: () => boolean;
  release: () => void;
};

export function createActionExecutionLock(): ActionExecutionLock {
  let locked = false;

  return {
    tryAcquire: () => {
      if (locked) return false;
      locked = true;
      return true;
    },
    release: () => {
      locked = false;
    },
  };
}

export function useActionButtons(
  options: UseActionButtonsOptions,
): UseActionButtonsResult {
  const { actions, onAction, onBeforeAction, confirmTimeout = 3000 } = options;

  const [confirmingActionId, setConfirmingActionId] = useState<string | null>(
    null,
  );
  const [executingActionId, setExecutingActionId] = useState<string | null>(
    null,
  );
  const executionLockRef = useRef<ActionExecutionLock>(
    createActionExecutionLock(),
  );

  useEffect(() => {
    if (!confirmingActionId) return;
    const id = setTimeout(() => setConfirmingActionId(null), confirmTimeout);
    return () => clearTimeout(id);
  }, [confirmingActionId, confirmTimeout]);

  useEffect(() => {
    if (!confirmingActionId) return;
    const handleKeyDown = (e: KeyboardEvent) => {
      if (e.key === "Escape") {
        setConfirmingActionId(null);
      }
    };

    window.addEventListener("keydown", handleKeyDown);
    return () => window.removeEventListener("keydown", handleKeyDown);
  }, [confirmingActionId]);

  const runAction = useCallback(
    async (actionId: string) => {
      const action = actions.find((a) => a.id === actionId);
      if (!action) return;

      const isAnyActionExecuting = executingActionId !== null;
      if (action.disabled || action.loading || isAnyActionExecuting) {
        return;
      }

      if (action.confirmLabel && confirmingActionId !== action.id) {
        setConfirmingActionId(action.id);
        return;
      }

      if (!executionLockRef.current.tryAcquire()) {
        return;
      }

      if (onBeforeAction) {
        const shouldProceed = await onBeforeAction(action.id);
        if (!shouldProceed) {
          setConfirmingActionId(null);
          executionLockRef.current.release();
          return;
        }
      }

      try {
        setExecutingActionId(action.id);
        await onAction(action.id);
      } finally {
        executionLockRef.current.release();
        setExecutingActionId(null);
        setConfirmingActionId(null);
      }
    },
    [actions, confirmingActionId, executingActionId, onAction, onBeforeAction],
  );

  const resolvedActions = useMemo(
    () =>
      actions.map((action) => {
        const isConfirming = confirmingActionId === action.id;
        const isThisActionExecuting = executingActionId === action.id;
        const isLoading = action.loading || isThisActionExecuting;
        const isDisabled =
          action.disabled ||
          (executingActionId !== null && !isThisActionExecuting);
        const currentLabel =
          isConfirming && action.confirmLabel
            ? action.confirmLabel
            : action.label;

        return {
          ...action,
          currentLabel,
          isConfirming,
          isExecuting: isThisActionExecuting,
          isDisabled,
          isLoading,
        };
      }),
    [actions, confirmingActionId, executingActionId],
  );

  return {
    actions: resolvedActions,
    runAction,
    confirmingActionId,
    executingActionId,
  };
}

demo.tsx
import MessageDraft from "@/components/ui/message-draft";

export default function Default() {
  return (
    <div className="flex w-full items-center justify-center p-8">
      <MessageDraft
        id="message-draft-email"
        channel="email"
        subject="Updated proposal attached"
        from="sarah.mitchell@acme.co"
        to={["marcus.chen@acme.co"]}
        body={`Hi Marcus,

I've attached the revised proposal with the changes we discussed. The new timeline reflects the Q2 launch date, and I've adjusted the budget breakdown in section 3.

Let me know if you have any questions.

Best,
Sarah`}
        onSend={() => console.log("Message sent")}
        onCancel={() => console.log("Message cancelled")}
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
