<!-- Charts · @edwinvakayil · https://21st.dev/@edwinvakayil/components/charts
     license: unspecified · category: carousel
     There are charts component -->

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
components/ui/charts.tsx
"use client";

import { motion } from "motion/react";
import * as React from "react";
import type { TooltipValueType } from "recharts";
import * as RechartsPrimitive from "recharts";

import { cn } from "@/lib/utils";

// Format: { THEME_NAME: CSS_SELECTOR }
const THEMES = { light: "", dark: ".dark" } as const;

export type ChartConfig = Record<
  string,
  {
    label?: React.ReactNode;
    icon?: React.ComponentType;
  } & (
    | { color?: string; theme?: never }
    | { color?: never; theme: Record<keyof typeof THEMES, string> }
  )
>;

function getPayloadConfigFromPayload(
  config: ChartConfig,
  payload: unknown,
  key: string
) {
  if (typeof payload !== "object" || payload === null) {
    return undefined;
  }

  const payloadPayload =
    "payload" in payload &&
    typeof payload.payload === "object" &&
    payload.payload !== null
      ? payload.payload
      : undefined;

  let configLabelKey: string = key;

  if (
    key in payload &&
    typeof payload[key as keyof typeof payload] === "string"
  ) {
    configLabelKey = payload[key as keyof typeof payload] as string;
  } else if (
    payloadPayload &&
    key in payloadPayload &&
    typeof payloadPayload[key as keyof typeof payloadPayload] === "string"
  ) {
    configLabelKey = payloadPayload[
      key as keyof typeof payloadPayload
    ] as string;
  }

  return configLabelKey in config ? config[configLabelKey] : config[key];
}

function getChartConfigKey(config: ChartConfig, payload: unknown, key: string) {
  if (typeof payload !== "object" || payload === null) {
    return key;
  }

  const payloadPayload =
    "payload" in payload &&
    typeof payload.payload === "object" &&
    payload.payload !== null
      ? payload.payload
      : undefined;

  let configLabelKey: string = key;

  if (
    key in payload &&
    typeof payload[key as keyof typeof payload] === "string"
  ) {
    configLabelKey = payload[key as keyof typeof payload] as string;
  } else if (
    payloadPayload &&
    key in payloadPayload &&
    typeof payloadPayload[key as keyof typeof payloadPayload] === "string"
  ) {
    configLabelKey = payloadPayload[
      key as keyof typeof payloadPayload
    ] as string;
  }

  if (configLabelKey in config) {
    return configLabelKey;
  }

  return key in config ? key : key;
}

function normalizeChartField(
  value: string | number | ((obj: unknown) => unknown) | undefined
) {
  return typeof value === "function" ? undefined : value;
}

function getChartSeriesKey(item: unknown, nameKey?: string) {
  if (typeof item !== "object" || item === null) {
    return nameKey ?? "value";
  }

  const record = item as {
    dataKey?: string | number | ((obj: unknown) => unknown);
    name?: string | number;
    value?: unknown;
    payload?: object;
  };

  if (nameKey) {
    const payloadRecord =
      record.payload && typeof record.payload === "object"
        ? (record.payload as Record<string, unknown>)
        : (record as Record<string, unknown>);

    if (
      nameKey in payloadRecord &&
      payloadRecord[nameKey] != null &&
      payloadRecord[nameKey] !== ""
    ) {
      return String(payloadRecord[nameKey]);
    }
  }

  const dataKey = normalizeChartField(record.dataKey);
  const value =
    typeof record.value === "string" || typeof record.value === "number"
      ? record.value
      : undefined;

  return String(dataKey ?? record.name ?? value ?? "value");
}

function getChartLegendLabel(
  item: unknown,
  itemConfig: ChartConfig[string] | undefined
) {
  if (typeof item !== "object" || item === null) {
    return null;
  }

  const record = item as {
    dataKey?: string | number | ((obj: unknown) => unknown);
    name?: string | number;
    value?: unknown;
  };
  if (itemConfig?.label != null) {
    return itemConfig.label;
  }

  if (record.name != null) {
    return String(record.name);
  }

  if (typeof record.value === "string") {
    return record.value;
  }

  if (record.dataKey != null && typeof record.dataKey !== "function") {
    return String(record.dataKey);
  }

  return null;
}

