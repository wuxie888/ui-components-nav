<!-- Relative Time · @haydenbleasel · https://21st.dev/@haydenbleasel/components/relative-time
     license: unspecified · category: date-picker
     Here is Relative Time component -->

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
components/ui/index.tsx
"use client";

import { useControllableState } from "@radix-ui/react-use-controllable-state";
import {
  createContext,
  type HTMLAttributes,
  useContext,
  useEffect,
} from "react";
import { cn } from "@/lib/utils";

const formatDate = (
  date: Date,
  timeZone: string,
  options?: Intl.DateTimeFormatOptions
) =>
  new Intl.DateTimeFormat(
    "en-US",
    options ?? {
      dateStyle: "long",
      timeZone,
    }
  ).format(date);

const formatTime = (
  date: Date,
  timeZone: string,
  options?: Intl.DateTimeFormatOptions
) =>
  new Intl.DateTimeFormat(
    "en-US",
    options ?? {
      hour: "2-digit",
      minute: "2-digit",
      second: "2-digit",
      timeZone,
    }
  ).format(date);

type RelativeTimeContextType = {
  time: Date;
  dateFormatOptions?: Intl.DateTimeFormatOptions;
  timeFormatOptions?: Intl.DateTimeFormatOptions;
};

const RelativeTimeContext = createContext<RelativeTimeContextType>({
  time: new Date(),
  dateFormatOptions: {
    dateStyle: "long",
  },
  timeFormatOptions: {
    hour: "2-digit",
    minute: "2-digit",
  },
});

export type RelativeTimeProps = HTMLAttributes<HTMLDivElement> & {
  time?: Date;
  defaultTime?: Date;
  onTimeChange?: (time: Date) => void;
  dateFormatOptions?: Intl.DateTimeFormatOptions;
  timeFormatOptions?: Intl.DateTimeFormatOptions;
};

export const RelativeTime = ({
  time: controlledTime,
  defaultTime = new Date(),
  onTimeChange,
  dateFormatOptions,
  timeFormatOptions,
  className,
  ...props
}: RelativeTimeProps) => {
  const [time, setTime] = useControllableState<Date>({
    defaultProp: defaultTime,
    prop: controlledTime,
    onChange: onTimeChange,
  });

  useEffect(() => {
    if (controlledTime) {
      return;
    }

    const interval = setInterval(() => {
      setTime(new Date());
    }, 1000);

    return () => clearInterval(interval);
  }, [setTime, controlledTime]);

  return (
    <RelativeTimeContext.Provider
      value={{
        time: time ?? defaultTime,
        dateFormatOptions,
        timeFormatOptions,
      }}
    >
      <div className={cn("grid gap-2", className)} {...props} />
    </RelativeTimeContext.Provider>
  );
};

export type RelativeTimeZoneProps = HTMLAttributes<HTMLDivElement> & {
  zone: string;
  dateFormatOptions?: Intl.DateTimeFormatOptions;
  timeFormatOptions?: Intl.DateTimeFormatOptions;
};

export type RelativeTimeZoneContextType = {
  zone: string;
};

const RelativeTimeZoneContext = createContext<RelativeTimeZoneContextType>({
  zone: "UTC",
});

export const RelativeTimeZone = ({
  zone,
  className,
  ...props
}: RelativeTimeZoneProps) => (
  <RelativeTimeZoneContext.Provider value={{ zone }}>
    <div
      className={cn(
        "flex items-center justify-between gap-1.5 text-xs",
        className
      )}
      {...props}
    />
  </RelativeTimeZoneContext.Provider>
);

export type RelativeTimeZoneDisplayProps = HTMLAttributes<HTMLDivElement>;

export const RelativeTimeZoneDisplay = ({
  className,
  ...props
}: RelativeTimeZoneDisplayProps) => {
  const { time, timeFormatOptions } = useContext(RelativeTimeContext);
  const { zone } = useContext(RelativeTimeZoneContext);
  const display = formatTime(time, zone, timeFormatOptions);

  return (
    <div
      className={cn("pl-8 text-muted-foreground tabular-nums", className)}
      {...props}
    >
      {display}
    </div>
  );
};

export type RelativeTimeZoneDateProps = HTMLAttributes<HTMLDivElement>;

export const RelativeTimeZoneDate = ({
  className,
  ...props
}: RelativeTimeZoneDateProps) => {
  const { time, dateFormatOptions } = useContext(RelativeTimeContext);
  const { zone } = useContext(RelativeTimeZoneContext);
  const display = formatDate(time, zone, dateFormatOptions);

  return <div {...props}>{display}</div>;
};

export type RelativeTimeZoneLabelProps = HTMLAttributes<HTMLDivElement>;

export const RelativeTimeZoneLabel = ({
  className,
  ...props
}: RelativeTimeZoneLabelProps) => (
  <div
    className={cn(
      "flex h-4 items-center justify-center rounded-xs bg-secondary px-1.5 font-mono",
      className
    )}
    {...props}
  />
);

demo.tsx
'use client';

import {
  RelativeTime,
  RelativeTimeZone,
  RelativeTimeZoneDate,
  RelativeTimeZoneDisplay,
  RelativeTimeZoneLabel,
} from '@/components/ui/relative-time';

const timezones = [
  { label: 'EST', zone: 'America/New_York' },
  { label: 'GMT', zone: 'Europe/London' },
  { label: 'JST', zone: 'Asia/Tokyo' },
];

const Example = () => (
  <div className="rounded-md border bg-background p-4">
    <RelativeTime timeFormatOptions={{ hour: '2-digit', minute: '2-digit' }}>
      {timezones.map(({ zone, label }) => (
        <RelativeTimeZone key={zone} zone={zone}>
          <RelativeTimeZoneLabel>{label}</RelativeTimeZoneLabel>
          <RelativeTimeZoneDate />
          <RelativeTimeZoneDisplay />
        </RelativeTimeZone>
      ))}
    </RelativeTime>
  </div>
);

export default Example;
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-use-controllable-state
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
