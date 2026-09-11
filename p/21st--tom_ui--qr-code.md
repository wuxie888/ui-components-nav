<!-- QR Code · @tom_ui · https://21st.dev/@tom_ui/components/qr-code
     license: MIT · category: grid
     QR code generator with rounded finder patterns and dot-style data modules. -->

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
components/ui/qr-code.tsx
"use client";

import * as React from "react";
import QRCodeLib from "qrcode";
import { cn } from "@/lib/utils";

interface QRCodeProps extends React.SVGProps<SVGSVGElement> {
  value: string;
  size?: number;
  fgColor?: string;
  bgColor?: string;
  errorCorrectionLevel?: "L" | "M" | "Q" | "H";
  className?: string;
}

function isInFinderPattern(row: number, col: number, size: number): boolean {
  return (
    (row < 7 && col < 7) ||
    (row < 7 && col >= size - 7) ||
    (row >= size - 7 && col < 7)
  );
}

export function QRCode({
  value,
  size = 268,
  fgColor = "var(--foreground)",
  bgColor = "var(--background)",
  errorCorrectionLevel = "M",
  className,
  ...props
}: QRCodeProps) {
  const qrData = React.useMemo(() => {
    try {
      return QRCodeLib.create(value, { errorCorrectionLevel });
    } catch {
      return null;
    }
  }, [value, errorCorrectionLevel]);

  if (!qrData) {
    return null;
  }

  const moduleCount = qrData.modules.size;
  const moduleSize = size / moduleCount;
  const totalSize = size;
  const circleRadius = moduleSize * (1 / 3);

  const finderPositions: [number, number][] = [
    [0, 0],
    [0, moduleCount - 7],
    [moduleCount - 7, 0],
  ];

  const finderSize = 7 * moduleSize;
  const innerPadding = moduleSize;
  const innerWhiteSize = 5 * moduleSize;
  const innerBlackSize = 3 * moduleSize;

  const circles: { cx: number; cy: number }[] = [];

  for (let row = 0; row < moduleCount; row++) {
    for (let col = 0; col < moduleCount; col++) {
      if (qrData.modules.get(row, col) && !isInFinderPattern(row, col, moduleCount)) {
        circles.push({
          cx: (col + 0.5) * moduleSize,
          cy: (row + 0.5) * moduleSize,
        });
      }
    }
  }

  return (
    <svg
      width={totalSize}
      height={totalSize}
      viewBox={`0 0 ${totalSize} ${totalSize}`}
      xmlns="http://www.w3.org/2000/svg"
      aria-label={`QR code for ${value}`}
      className={cn("block", className)}
      {...props}
    >
      <rect width={totalSize} height={totalSize} fill={bgColor} rx="12" ry="12" />
      {finderPositions.map(([r, c]) => {
        const x = c * moduleSize;
        const y = r * moduleSize;
        return (
          <g key={`${r}-${c}`}>
            <rect
              x={x}
              y={y}
              width={finderSize}
              height={finderSize}
              fill={fgColor}
              rx="12"
              ry="12"
            />
            <rect
              x={x + innerPadding}
              y={y + innerPadding}
              width={innerWhiteSize}
              height={innerWhiteSize}
              fill={bgColor}
              rx="8"
              ry="8"
            />
            <rect
              x={x + innerPadding * 2}
              y={y + innerPadding * 2}
              width={innerBlackSize}
              height={innerBlackSize}
              fill={fgColor}
              rx="3"
              ry="3"
            />
          </g>
        );
      })}
      {circles.map(({ cx, cy }, i) => (
        <circle
          key={i}
          cx={cx}
          cy={cy}
          r={circleRadius}
          fill={fgColor}
        />
      ))}
    </svg>
  );
}

demo.tsx
"use client";

import { QRCode } from "@/components/ui/qr-code";

const levels = ["L", "M", "Q", "H"] as const;

function Demo() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center overflow-hidden bg-background p-8">
      <div className="rounded-3xl border bg-card p-6 shadow-2xl">
        <div className="mb-5">
          <p className="text-sm text-muted-foreground">Error correction</p>
          <h3 className="text-2xl font-semibold tracking-tight">Four Levels</h3>
        </div>
        <div className="grid grid-cols-4 gap-4">
          {levels.map((level) => (
            <div key={level} className="flex flex-col items-center gap-2">
              <QRCode
                value={`https://21st.dev/qr-${level}`}
                size={126}
                errorCorrectionLevel={level}
                fgColor="#f8fafc"
                bgColor="#020617"
                className="rounded-xl border"
              />
              <span className="text-xs text-muted-foreground">Level {level}</span>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
}


export default { Demo };
```

Install NPM dependencies:
```bash
npm install qrcode
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