function getChartLegendSwatchColor(
  configKey: string,
  itemConfig: ChartConfig[string] | undefined,
  fallbackColor?: string
) {
  if (itemConfig) {
    return `var(--color-${configKey})`;
  }

  return fallbackColor;
}

const componentThemeClassName =
  "[--ic-background:#ffffff] [--ic-foreground:#111111] [--ic-primary:#111111] [--ic-secondary:#646b75] [--ic-surface-border:#e9edf2] [--ic-border:#e3e7ec] [--ic-card:#ffffff] [--ic-card-foreground:#111111] [--ic-muted:#f5f7fa] [--ic-muted-foreground:#6d7480] [--ic-accent:#f3f5f8] [--color-accent:var(--ic-accent)] [--color-accent-foreground:var(--ic-accent-foreground)] [--ic-accent-foreground:#111111] [--ic-input:#e3e7ec] [--ic-ring:rgba(17,17,17,0.16)] [--ic-destructive:#dc2626] [--ic-paper:#fcfcfd] [--ic-popover-foreground:#111111] [--ic-brand:#0ea5e9] [--ic-brand-soft:#bae6fd] [--ic-shadow-soft:0_18px_38px_-24px_rgba(15,23,42,0.35)] [--ic-chart-1:#1d4ed8] [--ic-chart-2:#7dd3fc] [--ic-chart-3:oklch(0.42_0.16_262)] [--ic-chart-4:oklch(0.84_0.07_228)] [--ic-chart-5:oklch(0.62_0.14_240)] [--color-background:var(--ic-background)] [--color-foreground:var(--ic-foreground)] [--color-primary:var(--ic-primary)] [--color-secondary:var(--ic-secondary)] [--color-border:var(--ic-border)] [--color-card:var(--ic-card)] [--color-card-foreground:var(--ic-card-foreground)] [--color-muted:var(--ic-muted)] [--color-muted-foreground:var(--ic-muted-foreground)] [--color-accent:var(--ic-accent)] [--color-accent-foreground:var(--ic-accent-foreground)] [--color-input:var(--ic-input)] [--color-ring:var(--ic-ring)] [--color-destructive:var(--ic-destructive)] [--color-paper:var(--ic-paper)] [--color-popover-foreground:var(--ic-popover-foreground)] [--color-brand:var(--ic-brand)] [--color-brand-soft:var(--ic-brand-soft)] [--color-chart-1:var(--ic-chart-1)] [--color-chart-2:var(--ic-chart-2)] [--color-chart-3:var(--ic-chart-3)] [--color-chart-4:var(--ic-chart-4)] [--color-chart-5:var(--ic-chart-5)] [--chart-1:var(--ic-chart-1)] [--chart-2:var(--ic-chart-2)] [--chart-3:var(--ic-chart-3)] [--chart-4:var(--ic-chart-4)] [--chart-5:var(--ic-chart-5)] dark:[--ic-background:#111111] dark:[--ic-foreground:#f6f3ec] dark:[--ic-primary:#f6f3ec] dark:[--ic-secondary:#cbc6bb] dark:[--ic-surface-border:#2a2a25] dark:[--ic-border:#2b2a25] dark:[--ic-card:#111111] dark:[--ic-card-foreground:#f6f3ec] dark:[--ic-muted:#171716] dark:[--ic-muted-foreground:#9a958a] dark:[--ic-accent:#1a1a18] [--color-accent:var(--ic-accent)] [--color-accent-foreground:var(--ic-accent-foreground)] dark:[--ic-accent-foreground:#f6f3ec] dark:[--ic-input:#2b2a25] dark:[--ic-ring:rgba(246,243,236,0.18)] dark:[--ic-destructive:#f87171] dark:[--ic-paper:#171716] dark:[--ic-popover-foreground:#f6f3ec] dark:[--ic-brand:#38bdf8] dark:[--ic-brand-soft:#0c4a6e] dark:[--ic-shadow-soft:0_20px_44px_-28px_rgba(0,0,0,0.6)] dark:[--ic-chart-1:#60a5fa] dark:[--ic-chart-2:#bfdbfe] dark:[--ic-chart-3:oklch(0.58_0.15_260)] dark:[--ic-chart-4:oklch(0.75_0.12_235)] dark:[--ic-chart-5:oklch(0.88_0.06_220)]";

