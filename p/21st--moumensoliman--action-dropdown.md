<!-- Action dropdown · @moumensoliman · https://21st.dev/@moumensoliman/components/action-dropdown
     license: unspecified · category: dropdown
     A click-driven action picker whose multi-level submenu opens beside the row, jumps straight to the selected item, and drills in place with an animated breadcrumb at a fixed width. -->

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
components/ui/native-action-dropdown-baseui.tsx
"use client";

import { cn } from "@/lib/utils";
import { AnimatePresence, motion, useReducedMotion } from "framer-motion";
import { Check, ChevronDown, ChevronRight } from "lucide-react";
import {
  useCallback,
  useEffect,
  useId,
  useLayoutEffect,
  useMemo,
  useRef,
  useState,
} from "react";

export interface ActionDropdownNode {
  /** Unique id. */
  id: string;
  /** Display name. */
  name: string;
  /** Secondary line shown under the name. */
  detail?: string;
  /** Small pill badge next to the name. */
  meta?: string;
  /** Trigger subtitle shown when this leaf is the value. */
  summary?: string;
  /** Optional root-level group label (sections render in first-seen order). */
  group?: string;
  /** Nested children — a node with children is a submenu parent. */
  children?: ActionDropdownNode[];
}

export interface NativeActionDropdownProps {
  /** The (recursive) option tree. */
  items: ActionDropdownNode[];
  /** Controlled selected id. */
  value?: string;
  /** Uncontrolled initial selected id. */
  defaultValue?: string;
  /** Fired with the selected leaf id. */
  onValueChange?: (id: string) => void;
  /** Panel header title. */
  label?: string;
  /** Panel header subtitle. */
  description?: string;
  /** Trigger text when nothing is selected. */
  placeholder?: string;
  className?: string;
}

// Walk the tree to the node with `id`, returning the root→node chain.
function findChain(
  nodes: ActionDropdownNode[],
  id: string
): ActionDropdownNode[] | null {
  for (const node of nodes) {
    if (node.id === id) return [node];
    if (node.children) {
      const sub = findChain(node.children, id);
      if (sub) return [node, ...sub];
    }
  }
  return null;
}

function resolveSelection(
  items: ActionDropdownNode[],
  id: string,
  placeholder: string
): { name: string; summary: string } {
  const chain = findChain(items, id);
  if (!chain || chain.length === 0) return { name: placeholder, summary: "" };
  const leaf = chain[chain.length - 1];
  const parent = chain.length > 1 ? chain[chain.length - 2] : null;
  return {
    name: leaf.name,
    summary: parent ? parent.name : (leaf.summary ?? ""),
  };
}

// The selected value expressed as a column-stack (one active index per level).
function colsForSelection(
  root: ActionDropdownNode[],
  items: ActionDropdownNode[],
  id: string
): number[] {
  const chain = findChain(items, id);
  if (!chain) return [0];
  const cols: number[] = [];
  let level = root;
  for (const node of chain) {
    const idx = level.findIndex((item) => item.id === node.id);
    if (idx < 0) break;
    cols.push(idx);
    level = node.children ?? [];
  }
  return cols.length ? cols : [0];
}

// Resolve a column-stack into the concrete list + active index per level.
function buildColumns(
  root: ActionDropdownNode[],
  cols: number[]
): { items: ActionDropdownNode[]; active: number }[] {
  const result: { items: ActionDropdownNode[]; active: number }[] = [];
  let level = root;
  for (let d = 0; d < cols.length; d += 1) {
    if (!level.length) break;
    const active = Math.max(0, Math.min(level.length - 1, cols[d] ?? 0));
    result.push({ items: level, active });
    const node = level[active];
    if (!node?.children?.length) break;
    level = node.children;
  }
  if (!result.length) result.push({ items: root, active: 0 });
  return result;
}

