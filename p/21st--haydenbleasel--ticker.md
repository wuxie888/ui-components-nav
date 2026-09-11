<!-- Ticker · @haydenbleasel · https://21st.dev/@haydenbleasel/components/ticker
     license: MIT · category: stat
     A composable finance ticker for displaying a stock symbol, logo, price, and color-coded price change. -->

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

import type { HTMLAttributes, ReactNode } from "react";
import { createContext, memo, useContext, useMemo } from "react";
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";
import { cn } from "@/lib/utils";

type TickerContextValue = {
  formatter: Intl.NumberFormat;
};

const DEFAULT_CURRENCY = "USD";
const DEFAULT_LOCALE = "en-US";

const defaultFormatter = new Intl.NumberFormat(DEFAULT_LOCALE, {
  style: "currency",
  currency: DEFAULT_CURRENCY,
  minimumFractionDigits: 2,
  maximumFractionDigits: 2,
});

const TickerContext = createContext<TickerContextValue>({
  formatter: defaultFormatter,
});

export const useTickerContext = () => useContext(TickerContext);

export type TickerProps = HTMLAttributes<HTMLButtonElement> & {
  currency?: string;
  locale?: string;
};

export const Ticker = memo(
  ({
    children,
    className,
    currency = DEFAULT_CURRENCY,
    locale = DEFAULT_LOCALE,
    ...props
  }: TickerProps & { children: ReactNode }) => {
    const formatter = useMemo(() => {
      try {
        return new Intl.NumberFormat(locale, {
          style: "currency",
          currency: currency.toUpperCase(),
          minimumFractionDigits: 2,
          maximumFractionDigits: 2,
        });
      } catch {
        return defaultFormatter;
      }
    }, [currency, locale]);

    return (
      <TickerContext.Provider value={{ formatter }}>
        <button
          className={cn(
            "inline-flex items-center gap-1.5 whitespace-nowrap align-middle",
            className
          )}
          type="button"
          {...props}
        >
          {children}
        </button>
      </TickerContext.Provider>
    );
  }
);
Ticker.displayName = "Ticker";

export type TickerIconProps = HTMLAttributes<HTMLImageElement> & {
  src?: string;
  symbol?: string;
  asChild?: boolean;
};

export const TickerIcon = memo(
  ({
    src,
    symbol,
    className,
    asChild,
    children,
    ...props
  }: TickerIconProps) => {
    if (asChild) {
      return (
        <div
          className={cn(
            "overflow-hidden rounded-full border border-border bg-muted",
            className
          )}
        >
          {children}
        </div>
      );
    }

    return (
      <Avatar className={cn("size-7 border border-border bg-muted", className)}>
        <AvatarImage src={src} {...props} />
        <AvatarFallback className="font-semibold text-muted-foreground text-sm">
          {symbol?.slice(0, 2).toUpperCase()}
        </AvatarFallback>
      </Avatar>
    );
  }
);
TickerIcon.displayName = "TickerIcon";

export type TickerSymbolProps = HTMLAttributes<HTMLSpanElement> & {
  symbol: string;
};

export const TickerSymbol = memo(
  ({ symbol, className, ...props }: TickerSymbolProps) => (
    <span className={cn("font-medium", className)} {...props}>
      {symbol.toUpperCase()}
    </span>
  )
);
TickerSymbol.displayName = "TickerSymbol";

export type TickerPriceProps = HTMLAttributes<HTMLSpanElement> & {
  price: number;
};

export const TickerPrice = memo(
  ({ price, className, ...props }: TickerPriceProps) => {
    const context = useTickerContext();

    const formattedPrice = useMemo(
      () => context.formatter.format(price),
      [price, context]
    );

    return (
      <span className={cn("text-muted-foreground", className)} {...props}>
        {formattedPrice}
      </span>
    );
  }
);
TickerPrice.displayName = "TickerPrice";

export type TickerPriceChangeProps = HTMLAttributes<HTMLSpanElement> & {
  change: number;
  isPercent?: boolean;
};