/** Calm easing — no spring overshoot on data surfaces. */
const chartEase = [0.22, 1, 0.36, 1] as const;

const chartSurfaceFade = {
  duration: 0.22,
  ease: chartEase,
};

const chartTooltipFade = {
  duration: 0.14,
  ease: chartEase,
};

const CHART_SERIES_ANIMATION_DURATION = 480;
const CHART_SERIES_ANIMATION_EASING = "ease-out" as const;
const CHART_SERIES_STAGGER_MS = 32;

function getChartAnimationTimeoutMs(seriesCount: number) {
  const safeSeriesCount = Math.max(1, seriesCount);

  return (
    CHART_SERIES_ANIMATION_DURATION +
    CHART_SERIES_STAGGER_MS * Math.max(0, safeSeriesCount - 1)
  );
}

const MotionDiv = motion.div;

type MotionSafeDivProps = Omit<
  React.HTMLAttributes<HTMLDivElement>,
  | "onAnimationEnd"
  | "onAnimationIteration"
  | "onAnimationStart"
  | "onDrag"
  | "onDragEnd"
  | "onDragEnter"
  | "onDragExit"
  | "onDragLeave"
  | "onDragOver"
  | "onDragStart"
  | "onDrop"
>;

type TooltipNameType = number | string;

const CHART_RESIZE_DEBOUNCE_MS = 150;

type ChartContextProps = {
  config: ChartConfig;
  chartAnimationActive: boolean;
};

const ChartContext = React.createContext<ChartContextProps | null>(null);

function useChart() {
  const context = React.useContext(ChartContext);

  if (!context) {
    throw new Error("useChart must be used within a <ChartContainer />");
  }

  return context;
}

function ChartStyle({ id, config }: { id: string; config: ChartConfig }) {
  const colorConfig = Object.entries(config).filter(
    ([, config]) => config.theme ?? config.color
  );

  if (!colorConfig.length) {
    return null;
  }

  return (
    <style
      // biome-ignore lint/security/noDangerouslySetInnerHtml: scoped chart color variables from trusted ChartConfig
      dangerouslySetInnerHTML={{
        __html: Object.entries(THEMES)
          .map(
            ([theme, prefix]) => `
${prefix} [data-chart="${id}"] {
${colorConfig
  .map(([key, itemConfig]) => {
    const color =
      itemConfig.theme?.[theme as keyof typeof itemConfig.theme] ??
      itemConfig.color;
    return color ? `  --color-${key}: ${color};` : null;
  })
  .join("\n")}
}
`
          )
          .join("\n"),
      }}
    />
  );
}