// Motion spec — see animations.dev easing blueprint. Submenus enter/exit the
// viewport → ease-out; springs only for the highlight + tactile bits.
const EASE_OUT = [0.22, 1, 0.36, 1] as const;
const PANEL_SPRING = { type: "spring", duration: 0.34, bounce: 0.16 } as const;
const PANEL_EXIT = { duration: 0.13, ease: EASE_OUT } as const;
const HIGHLIGHT_SPRING = {
  type: "spring",
  duration: 0.28,
  bounce: 0.2,
} as const;

// Beside-the-row submenu sizing, used for viewport collision checks.
const SUBMENU_W = 256; // w-64
const GAP = 8; // ml-2 / mr-2
type SubmenuSide = "right" | "left" | "below";

// useLayoutEffect on the client (no flash), useEffect on the server.
const useIsoLayoutEffect =
  typeof window !== "undefined" ? useLayoutEffect : useEffect;

export function NativeActionDropdownBaseUI({
  items,
  value,
  defaultValue,
  onValueChange,
  label = "Choose an option",
  description,
  placeholder = "Select…",
  className,
}: NativeActionDropdownProps) {
  const isControlled = value !== undefined;
  const [internalValue, setInternalValue] = useState(defaultValue ?? "");
  const selectedId = isControlled ? value : internalValue;

  const [isOpen, setIsOpen] = useState(false);
  const [cols, setCols] = useState<number[]>([0]);
  // Collision-aware placement (recomputed on open / drill / resize).
  const [submenuSide, setSubmenuSide] = useState<SubmenuSide>("right");
  const [panelAbove, setPanelAbove] = useState(false);

  const listboxId = useId();
  const shouldReduceMotion = useReducedMotion();
  const reduce = shouldReduceMotion ?? false;

  const triggerRef = useRef<HTMLButtonElement>(null);
  const listboxRef = useRef<HTMLDivElement>(null);

  // Root sections in first-seen order; flat root list keeps that order.
  const groupKeys = useMemo(() => {
    const keys: string[] = [];
    for (const item of items) {
      const key = item.group ?? "";
      if (!keys.includes(key)) keys.push(key);
    }
    return keys;
  }, [items]);
  const hasGroups = groupKeys.length > 1 || (groupKeys[0] ?? "") !== "";
  const orderedModes = useMemo(
    () =>
      groupKeys.flatMap((key) => items.filter((m) => (m.group ?? "") === key)),
    [groupKeys, items]
  );

  const selection = resolveSelection(items, selectedId, placeholder);
  const optionId = useCallback(
    (id: string) => `${listboxId}-${id}`,
    [listboxId]
  );

  const columns = useMemo(
    () => buildColumns(orderedModes, cols),
    [orderedModes, cols]
  );
  const base = useMemo(() => columns.map((column) => column.active), [columns]);
  const depth = columns.length - 1;
  const activeNode = columns[depth].items[base[depth]];

  // Ids on the path to the current value — flags which branches hold it.
  const selectedPath = useMemo(
    () => new Set((findChain(items, selectedId) ?? []).map((n) => n.id)),
    [items, selectedId]
  );

  // Cols for opening a root item's submenu: jump straight to the selected item
  // when it lives in this subtree, otherwise open its first level.
  const colsForRoot = useCallback(
    (rootIndex: number): number[] => {
      const chain = findChain(items, selectedId);
      if (
        chain &&
        chain.length > 1 &&
        chain[0].id === orderedModes[rootIndex]?.id
      ) {
        return colsForSelection(orderedModes, items, selectedId);
      }
      return [rootIndex, 0];
    },
    [items, orderedModes, selectedId]
  );

  const open = useCallback(() => {
    const chain = findChain(items, selectedId);
    const rootIdx = chain
      ? orderedModes.findIndex((mode) => mode.id === chain[0].id)
      : 0;
    setCols([rootIdx < 0 ? 0 : rootIdx]);
    setIsOpen(true);
  }, [items, orderedModes, selectedId]);

  const close = useCallback((returnFocus = true) => {
    setIsOpen(false);
    if (returnFocus) triggerRef.current?.focus();
  }, []);

  const handleSelect = useCallback(
    (id: string) => {
      if (!isControlled) setInternalValue(id);
      onValueChange?.(id);
      close();
    },
    [close, isControlled, onValueChange]
  );

  useEffect(() => {
    if (isOpen) listboxRef.current?.focus();
  }, [isOpen]);

  // Keep the panel and submenu inside the viewport: flip the panel above the
  // trigger near the bottom, and open the submenu left / below when there's no
  // room on the right (full-width "below" doubles as the small-screen layout).
  const computePlacement = useCallback(() => {
    if (typeof window === "undefined") return;
    const vw = window.innerWidth;
    const vh = window.innerHeight;

    const trigger = triggerRef.current?.getBoundingClientRect();
    if (trigger) {
      const roomBelow = vh - trigger.bottom;
      const roomAbove = trigger.top;
      setPanelAbove(
        roomBelow < Math.min(360, roomAbove) && roomAbove > roomBelow
      );
    }

    const panel = listboxRef.current?.getBoundingClientRect();
    if (panel) {
      const need = SUBMENU_W + GAP;
      if (vw < 480 || (vw - panel.right < need && panel.left < need)) {
        setSubmenuSide("below");
      } else if (vw - panel.right >= need) {
        setSubmenuSide("right");
      } else {
        setSubmenuSide("left");
      }
    }
  }, []);

  useIsoLayoutEffect(() => {
    if (!isOpen) return;
    computePlacement();
    window.addEventListener("resize", computePlacement);
    return () => window.removeEventListener("resize", computePlacement);
  }, [isOpen, depth, computePlacement]);

  const handleTriggerKeyDown = (event: React.KeyboardEvent) => {
    if (event.key === "ArrowDown" || event.key === "ArrowUp") {
      event.preventDefault();
      open();
    }
  };

  const handleListboxKeyDown = (event: React.KeyboardEvent) => {
    const last = depth;
    const levelItems = columns[last].items;

    switch (event.key) {
      case "ArrowDown":
        event.preventDefault();
        setCols([
          ...base.slice(0, last),
          Math.min(levelItems.length - 1, base[last] + 1),
        ]);
        break;
      case "ArrowUp":
        event.preventDefault();
        setCols([...base.slice(0, last), Math.max(0, base[last] - 1)]);
        break;
      case "Home":
        event.preventDefault();
        setCols([...base.slice(0, last), 0]);
        break;
      case "End":
        event.preventDefault();
        setCols([...base.slice(0, last), levelItems.length - 1]);
        break;
      case "ArrowRight":
        if (activeNode?.children?.length) {
          event.preventDefault();
          setCols([...base, 0]);
        }
        break;
      case "ArrowLeft":
        if (depth > 0) {
          event.preventDefault();
          setCols(base.slice(0, -1));
        }
        break;
      case "Enter":
      case " ":
        event.preventDefault();
        if (activeNode?.children?.length) setCols([...base, 0]);
        else if (activeNode) handleSelect(activeNode.id);
        break;
      case "Escape":
        event.preventDefault();
        if (depth > 0) setCols(base.slice(0, -1));
        else close();
        break;
      case "Tab":
        close(false);
        break;
      default:
        break;
    }
  };

  // Click drives everything: a parent opens its submenu (the root jumps
  // straight to the selected item if it lives in that subtree); a leaf selects.
  const handleClick = (columnIndex: number, itemIndex: number) => {
    const node = columns[columnIndex].items[itemIndex];
    if (node?.children?.length) {
      setCols(
        columnIndex === 0
          ? colsForRoot(itemIndex)
          : [...base.slice(0, columnIndex), itemIndex, 0]
      );
    } else if (node) {
      handleSelect(node.id);
    }
  };

  const renderOption = (
    node: ActionDropdownNode,
    columnIndex: number,
    itemIndex: number,
    staggerIndex?: number
  ) => {
    const hasChildren = (node.children?.length ?? 0) > 0;
    const isSelected = selectedId === node.id;
    const isActive = base[columnIndex] === itemIndex;
    const isOpenParent =
      isActive && hasChildren && columns.length > columnIndex + 1;
    const onSelectedPath = hasChildren && selectedPath.has(node.id);
    const staggered = staggerIndex !== undefined && !reduce;

    return (
      <motion.button
        id={optionId(node.id)}
        type="button"
        role={hasChildren ? "menuitem" : "menuitemradio"}
        aria-haspopup={hasChildren ? "menu" : undefined}
        aria-expanded={hasChildren ? isOpenParent : undefined}
        aria-checked={hasChildren ? undefined : isSelected}
        tabIndex={-1}
        onClick={() => handleClick(columnIndex, itemIndex)}
        whileTap={reduce ? undefined : { scale: 0.99 }}
        initial={staggered ? { opacity: 0, y: 3 } : false}
        animate={staggered ? { opacity: 1, y: 0 } : undefined}
        transition={
          staggered
            ? {
                delay: 0.04 + staggerIndex * 0.03,
                duration: 0.16,
                ease: EASE_OUT,
              }
            : undefined
        }
        className="group relative block w-full cursor-pointer rounded-lg px-2.5 py-2 text-left outline-none transition-colors hover:bg-accent/40"
      >
        {isActive ? (
          <motion.span
            layoutId={`${listboxId}-hl-${columnIndex}`}
            aria-hidden="true"
            transition={reduce ? { duration: 0 } : HIGHLIGHT_SPRING}
            className="pointer-events-none absolute inset-0 rounded-lg bg-accent"
          />
        ) : null}

        <span className="relative z-10 flex items-start gap-2">
          <span className="min-w-0 flex-1">
            <span className="flex items-center gap-2">
              <span className="truncate text-sm font-medium text-foreground">
                {node.name}
              </span>
              {node.meta ? (
                <span className="shrink-0 rounded-full border border-border bg-background px-1.5 py-px text-[10px] font-medium leading-4 text-muted-foreground">
                  {node.meta}
                </span>
              ) : null}
            </span>
            {node.detail ? (
              <span className="mt-0.5 block text-[11px] leading-4 text-muted-foreground">
                {node.detail}
              </span>
            ) : null}
          </span>

          <span className="mt-0.5 flex h-5 min-w-5 shrink-0 items-center justify-end gap-1">
            {hasChildren ? (
              <>
                {onSelectedPath ? (
                  <span
                    className="h-1.5 w-1.5 rounded-full bg-primary"
                    aria-hidden="true"
                  />
                ) : null}
                <ChevronRight
                  className={cn(
                    "h-4 w-4 transition-colors",
                    isOpenParent ? "text-foreground" : "text-muted-foreground"
                  )}
                  aria-hidden="true"
                />
              </>
            ) : (
              <AnimatePresence initial={false}>
                {isSelected ? (
                  <motion.span
                    key="check"
                    initial={reduce ? false : { opacity: 0, scale: 0.5 }}
                    animate={{ opacity: 1, scale: 1 }}
                    exit={reduce ? undefined : { opacity: 0, scale: 0.5 }}
                    transition={
                      reduce
                        ? { duration: 0 }
                        : { type: "spring", duration: 0.3, bounce: 0.45 }
                    }
                    className="text-primary"
                  >
                    <Check className="h-4 w-4" aria-hidden="true" />
                  </motion.span>
                ) : null}
              </AnimatePresence>
            )}
          </span>
        </span>
      </motion.button>
    );
  };

  // One bounded submenu beside the open root row. It drills in place; the path
  // is shown as a breadcrumb (not stacked cards), so depth never widens it.
  // On small screens ("below") it renders IN FLOW as an accordion so it pushes
  // the remaining rows down instead of covering them.
  const renderSubmenu = () => {
    const parent =
      depth > 0 ? columns[depth - 1].items[base[depth - 1]] : undefined;
    const isInline = submenuSide === "below";

    if (isInline) {
      return (
        <motion.div
          key="submenu"
          role="group"
          aria-label={parent ? `${parent.name} options` : "Submenu"}
          initial={reduce ? { opacity: 0 } : { opacity: 0, height: 0 }}
          animate={reduce ? { opacity: 1 } : { opacity: 1, height: "auto" }}
          exit={
            reduce
              ? { opacity: 0, transition: { duration: 0 } }
              : {
                  opacity: 0,
                  height: 0,
                  transition: { duration: 0.14, ease: EASE_OUT },
                }
          }
          transition={
            reduce ? { duration: 0 } : { duration: 0.18, ease: EASE_OUT }
          }
          className="overflow-hidden"
        >
          <div className="mt-1 w-full rounded-xl border border-border bg-popover p-1 text-popover-foreground shadow-sm ring-1 ring-black/[0.02]">
            {renderSubmenuInner()}
          </div>
        </motion.div>
      );
    }

    const sideClass =
      submenuSide === "right"
        ? "left-full top-0 ml-2 w-64"
        : "right-full top-0 mr-2 w-64";
    const sideOrigin = submenuSide === "right" ? "left top" : "right top";
    const enterX = reduce ? 0 : submenuSide === "left" ? 6 : -6;
    return (
      <motion.div
        key="submenu"
        role="group"
        aria-label={parent ? `${parent.name} options` : "Submenu"}
        initial={{ opacity: 0, x: enterX, scale: reduce ? 1 : 0.98 }}
        animate={{ opacity: 1, x: 0, scale: 1 }}
        exit={{
          opacity: 0,
          x: enterX === 0 ? 0 : enterX > 0 ? 4 : -4,
          scale: reduce ? 1 : 0.98,
          transition: reduce
            ? { duration: 0 }
            : { duration: 0.1, ease: EASE_OUT },
        }}
        transition={
          reduce ? { duration: 0 } : { duration: 0.14, ease: EASE_OUT }
        }
        style={{ transformOrigin: sideOrigin }}
        className={cn(
          "absolute z-50 rounded-xl border border-border bg-popover p-1 text-popover-foreground shadow-xl ring-1 ring-black/[0.02]",
          sideClass
        )}
      >
        {renderSubmenuInner()}
      </motion.div>
    );
  };

  const renderSubmenuInner = () => (
    <>
      <div className="flex flex-wrap items-center gap-x-0.5 gap-y-0.5 px-1.5 pb-1.5 pt-1">
          <AnimatePresence initial={false} mode="popLayout">
            {Array.from({ length: depth }).map((_, level) => {
              const crumb = columns[level].items[base[level]];
              const isLast = level === depth - 1;
              return (
                <motion.span
                  key={crumb?.id ?? level}
                  initial={reduce ? false : { opacity: 0, x: -4 }}
                  animate={{ opacity: 1, x: 0 }}
                  exit={
                    reduce
                      ? { opacity: 0 }
                      : { opacity: 0, x: -4, transition: { duration: 0.1 } }
                  }
                  transition={
                    reduce
                      ? { duration: 0 }
                      : { duration: 0.14, ease: EASE_OUT }
                  }
                  className="flex items-center gap-x-0.5"
                >
                  {level > 0 ? (
                    <ChevronRight
                      className="h-3 w-3 shrink-0 text-muted-foreground/50"
                      aria-hidden="true"
                    />
                  ) : null}
                  {isLast ? (
                    <span className="truncate text-[11px] font-semibold text-foreground">
                      {crumb?.name}
                    </span>
                  ) : (
                    <button
                      type="button"
                      role="menuitem"
                      tabIndex={-1}
                      aria-label={`Go to ${crumb?.name ?? ""}`}
                      onClick={() => setCols(base.slice(0, level + 2))}
                      className="truncate rounded text-[11px] font-medium text-muted-foreground outline-none transition-colors hover:text-foreground focus-visible:text-foreground"
                    >
                      {crumb?.name}
                    </button>
                  )}
                </motion.span>
              );
            })}
          </AnimatePresence>
        </div>

        <div className="relative max-h-[min(60vh,18rem)] overflow-y-auto border-t border-border pt-1">
          <AnimatePresence mode="popLayout" initial={false}>
            <motion.div
              key={depth}
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              exit={{
                opacity: 0,
                transition: { duration: reduce ? 0 : 0.08, ease: EASE_OUT },
              }}
              transition={
                reduce ? { duration: 0 } : { duration: 0.12, ease: EASE_OUT }
              }
            >
              {columns[depth].items.map((node, itemIndex) =>
                renderOption(node, depth, itemIndex, itemIndex)
              )}
            </motion.div>
          </AnimatePresence>
        </div>
    </>
  );

  const renderRootRow = (mode: ActionDropdownNode, flatIndex: number) => {
    const submenuOpen =
      base[0] === flatIndex && depth >= 1 && (mode.children?.length ?? 0) > 0;
    return (
      <div key={mode.id} className="relative">
        {renderOption(mode, 0, flatIndex, flatIndex)}
        <AnimatePresence>
          {submenuOpen ? renderSubmenu() : null}
        </AnimatePresence>
      </div>
    );
  };

  return (
    <div className={cn("relative w-full max-w-xs", className)}>
      <motion.button
        ref={triggerRef}
        type="button"
        onClick={() => (isOpen ? close(false) : open())}
        onKeyDown={handleTriggerKeyDown}
        whileTap={reduce ? undefined : { scale: 0.985 }}
        transition={{ duration: 0.12, ease: EASE_OUT }}
        aria-expanded={isOpen}
        aria-haspopup="menu"
        aria-controls={isOpen ? listboxId : undefined}
        className="flex w-full items-center gap-3 cursor-pointer rounded-lg border border-border bg-card px-4 py-2.5 text-left shadow-sm transition-colors hover:bg-accent/40 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 focus-visible:ring-offset-background"
      >
        <span className="min-w-0 flex-1">
          <span className="block truncate text-sm font-medium">
            {selection.name}
          </span>
          {selection.summary ? (
            <span className="mt-0.5 block truncate text-[11px] leading-4 text-muted-foreground">
              {selection.summary}
            </span>
          ) : null}
        </span>
        <motion.span
          aria-hidden="true"
          animate={reduce ? undefined : { rotate: isOpen ? 180 : 0 }}
          transition={{ duration: 0.18, ease: EASE_OUT }}
          className="shrink-0 text-muted-foreground"
        >
          <ChevronDown className="h-4 w-4" />
        </motion.span>
      </motion.button>

      <AnimatePresence>
        {isOpen ? (
          <>
            <button
              type="button"
              aria-label="Close dropdown"
              tabIndex={-1}
              className="fixed inset-0 z-40 cursor-default bg-transparent"
              onClick={() => close()}
            />

            <motion.div
              ref={listboxRef}
              id={listboxId}
              role="menu"
              tabIndex={-1}
              aria-label={label}
              aria-activedescendant={
                activeNode ? optionId(activeNode.id) : undefined
              }
              onKeyDown={handleListboxKeyDown}
              initial={{
                opacity: 0,
                y: reduce ? 0 : panelAbove ? 6 : -6,
                scale: reduce ? 1 : 0.97,
              }}
              animate={{ opacity: 1, y: 0, scale: 1 }}
              exit={{
                opacity: 0,
                y: reduce ? 0 : panelAbove ? 4 : -4,
                scale: reduce ? 1 : 0.98,
                transition: reduce ? { duration: 0 } : PANEL_EXIT,
              }}
              transition={reduce ? { duration: 0 } : PANEL_SPRING}
              style={{
                transformOrigin: panelAbove ? "bottom center" : "top center",
              }}
              className={cn(
                "absolute left-0 z-50 w-full rounded-xl border border-border bg-popover p-1.5 text-popover-foreground shadow-xl outline-none ring-1 ring-black/[0.02]",
                panelAbove
                  ? "bottom-full mb-2 origin-bottom"
                  : "top-full mt-2 origin-top"
              )}
            >
              <div role="none" className="px-2 pb-1.5 pt-2">
                <p className="text-sm font-medium">{label}</p>
                {description ? (
                  <p className="mt-0.5 text-[11px] leading-4 text-muted-foreground">
                    {description}
                  </p>
                ) : null}
              </div>

              {hasGroups ? (
                groupKeys.map((group) => (
                  <div
                    key={group || "ungrouped"}
                    role="group"
                    aria-label={group || undefined}
                    className="pb-1 pt-1.5"
                  >
                    {group ? (
                      <div
                        aria-hidden="true"
                        className="px-2 pb-1 text-[10px] font-medium uppercase tracking-[0.14em] text-muted-foreground"
                      >
                        {group}
                      </div>
                    ) : null}
                    {orderedModes
                      .map((mode, flatIndex) => ({ mode, flatIndex }))
                      .filter(({ mode }) => (mode.group ?? "") === group)
                      .map(({ mode, flatIndex }) =>
                        renderRootRow(mode, flatIndex)
                      )}
                  </div>
                ))
              ) : (
                <div role="none" className="pb-1 pt-1">
                  {orderedModes.map((mode, flatIndex) =>
                    renderRootRow(mode, flatIndex)
                  )}
                </div>
              )}

              <div
                role="none"
                className="mt-1 flex items-center justify-between border-t border-border px-2 pb-1 pt-2 text-[11px] leading-4 text-muted-foreground"
              >
                <span className="truncate">
                  Current:{" "}
                  <span className="font-medium text-foreground">
                    {selection.name}
                  </span>
                </span>
                <span
                  aria-hidden="true"
                  className="hidden shrink-0 items-center gap-1 sm:flex"
                >
                  {["↑↓", "←", "→", "↵"].map((key) => (
                    <kbd
                      key={key}
                      className="rounded border border-border bg-background px-1 font-sans text-[10px]"
                    >
                      {key}
                    </kbd>
                  ))}
                </span>
              </div>
            </motion.div>
          </>
        ) : null}
      </AnimatePresence>
    </div>
  );
}