export const TickerPriceChange = memo(
  ({ change, isPercent, className, ...props }: TickerPriceChangeProps) => {
    const isPositiveChange = useMemo(() => change >= 0, [change]);
    const context = useTickerContext();

    const changeFormatted = useMemo(() => {
      if (isPercent) {
        return `${change.toFixed(2)}%`;
      }
      return context.formatter.format(change);
    }, [change, isPercent, context]);

    return (
      <span
        className={cn(
          "flex items-center gap-0.5",
          isPositiveChange
            ? "text-green-600 dark:text-green-500"
            : "text-red-600 dark:text-red-500",
          className
        )}
        {...props}
      >
        <svg
          aria-labelledby="ticker-change-icon-title"
          className={isPositiveChange ? "" : "rotate-180"}
          fill="currentColor"
          height="12"
          role="img"
          viewBox="0 0 24 24"
          width="12"
          xmlns="http://www.w3.org/2000/svg"
        >
          <title id="ticker-change-icon-title">
            {isPositiveChange ? "Up icon" : "Down icon"}
          </title>
          <path d="M24 22h-24l12-20z" />
        </svg>
        {changeFormatted}
      </span>
    );
  }
);
TickerPriceChange.displayName = "TickerPriceChange";

demo.tsx
"use client";

import {
  Ticker,
  TickerIcon,
  TickerPrice,
  TickerPriceChange,
  TickerSymbol,
} from "@/components/ui/ticker";
import Image from "next/image";

const ticker = "GOOG";