function ChartContainer({
  id,
  className,
  children,
  config,
  initialDimension,
  seriesCount,
  ...props
}: MotionSafeDivProps & {
  config: ChartConfig;
  children: React.ComponentProps<
    typeof RechartsPrimitive.ResponsiveContainer
  >["children"];
  initialDimension?: {
    width: number;
    height: number;
  };
  /** Overrides inferred series count for animation timing when config keys do not match plotted series. */
  seriesCount?: number;
}) {
  const uniqueId = React.useId();
  const chartId = `chart-${id ?? uniqueId.replace(/:/g, "")}`;
  const initialChartAnimationDoneRef = React.useRef(false);
  const resizeMeasureCountRef = React.useRef(0);
  const [chartAnimationActive, setChartAnimationActive] = React.useState(true);
  const resolvedSeriesCount = seriesCount ?? Object.keys(config).length;

  React.useEffect(() => {
    const timeoutId = window.setTimeout(() => {
      initialChartAnimationDoneRef.current = true;
      setChartAnimationActive(false);
    }, getChartAnimationTimeoutMs(resolvedSeriesCount));

    return () => {
      window.clearTimeout(timeoutId);
    };
  }, [resolvedSeriesCount]);

  const handleResize = React.useCallback(() => {
    resizeMeasureCountRef.current += 1;

    if (resizeMeasureCountRef.current <= 1) {
      return;
    }

    if (initialChartAnimationDoneRef.current) {
      setChartAnimationActive(false);
    }
  }, []);

  return (
    <ChartContext.Provider
      value={{
        config,
        chartAnimationActive,
      }}
    >
      <MotionDiv
        animate={{ opacity: 1 }}
        className={cn(
          componentThemeClassName,
          "flex aspect-video min-h-[12rem] w-full justify-center text-xs [&_.recharts-cartesian-axis-tick_text]:fill-muted-foreground [&_.recharts-cartesian-grid_line[stroke='#ccc']]:stroke-border/50 [&_.recharts-curve.recharts-tooltip-cursor]:stroke-border [&_.recharts-dot[stroke='#fff']]:stroke-transparent [&_.recharts-layer]:outline-hidden [&_.recharts-polar-grid_[stroke='#ccc']]:stroke-border [&_.recharts-radial-bar-background-sector]:fill-muted [&_.recharts-rectangle.recharts-tooltip-cursor]:fill-muted [&_.recharts-reference-line_[stroke='#ccc']]:stroke-border [&_.recharts-sector[stroke='#fff']]:stroke-transparent [&_.recharts-sector]:outline-hidden [&_.recharts-surface]:outline-hidden",
          className
        )}
        data-chart={chartId}
        data-slot="chart"
        initial={{ opacity: 0 }}
        transition={chartSurfaceFade}
        {...props}
      >
        <ChartStyle config={config} id={chartId} />
        <RechartsPrimitive.ResponsiveContainer
          debounce={CHART_RESIZE_DEBOUNCE_MS}
          height="100%"
          onResize={handleResize}
          width="100%"
          {...(initialDimension ? { initialDimension } : {})}
        >
          {children}
        </RechartsPrimitive.ResponsiveContainer>
      </MotionDiv>
    </ChartContext.Provider>
  );
}

function useChartSeriesAnimation(seriesIndex = 0) {
  const { chartAnimationActive } = useChart();

  return {
    animationBegin: seriesIndex * CHART_SERIES_STAGGER_MS,
    animationDuration: chartAnimationActive
      ? CHART_SERIES_ANIMATION_DURATION
      : 0,
    animationEasing: CHART_SERIES_ANIMATION_EASING,
    isAnimationActive: chartAnimationActive,
  };
}

function ChartBar({
  animationBegin,
  animationDuration,
  animationEasing,
  isAnimationActive,
  seriesIndex = 0,
  ...props
}: React.ComponentProps<typeof RechartsPrimitive.Bar> & {
  /** Staggers bar growth when plotting multiple series (0, 1, 2, …). */
  seriesIndex?: number;
}) {
  const defaults = useChartSeriesAnimation(seriesIndex);
  const shouldAnimate = isAnimationActive ?? defaults.isAnimationActive;

  return (
    <RechartsPrimitive.Bar
      {...props}
      animationBegin={animationBegin ?? defaults.animationBegin}
      animationDuration={
        shouldAnimate ? (animationDuration ?? defaults.animationDuration) : 0
      }
      animationEasing={animationEasing ?? defaults.animationEasing}
      isAnimationActive={shouldAnimate}
    />
  );
}

function ChartLine({
  animationBegin,
  animationDuration,
  animationEasing,
  isAnimationActive,
  seriesIndex = 0,
  ...props
}: React.ComponentProps<typeof RechartsPrimitive.Line> & {
  seriesIndex?: number;
}) {
  const defaults = useChartSeriesAnimation(seriesIndex);
  const shouldAnimate = isAnimationActive ?? defaults.isAnimationActive;

  return (
    <RechartsPrimitive.Line
      {...props}
      animationBegin={animationBegin ?? defaults.animationBegin}
      animationDuration={
        shouldAnimate ? (animationDuration ?? defaults.animationDuration) : 0
      }
      animationEasing={animationEasing ?? defaults.animationEasing}
      isAnimationActive={shouldAnimate}
    />
  );
}