demo.tsx
"use client";

import {
  type ActionDropdownNode,
  NativeActionDropdown,
} from "@/components/ui/action-dropdown";

const actionModes: ActionDropdownNode[] = [
  {
    id: "design-review",
    name: "Design review",
    summary: "Balanced UI critique",
    detail: "Checks hierarchy, spacing, contrast, motion, and product clarity.",
    meta: "Best default",
    group: "Suggested",
  },
  {
    id: "quick-draft",
    name: "Quick draft",
    summary: "Fast component pass",
    detail: "Produces a smaller, shippable idea with fewer edge-case notes.",
    meta: "Fast",
    group: "Suggested",
  },
  {
    id: "deep-audit",
    name: "Deep audit",
    detail: "Detailed interaction QA across multiple areas.",
    meta: "3 areas",
    group: "More modes",
    children: [
      {
        id: "audit-a11y",
        name: "Accessibility",
        detail: "Keyboard, focus, and ARIA.",
        children: [
          {
            id: "a11y-keyboard",
            name: "Keyboard flow",
            detail: "Tab order and shortcuts.",
            children: [
              {
                id: "kbd-tab",
                name: "Tab order",
                detail: "Logical sequence of focus stops.",
              },
              {
                id: "kbd-shortcuts",
                name: "Shortcuts",
                detail: "Discoverable key bindings.",
                children: [
                  { id: "kbd-global", name: "Global", detail: "App-wide bindings." },
                  {
                    id: "kbd-scoped",
                    name: "Scoped",
                    detail: "Context-specific bindings.",
                  },
                ],
              },
            ],
          },
          {
            id: "a11y-focus",
            name: "Focus order",
            detail: "Visible focus and traps.",
            children: [
              {
                id: "focus-rings",
                name: "Focus rings",
                detail: "Visible focus indicators.",
              },
              {
                id: "focus-traps",
                name: "Focus traps",
                detail: "Containment inside overlays.",
              },
            ],
          },
          { id: "a11y-aria", name: "ARIA roles", detail: "Roles, names, and states." },
          {
            id: "a11y-contrast",
            name: "Color contrast",
            detail: "WCAG contrast ratios.",
          },
        ],
      },
      {
        id: "audit-perf",
        name: "Performance",
        detail: "Re-renders and runtime cost.",
        children: [
          {
            id: "perf-render",
            name: "Render cost",
            detail: "Re-render frequency and depth.",
          },
          {
            id: "perf-bundle",
            name: "Bundle size",
            detail: "Shipped JavaScript weight.",
            children: [
              {
                id: "bundle-split",
                name: "Code splitting",
                detail: "Route and component chunks.",
              },
              {
                id: "bundle-shake",
                name: "Tree shaking",
                detail: "Dead-code elimination.",
              },
            ],
          },
          {
            id: "perf-runtime",
            name: "Runtime profiling",
            detail: "Flame charts and hot paths.",
          },
        ],
      },
      {
        id: "audit-responsive",
        name: "Responsive",
        detail: "Breakpoints and touch.",
        children: [
          { id: "resp-break", name: "Breakpoints", detail: "Layout at each size." },
          { id: "resp-touch", name: "Touch targets", detail: "Hit-area sizing." },
          {
            id: "resp-container",
            name: "Container queries",
            detail: "Component-driven layout.",
          },
        ],
      },
    ],
  },
  {
    id: "ship-check",
    name: "Ship check",
    detail: "Final release scan before publishing.",
    meta: "2 stages",
    group: "More modes",
    children: [
      {
        id: "ship-premerge",
        name: "Pre-merge",
        detail: "Checks before merging.",
        children: [
          { id: "pre-lint", name: "Lint & types", detail: "Static analysis gate." },
          { id: "pre-visual", name: "Visual diff", detail: "Snapshot comparison." },
        ],
      },
      {
        id: "ship-prerelease",
        name: "Pre-release",
        detail: "Final pre-publish pass.",
        children: [
          {
            id: "rel-changelog",
            name: "Changelog",
            detail: "Summarize user-facing changes.",
          },
          {
            id: "rel-version",
            name: "Version bump",
            detail: "Choose the semver increment.",
            children: [
              { id: "ver-patch", name: "Patch", detail: "Backward-compatible fixes." },
              {
                id: "ver-minor",
                name: "Minor",
                detail: "Backward-compatible features.",
              },
              { id: "ver-major", name: "Major", detail: "Breaking changes." },
            ],
          },
        ],
      },
    ],
  },
  {
    id: "code-health",
    name: "Code health",
    detail: "Maintainability metrics.",
    meta: "New",
    group: "More modes",
    children: [
      {
        id: "health-complexity",
        name: "Complexity",
        detail: "Function and module complexity.",
        children: [
          { id: "cx-cyclomatic", name: "Cyclomatic", detail: "Branch count per unit." },
          {
            id: "cx-cognitive",
            name: "Cognitive",
            detail: "Human-perceived difficulty.",
          },
        ],
      },
      { id: "health-duplication", name: "Duplication", detail: "Copy-paste detection." },
      {
        id: "health-coverage",
        name: "Coverage",
        detail: "Test coverage breakdown.",
        children: [
          { id: "cov-unit", name: "Unit", detail: "Isolated logic tests." },
          {
            id: "cov-integration",
            name: "Integration",
            detail: "Cross-module tests.",
          },
          { id: "cov-e2e", name: "End-to-end", detail: "Full-flow tests." },
        ],
      },
    ],
  },
];

export default function DemoOne() {
  return (
    <div className="flex min-h-[460px] w-full items-start justify-center bg-background p-4 pt-10">
      <NativeActionDropdown
        items={actionModes}
        defaultValue="kbd-tab"
        label="Choose action mode"
        description="Click a mode with a chevron to open its submenu beside it."
      />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion lucide-react
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