const Example = () => (
  <div className="flex items-center justify-center p-10">
    <Ticker>
      <TickerIcon asChild>
        <Image
          alt={ticker}
          height={26}
          src="data:image/jpeg;base64,/9j/2wCEAAEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAf/AABEIADQANAMBIgACEQEDEQH/xAGiAAABBQEBAQEBAQAAAAAAAAAAAQIDBAUGBwgJCgsQAAIBAwMCBAMFBQQEAAABfQECAwAEEQUSITFBBhNRYQcicRQygZGhCCNCscEVUtHwJDNicoIJChYXGBkaJSYnKCkqNDU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6g4SFhoeIiYqSk5SVlpeYmZqio6Slpqeoqaqys7S1tre4ubrCw8TFxsfIycrS09TV1tfY2drh4uPk5ebn6Onq8fLz9PX29/j5+gEAAwEBAQEBAQEBAQAAAAAAAAECAwQFBgcICQoLEQACAQIEBAMEBwUEBAABAncAAQIDEQQFITEGEkFRB2FxEyIygQgUQpGhscEJIzNS8BVictEKFiQ04SXxFxgZGiYnKCkqNTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqCg4SFhoeIiYqSk5SVlpeYmZqio6Slpqeoqaqys7S1tre4ubrCw8TFxsfIycrS09TV1tfY2dri4+Tl5ufo6ery8/T19vf4+fr/2gAMAwEAAhEDEQA/AP7+K+Jv2sv27PhD+yhYPY60bnxr8R7m1Fzpfw68PXEMd6kckZe3vfE+rSJPa+F9Lnynlyz299q11HItxpuiahbx3EsPr37Rnxdf4P8Aw51DWdMEM3izVvM0nwnazKskY1OWJmk1S4hbPm2ejwZu5I2UxXFz9isJWiW9Eqfy5fFzwrrniTWta8Sa9d3+ta1rN7c6jquq6hJJdXt/e3LtJPc3U0hZ3d3JOSdqqAqhUCrX8f8Aj59Kvh7ws4owXhvlFbC4rjTFYOhmOa1azjUw3DuBxl/qMKtO/LVzXHU08TQw1V8mGwjw+Kr0q1PGYeEv7H+in9Hrh/xUzdcQ+IeJxWH4KwOJ9hQyrBV5YPGcS4yk4utRnjY2rYLKaDapYmthXDF4mrKph8JicJUoVa8fSfil/wAFeP2vfF+rNP4J1nwr8JNESSX7NpfhnwrofiK8lt2bMK6tq/jmw8SPc3US4VrnSbLQYJTkmyQHbXlMH/BT/wDbpV49nx2uwUUhmk8B/C2dZWMjvvaO48ESx5CukYVUVNsY+UuWZvqj9nn/AIJDfEX4s6fZeMfiv4mb4R+EtRWK703QU0j+1vHer2TlHWaawubqysfCtvdQMXtJ9TbUtTDJm58OwwSw3En2Br//AARF+B8+kSQ+Evi98U9F18Q7bfUvENv4S8S6SJtzMJbnRtO0XwpeTKd2zZDrtsQiqN7MGZ+7h/K/GLiXA4fPamOz/C0cZThiqFPF51UyqtXo1IqcJU8vjiKCwsZppwVWjhozi1OK5JqT/v7NfFT9np4e4ynwZV4Q8Nswr5fP6ljcXgvDDDcX4fAVYNwnHMuIq2TZliM2q0W5Rrzw2MzevTlF05tVqbpx+e/2fv8Agsx8QNKvbDRv2ivBul+MNBfybe48Z+BbSPQfF1lhZDLf6h4emuv+Ea8QeY5gQ2ult4PFtF506G/lVLN/34+HHxK8C/Fzwdo/j74ceJdO8V+E9dg87T9W013KllwJ7O8tZ44b3TdSs5MwX+l6jbWuo2Fwr295awTIyD+crwZ/wSA+OMnxxg8D+ONT0q0+EFiq6xqfxW8O3cEx1jRFudkehaDouoL/AGjYeM79R5VxHqljPoegoLjUvt3iCKDTrLXP6N/hz8OfBXwl8FaB8Pfh7oFl4a8I+GrJbLStKslbai7mluLu7uJWe5v9Sv7l5b3U9TvZZ7/Ub+e4vb24muZpJG/cuBpcW+xr0uIXVlh6H7qjPHR/2+VeLXMo1I29vh1Hmcq9V1HObiqNWcIz5f4b+mHg/owUcRw5j/A+phFxFnMI5nm+G4RxCnwXh8mxFKo8PLFYKtzf2RxBUrKlGnk+WxwVPCYWniZ5tl+GxVbByr9vRRRX6Afw8fmb+19qh8Q/EO38Ps7G18MaJaQrCTlEv9XA1K7nVexms5NLicjk/ZUzwBXnn7L3wN0Xxn8UU1/xBp8F/o3giCLXBazxJLa3euST+VocV1Gw2yRQSx3WqBGykk2mwQzxyQSSoeh/aegmsvjN4mnlVkj1C18P3luT92SFNA02wZ1+lxY3EZx3RuhBr1f9i/XLF9X8f6K00P8AaN1p/h/UoIAf3slpplzqlreSgFiSkMurWCk46zJkjiv+eTg7l49/af5zlXHbdbL/APiNniBS9hjn+4xVDgbBcR1OCMFUhW9ythsQ+HeHsLDDVIzoYqhKGHUJwqxhL+3K2e47hfwEpf2Bip4atX4TyqCnh5uFak86ng4ZvUpSg1KnUUcbjqjqRanTqJ1FJSjdfftfDvx+/wCChf7OP7NXxBk+GfxP1Lxba+KItG0zXXi0bwrdavZfYNW8/wCxMLyKeNDK32aXzItuUwuSd1fcVfzKf8FWf2dPj58Tf2r7vxP8Ovgx8T/HPhx/h74NsF13wn4I8Ra/pLXtoNU+1Wg1DTNPubU3Nt5kfnQ+Zvj3pvUbhn/qT+jrwHwP4i+IFTh/xAzWrk+Qx4fzLMI4ulm2Cyabx+GxGAp4el9cx9KtQanTr15Ojyc8+Tmi0oSv/mf4w8V8T8HcIxzfhLAQzHNXm2Cwjw9TAYrMYrC1qWKnWqfV8JUpVU4ypU0qjlyR5rNNyR+j3/D4r9i3/oN/EP8A8IG+/wDkuj/h8V+xb/0G/iH/AOEDff8AyXX82tx+x1+1hawTXVz+zb8cLe2topJ7ieb4Y+MI4YIIUaSWaWR9JCRxxxhnkdiERFLMQATXzdX9/ZZ9C/6PGdKs8m4i4kzZYZ01iHlnF2SY9UHV5/ZKs8LlNX2Tqezqez5+Xn5J8t+WVv5Lx30k/F/LHSWZZNkuXuspuisdw/mmEdVU+VVHSVfMKbqKDnDncb8vPHmtzK/973wf+LHg745fDfwv8VfAF1eXnhDxfa3l3o1xf2b6fetHYanfaPdpc2cjM9vLDqGnXcLxszHMeQcEV6VXwH/wS6sbrTv2EfgJb3iPHLJY+Pb9FcEE2uqfFTxzqdi4ySdklld28kZzgxspAAIA+/K/y38QMjwPDPHnG3DeV1KtbLOHuLuJMjy6rXqRq16uBynOcbgMJUrVYQpwqVZ4fD05VKkKcIzm3KMIppL+6OEs0xWd8K8M51joU6eNzfh/Js0xlOlCUKVPFY/LcNi8RCnCcpyhThVqzjCEpzlGKScpNNv8+/27vCN7b+HtI+KWmwPLFoka6B4lMYZjb6fdXLy6LfuFG1IIdRurqwnkYlmn1PT41XaHI/Ivwt+1Frfwa8faN460V0uH0i5Zb/S7i4aG11zSJx5Wo6TcuqyFI7u3LeTOYZjZXkdpfRxSTWkan+mbWNH0vxBpOpaFrdjbapo+sWN1puqadeRrNa31heQvb3VrcRN8rxTQyPG44OG4IIBH8z/7dX7DXxL+Ceoax448C6Xqnjb4OTy3F6upaZDNqGseB7Z2aQWHiuziEt1/Z1omYrfxPFHJp0sccY1WTTbyeCC4/wAcfpT/AESuIKXjfgPpDeGrxtCWYY/KM14ilk8V/afDXFWRxwtLB8TYOnGMubAY+jg8LWx01RqLC5nQxWLxvtKGZP2M+LXivxtwp4bTjk1Gvj8Hk9GvCtCjSqYiVDBTlOtz4ihTUp1MLQlOaqVOXko0eRVHClByX74/AX9p34N/tHaBDq/w28Xafeaolqk+teDb25t7Txj4ckO1ZotV0NpjdfZ45mMMWrWYutGvWUmxv5wDt+ga/gVbWdQ06+hv9MvrzTr+0lE1rfWN1NaXtrKpO2W2ubd45oJAD8rxurjPDDkV1Gv/ABx+NfibSZdB8RfGD4p69odxE0E2i618QfFuq6TLCw2tBLpt/q89m8TKSGiaEowLArgmv794B8Rc1zjJsufEuX0aWZSoUo4nGYKbhh8TNRip4j6pODeHc3eUqcK1SHNdwVOFoR/z1o/tC8NluGrYbO/DqpmWaYdSjDE5Rn0MDgMZNXUHUoYvL8ZXwF2kpuFbHq7c4QirU1/RV/wU2/4KG+APhj8NPGHwK+E3ibTvFvxe8daVqXhTxDd+Hr+K+034aeHtSil03xBNqeqWMrQp4yvLGS80zR9EtLkajoc8r6/q5sPsmk2Wu/zEeBvC+v8Aj/xZ4Z8DeFbCTVvE3i/XdK8N6BpsJVXvNX1m9h0+wt97lY4kkuJ4hLPKyxQR75pnSONiK+keGde8V61p/hzwvomr+JPEOs3KWOk6FoOmXmsaxql7Lny7Ww0zT4bi8vbhwrFYbeGSRsZAwCR/Td/wTI/4Js6j+z/dw/Hn452lp/wtu6sJ7bwV4OjkgvY/hxp2p2r22oanqt5CZbafxpqdlPcaZ9nsJprPQdIuLyB7u91DVJ4dG/1C8CfHLhLwb8O85xmBpxxOc45VcVRwdSXNiM7ziVD2OBpVlTa9hluDfK6sotQw+HdecPa4zExjX/L+EOKfFD6XXivQx2Iyl5VwrlnssHi6mCjWnknCmSKq69ajLH14Rjj8+zG8nCMowxGNr+ylHDYTK8I/qn6y/CP4e2Hwl+Fnw6+GGmTfabH4f+CvDXhCG8MaxPqB0DSLTTZdSmjQBVuNSnt5L64wBme4kY8k16JRRX8VY3GYnMcbi8wxtWVfGY/FYjGYuvO3PWxOKqzr16srJLmqVZznKySvJ2SR/rthcPQweGw+Ew1NUsPhaFLDUKUX7tOjQpxpUqcbpu0KcYxV23ZbhRRRXKbHyF8TP2Cf2Q/i5q0uveNPgf4WfW7iWW4u9U8M3Ou+BLq/up3eSa81Q+B9W8PRateTSO0kt3qkV5cSOQzyMQMeUR/8Epf2Fo38yT4PX9woyfKm+JXxQWM8dCbfxjBLj6SA+9fopSHofof5VwPKssc3UeXYH2km3Kf1Shzyb3cpezvJvrds/Osw8HvCXOMbVzDNfDDw9zLH4ip7XEY3HcG8O4rFYipJ61MRXrZdOrXm+sqspyfc8V+En7OPwK+BFu8Hwj+FvhDwPNNbCzutW0vS0m8R39oHWQW2p+KdRa98SapAsirIsOo6rdRrIN6qHJJ9roortjGMIqMIxjGKtGMUoxilsklZJeSR9rleUZVkeBoZZkmWZfk+W4aPLhsvyvBYbL8Dh4veNDCYSlRoUo+VOnFBRRRVHoH/2Q=="
          width={26}
        />
      </TickerIcon>
      <TickerSymbol symbol={ticker} />
      <TickerPrice price={175.41} />
      <TickerPriceChange change={2.13} />
    </Ticker>
  </div>
);

export default Example;
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar
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