function ChartArea({
  animationBegin,
  animationDuration,
  animationEasing,
  isAnimationActive,
  seriesIndex = 0,
  ...props
}: React.ComponentProps<typeof RechartsPrimitive.Area> & {
  seriesIndex?: number;
}) {
  const defaults = useChartSeriesAnimation(seriesIndex);
  const shouldAnimate = isAnimationActive ?? defaults.isAnimationActive;

  return (
    <RechartsPrimitive.Area
      {...props}
      animationBegin={animationBegin ?? defaults.animationBegin}
      animationDuration={
        shouldAnimate ? (animationDuration ?? defaults.animationDuration) : 0
      }
      animationEasing={animationEasing ?? defaults.animationEasing}
      isAnimationActive={shouldAnimate}
    />
  );
}

function ChartTooltipRow({
  color,
  formatter,
  hideIndicator,
  index,
  indicator,
  item,
  itemConfig,
  nestLabel,
  tooltipLabel,
}: {
  color?: string;
  formatter: React.ComponentProps<typeof ChartTooltipContent>["formatter"];
  hideIndicator: boolean;
  index: number;
  indicator: "line" | "dot" | "dashed";
  item: NonNullable<
    React.ComponentProps<typeof ChartTooltipContent>["payload"]
  >[number];
  itemConfig: ChartConfig[string] | undefined;
  nestLabel: boolean;
  tooltipLabel: React.ReactNode;
}) {
  const indicatorColor = color ?? item.payload?.fill ?? item.color;

  return (
    <div
      className={cn(
        "flex w-full flex-wrap items-stretch gap-2 [&>svg]:h-2.5 [&>svg]:w-2.5 [&>svg]:text-muted-foreground",
        indicator === "dot" && "items-center"
      )}
    >
      {formatter && item?.value !== undefined ? (
        formatter(
          item.value,
          item.name ?? String(item.dataKey ?? index),
          item,
          index,
          item.payload
        )
      ) : (
        <>
          {itemConfig?.icon ? (
            <itemConfig.icon />
          ) : (
            !hideIndicator && (
              <div
                className={cn(
                  "shrink-0 rounded-[2px] border-(--color-border) bg-(--color-bg)",
                  {
                    "h-2.5 w-2.5": indicator === "dot",
                    "w-1": indicator === "line",
                    "w-0 border-[1.5px] border-dashed bg-transparent":
                      indicator === "dashed",
                    "my-0.5": nestLabel && indicator === "dashed",
                  }
                )}
                style={
                  {
                    "--color-bg": indicatorColor,
                    "--color-border": indicatorColor,
                  } as React.CSSProperties
                }
              />
            )
          )}
          <div
            className={cn(
              "flex flex-1 justify-between leading-none",
              nestLabel ? "items-end" : "items-center"
            )}
          >
            <div className="grid gap-1.5">
              {nestLabel ? tooltipLabel : null}
              <span className="text-muted-foreground">
                {itemConfig?.label ?? item.name}
              </span>
            </div>
            {item.value != null && (
              <span className="font-medium font-mono text-foreground tabular-nums">
                {typeof item.value === "number"
                  ? item.value.toLocaleString()
                  : String(item.value)}
              </span>
            )}
          </div>
        </>
      )}
    </div>
  );
}

