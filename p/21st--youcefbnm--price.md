<!-- Price · @youcefbnm · https://21st.dev/@youcefbnm/components/price
     license: no-license · category: pricing-section
     Composable price component that formats amounts and currencies with support for sale pricing, compare-at values, savings, and ranges. -->

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
'use client';
import { cn } from '@/lib/utils';
import React from 'react';

export const currencyMinorUnits: Record<string, number> = {
  USD: 2,
  EUR: 2,
  GBP: 2,
  JPY: 0,
};

interface PriceContextValue {
  amount: number | null;
  compareAt?: number | null;
  savingsPercent?: number | null;
  currency?: string;
  locale?: string;
}
const PriceContext = React.createContext<PriceContextValue | undefined>(
  undefined,
);
function usePriceContext() {
  const context = React.useContext(PriceContext);
  if (context === undefined) {
    throw new Error('usePriceContext must be used within a PriceProvider');
  }
  return context;
}
/**
 * Returns the divider to convert minor units to major units.
 * e.g. USD -> 100, JPY -> 1
 */
export function minorUnitDivider(currency?: string): number {
  const code = (currency ?? 'USD').toUpperCase();
  const digits = currencyMinorUnits[code] ?? 2;
  return Math.pow(10, digits);
}

/**
 * Memoized Intl.NumberFormat factory keyed by locale|currency|digits.
 */
const nfCache = new Map<string, Intl.NumberFormat>();
function getNumberFormatter(
  locale: string,
  currency: string,
  fractionDigits: number,
) {
  const key = `${locale}|${currency}|${fractionDigits}`;
  const cached = nfCache.get(key);
  if (cached) return cached;
  const nf = new Intl.NumberFormat(locale, {
    style: 'currency',
    currency,
    minimumFractionDigits: fractionDigits,
    maximumFractionDigits: fractionDigits,
    currencyDisplay: 'symbol',
  });
  nfCache.set(key, nf);
  return nf;
}

/**
 * Format an integer minor-unit amount (e.g. cents) to a localized currency string.
 * - amountMinor: integer (may be negative)
 * - currency: ISO code, defaults to USD
 * - locale: BCP47 locale, defaults to en-US
 */
export function formatCurrencyFromMinor(
  amountMinor: number,
  currency = 'USD',
  locale = 'en-US',
): string {
  if (!Number.isInteger(amountMinor)) {
    console.warn('amountMinor should be an integer');
    amountMinor = Math.round(amountMinor);
  }
  const digits = currencyMinorUnits[currency.toUpperCase()] ?? 2;
  const divider = minorUnitDivider(currency);
  // Use exact division (no floating rounding) then delegate to Intl for display.
  const amountMajor = amountMinor / divider;
  const nf = getNumberFormatter(locale, currency, digits);
  return nf.format(amountMajor);
}

// components/Price.tsx

interface PriceProps extends React.ComponentProps<'div'> {
  amount: number | null; // minor units
  currency?: 'USD' | 'EUR' | 'GBP' | 'JPY';
  locale?: string;
  compareAt?: number | null; // minor units
  range?: { min: number | null; max: number | null } | null;
}
const format = (amt: number | null, currency?: string, locale?: string) => {
  if (amt === null) return '';
  return formatCurrencyFromMinor(amt, currency ?? 'USD', locale ?? 'en-US');
};

export function Price({
  amount,
  currency = 'USD',
  locale = 'en-US',
  compareAt = null,
  range = null,
  className,
  ...props
}: PriceProps) {
  // Range handling
  if (range) {
    if (range.min === null || range.max === null) return null;
    if (range.min === range.max) {
      return (
        <span {...props}>
          <span className="sr-only">Price: </span>
          {format(range.min)}
        </span>
      );
    }
    return (
      <span {...props}>
        <span className="sr-only">Price range: </span>
        {format(range.min)}&nbsp;–&nbsp;{format(range.max)}
      </span>
    );
  }

  // Unavailable / Free
  if (amount == null) {
    return (
      <span aria-hidden={false} {...props}>
        <span className="sr-only">Price not available</span>
      </span>
    );
  }
  if (amount === 0) {
    return (
      <span {...props}>
        <span className="sr-only">Free</span>
        Free
      </span>
    );
  }

  const isOnSale = compareAt != null && compareAt > amount;
  let savingsPercent: number | null = null;
  if (isOnSale) {
    // integer math to avoid float errors:
    savingsPercent = Math.round(((compareAt - amount) * 100) / compareAt);
  }
  return (
    <PriceContext.Provider
      value={{ amount, compareAt, savingsPercent, currency, locale }}
    >
      <span
        className={cn('inline-flex items-baseline', className)}
        {...props}
      />
    </PriceContext.Provider>
  );
}

export function PriceCurrent({ ...props }: React.ComponentProps<'span'>) {
  const { amount, currency, locale } = usePriceContext();
  return (
    <span {...props}>
      <span className="sr-only">Price: </span>
      {format(amount, currency, locale)}
    </span>
  );
}

export function PriceCompareAt({ ...props }: React.ComponentProps<'span'>) {
  const { compareAt, currency, locale } = usePriceContext();
  return (
    <span {...props}>
      <span className="sr-only">Old Price: </span>
      {format(compareAt!, currency, locale)}
    </span>
  );
}

export function PriceSavings({
  children,
  ...props
}: React.ComponentProps<'span'>) {
  const { savingsPercent } = usePriceContext();
  return (
    <span aria-hidden="true" {...props}>
      <span className="sr-only">Save: </span>
      {children} {savingsPercent}%
    </span>
  );
}

demo.tsx
import {
  Price,
  PriceCompareAt,
  PriceCurrent,
  PriceSavings,
} from "@/components/ui/price";
import { badgeVariants } from "@/components/ui/badge";

export default function PriceDemo() {
  return (
    <div className="p-8 space-y-6 bg-background text-foreground">
      <div className="space-y-2">
        <h3 className="font-semibold">Price with sale</h3>
        <Price
          className="tabular-nums tracking-tighter space-x-2"
          amount={1999}
          compareAt={2499}
          currency="USD"
        >
          <PriceCurrent />
          <PriceCompareAt className="text-sm line-through text-muted-foreground" />
          <PriceSavings
            className={`${badgeVariants({ variant: "outline" })} text-emerald-500 bg-emerald-100 border-none`}
          >
            Save
          </PriceSavings>
        </Price>
      </div>

      <div className="space-y-2">
        <h3 className=" ">Price with diffrent currency</h3>
        <div className="flex gap-2">
          <Price
            className="tabular-nums tracking-tighter space-x-2"
            amount={2000}
            currency="GBP"
          >
            <PriceCurrent />
          </Price>
          <Price
            className="tabular-nums tracking-tighter space-x-2"
            amount={1999}
            currency="EUR"
          >
            <PriceCurrent />
          </Price>
          <Price
            className="tabular-nums tracking-tighter space-x-2"
            amount={20000}
            currency="JPY"
          >
            <PriceCurrent />
          </Price>
        </div>
      </div>
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge
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