function ChartTooltipContent({
  active,
  payload,
  className,
  indicator = "dot",
  hideLabel = false,
  hideIndicator = false,
  label,
  labelFormatter,
  labelClassName,
  formatter,
  color,
  nameKey,
  labelKey,
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip> &
  React.ComponentProps<"div"> & {
    hideLabel?: boolean;
    hideIndicator?: boolean;
    indicator?: "line" | "dot" | "dashed";
    nameKey?: string;
    labelKey?: string;
  } & Omit<
    RechartsPrimitive.DefaultTooltipContentProps<
      TooltipValueType,
      TooltipNameType
    >,
    "accessibilityLayer"
  >) {
  const { config } = useChart();

  const tooltipLabel = React.useMemo(() => {
    if (hideLabel || !payload?.length) {
      return null;
    }

    const [item] = payload;
    const key = `${labelKey ?? item?.dataKey ?? item?.name ?? "value"}`;
    const itemConfig = getPayloadConfigFromPayload(config, item, key);
    const value =
      !labelKey && typeof label === "string"
        ? (config[label]?.label ?? label)
        : itemConfig?.label;

    if (labelFormatter) {
      return (
        <div className={cn("font-medium", labelClassName)}>
          {labelFormatter(value, payload)}
        </div>
      );
    }

    if (!value) {
      return null;
    }

    return <div className={cn("font-medium", labelClassName)}>{value}</div>;
  }, [
    label,
    labelFormatter,
    payload,
    hideLabel,
    labelClassName,
    config,
    labelKey,
  ]);

  if (!(active && payload?.length)) {
    return null;
  }

  const nestLabel = payload.length === 1 && indicator !== "dot";

  return (
    <MotionDiv
      animate={{ opacity: 1 }}
      className={cn(
        "grid min-w-32 items-start gap-1.5 rounded-lg border border-border/50 bg-background px-2.5 py-1.5 text-xs shadow-xl",
        className
      )}
      initial={{ opacity: 0 }}
      role="tooltip"
      transition={chartTooltipFade}
    >
      {nestLabel ? null : tooltipLabel}
      <div className="grid gap-1.5">
        {payload
          .filter((item) => item.type !== "none")
          .map((item, index) => {
            const key = getChartSeriesKey(item, nameKey);
            const itemConfig = getPayloadConfigFromPayload(config, item, key);

            return (
              <ChartTooltipRow
                color={color}
                formatter={formatter}
                hideIndicator={hideIndicator}
                index={index}
                indicator={indicator}
                item={item}
                itemConfig={itemConfig}
                key={`${key}-${index}`}
                nestLabel={nestLabel}
                tooltipLabel={tooltipLabel}
              />
            );
          })}
      </div>
    </MotionDiv>
  );
}

function ChartTooltip({
  content,
  ...props
}: React.ComponentProps<typeof RechartsPrimitive.Tooltip>) {
  return (
    <RechartsPrimitive.Tooltip
      content={
        (content ?? ChartTooltipContentRenderer) as React.ComponentProps<
          typeof RechartsPrimitive.Tooltip
        >["content"]
      }
      {...props}
    />
  );
}

function ChartTooltipContentRenderer(
  props: RechartsPrimitive.TooltipContentProps<
    TooltipValueType,
    TooltipNameType
  >
) {
  return (
    <ChartTooltipContent
      {...(props as React.ComponentProps<typeof ChartTooltipContent>)}
    />
  );
}

function ChartLegendContent({
  className,
  hideIcon = false,
  payload,
  verticalAlign = "bottom",
  nameKey,
}: React.ComponentProps<"div"> & {
  hideIcon?: boolean;
  nameKey?: string;
} & RechartsPrimitive.DefaultLegendContentProps) {
  const { config } = useChart();

  if (!payload?.length) {
    return null;
  }

  return (
    <MotionDiv
      animate={{ opacity: 1, y: 0 }}
      className={cn(
        "flex items-center justify-center gap-4",
        verticalAlign === "top" ? "pb-3" : "pt-3",
        className
      )}
      initial={{ opacity: 0, y: 4 }}
      transition={chartTooltipFade}
    >
      {payload
        .filter((item) => item.type !== "none")
        .map((item) => {
          const seriesKey = getChartSeriesKey(item, nameKey);
          const configKey = getChartConfigKey(config, item, seriesKey);
          const itemConfig = getPayloadConfigFromPayload(
            config,
            item,
            seriesKey
          );
          const label = getChartLegendLabel(item, itemConfig);
          const swatchColor = getChartLegendSwatchColor(
            configKey,
            itemConfig,
            item.color
          );

          return (
            <div
              className="flex items-center gap-1.5 [&>svg]:h-3 [&>svg]:w-3 [&>svg]:text-muted-foreground"
              key={seriesKey}
            >
              {itemConfig?.icon && !hideIcon ? (
                <itemConfig.icon />
              ) : (
                <div
                  className="h-2 w-2 shrink-0 rounded-[2px]"
                  style={{
                    backgroundColor: swatchColor,
                  }}
                />
              )}
              {label}
            </div>
          );
        })}
    </MotionDiv>
  );
}

function ChartLegend({
  className,
  content,
  hideIcon,
  nameKey,
  ...props
}: React.ComponentProps<typeof RechartsPrimitive.Legend> & {
  className?: string;
  hideIcon?: boolean;
  nameKey?: string;
}) {
  const legendContent =
    content ??
    ((legendProps: RechartsPrimitive.DefaultLegendContentProps) => (
      <ChartLegendContent
        {...(legendProps as React.ComponentProps<typeof ChartLegendContent>)}
        className={className}
        hideIcon={hideIcon}
        nameKey={nameKey}
      />
    ));

  return (
    <RechartsPrimitive.Legend
      content={
        legendContent as React.ComponentProps<
          typeof RechartsPrimitive.Legend
        >["content"]
      }
      {...props}
    />
  );
}

function ChartEmptyState({
  className,
  description = "Try adjusting filters or adding data to render this chart.",
  label = "No data available",
}: {
  className?: string;
  description?: React.ReactNode;
  label?: React.ReactNode;
}) {
  return (
    <div
      className={cn(
        "flex h-full min-h-[12rem] flex-col items-center justify-center gap-1 rounded-lg border border-border/70 border-dashed bg-muted/20 px-6 text-center",
        className
      )}
    >
      <p className="font-medium text-foreground text-sm">{label}</p>
      {description ? (
        <p className="max-w-xs text-muted-foreground text-xs">{description}</p>
      ) : null}
    </div>
  );
}

export {
  ChartArea,
  ChartBar,
  ChartContainer,
  ChartEmptyState,
  ChartLegend,
  ChartLegendContent,
  ChartLine,
  ChartTooltip,
  ChartTooltipContent,
  useChart,
};

export {
  getChartAnimationTimeoutMs,
  getChartLegendLabel,
  getChartLegendSwatchColor,
  getChartSeriesKey,
  getPayloadConfigFromPayload,
};

demo.tsx
"use client";

import { ChevronLeft, ChevronRight } from "lucide-react";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import { useState } from "react";
import {
  BarChart,
  CartesianGrid,
  Line,
  LineChart,
  XAxis,
  YAxis,
} from "recharts";
import {
  ChartBar,
  ChartContainer,
  ChartTooltip,
  ChartTooltipContent,
  type ChartConfig,
} from "@/components/ui/chart";
import { cn } from "@/lib/utils";

const barChartData = [
  { month: "Jan", sessions: 1240, conversions: 420 },
  { month: "Feb", sessions: 1580, conversions: 510 },
  { month: "Mar", sessions: 1420, conversions: 480 },
  { month: "Apr", sessions: 1890, conversions: 620 },
  { month: "May", sessions: 1760, conversions: 590 },
  { month: "Jun", sessions: 2100, conversions: 710 },
];

const lineChartData = [
  { month: "Jan", sessions: 520, conversions: 480 },
  { month: "Feb", sessions: 470, conversions: 540 },
  { month: "Mar", sessions: 560, conversions: 505 },
  { month: "Apr", sessions: 495, conversions: 565 },
  { month: "May", sessions: 545, conversions: 490 },
  { month: "Jun", sessions: 485, conversions: 530 },
];

const lineChartDomain = [400, 600];

const chartConfig = {
  sessions: {
    label: "Sessions",
    color: "#2563eb",
  },
  conversions: {
    label: "Conversions",
    color: "#7dd3fc",
  },
} satisfies ChartConfig;

const chartSlides = [
  { id: "bar", label: "Bar chart" },
  { id: "line", label: "Line chart" },
] as const;

const chartPreviewClassName = "mx-auto w-full max-w-lg";

export function ChartsPreview() {
  const [slideIndex, setSlideIndex] = useState(0);
  const reduceMotion = useReducedMotion() ?? false;
  const activeSlide = chartSlides[slideIndex];

  const goToSlide = (nextIndex: number) => {
    setSlideIndex((nextIndex + chartSlides.length) % chartSlides.length);
  };

  return (
    <div className="mx-auto flex w-full max-w-2xl flex-col items-center gap-5">
      <div className="relative w-full overflow-hidden">
        <AnimatePresence initial={false} mode="wait">
          <motion.div
            key={activeSlide.id}
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
            transition={{ duration: 0.22, ease: [0.22, 1, 0.36, 1] }}
            className="w-full"
          >
            {activeSlide.id === "bar" ? (
              <ChartContainer className="w-full" config={chartConfig}>
                <BarChart accessibilityLayer data={barChartData}>
                  <CartesianGrid vertical={false} />
                  <XAxis
                    axisLine={false}
                    dataKey="month"
                    tickFormatter={(value) => value.slice(0, 3)}
                    tickLine={false}
                    tickMargin={10}
                  />
                  <ChartTooltip
                    content={<ChartTooltipContent indicator="dashed" />}
                    cursor={false}
                  />
                  <ChartBar dataKey="sessions" fill="var(--color-sessions)" radius={4} seriesIndex={0} />
                  <ChartBar dataKey="conversions" fill="var(--color-conversions)" radius={4} seriesIndex={1} />
                </BarChart>
              </ChartContainer>
            ) : (
              <ChartContainer className={chartPreviewClassName} config={chartConfig}>
                <LineChart accessibilityLayer data={lineChartData}>
                  <CartesianGrid vertical={false} />
                  <XAxis
                    axisLine={false}
                    dataKey="month"
                    tickFormatter={(value) => value.slice(0, 3)}
                    tickLine={false}
                    tickMargin={10}
                  />
                  <YAxis domain={lineChartDomain} hide />
                  <ChartTooltip
                    content={<ChartTooltipContent indicator="dot" />}
                    cursor={false}
                  />
                  <Line dataKey="sessions" type="monotone" stroke="var(--color-sessions)" strokeWidth={2} dot={false} />
                  <Line dataKey="conversions" type="monotone" stroke="var(--color-conversions)" strokeWidth={2} dot={false} />
                </LineChart>
              </ChartContainer>
            )}
          </motion.div>
        </AnimatePresence>
      </div>

      <div className="flex w-full items-center justify-between gap-3">
        <button
          type="button"
          aria-label="Previous chart"
          className="inline-flex size-8 items-center justify-center rounded-md border border-border"
          onClick={() => goToSlide(slideIndex - 1)}
        >
          <ChevronLeft className="size-4" />
        </button>
        <div className="flex items-center gap-2" role="tablist" aria-label="Chart examples">
          {chartSlides.map((slide, index) => (
            <button
              key={slide.id}
              type="button"
              role="tab"
              aria-label={slide.label}
              aria-selected={index === slideIndex}
              className={cn(
                "rounded-full transition-[width,background-color] duration-300",
                index === slideIndex ? "h-2 w-6 bg-foreground" : "size-2 bg-muted-foreground/35"
              )}
              onClick={() => goToSlide(index)}
            />
          ))}
        </div>
        <button
          type="button"
          aria-label="Next chart"
          className="inline-flex size-8 items-center justify-center rounded-md border border-border"
          onClick={() => goToSlide(slideIndex + 1)}
        >
          <ChevronRight className="size-4" />
        </button>
      </div>
      <p className="text-center text-xs text-muted-foreground">{activeSlide.label}</p>
    </div>
  );
}

export default ChartsPreview
```

Install NPM dependencies:
```bash
npm install motion recharts
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
