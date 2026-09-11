<!-- BeUI Multi-chain Swap  · @saurabh10102 · https://21st.dev/@saurabh10102/components/be-ui-multi-chain-swap
     license: unspecified · category: dropdown
      -->

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
components/motion/swap.tsx
"use client";
// beui.dev/components/blocks/swap

import { useEffect, useMemo, useState } from "react";
import { Settings } from "lucide-react";
import { useReducedMotion } from "motion/react";
import { cn } from "@/lib/utils";
import { CHAINS, TOKENS } from "./swap/data";
import { ActionButton, DestinationRow, FlipButton } from "./swap/controls";
import { Field } from "./swap/field";
import { QuoteRow } from "./swap/quote-row";
import { TokenPicker } from "./swap/token-picker";
import type { Chain, Token, TokenSide } from "./swap/types";
import { formatAmount } from "./swap/utils";

export type { Chain, Token } from "./swap/types";
export { SWAP_DRAWER_EASE } from "./swap/constants";

export interface MultiChainSwapProps {
  chains?: Chain[];
  tokens?: Token[];
  defaultFromId?: string;
  defaultToId?: string;
  className?: string;
}

export function MultiChainSwap({
  chains = CHAINS,
  tokens = TOKENS,
  defaultFromId = "eth-eth",
  defaultToId = "sol-sol",
  className,
}: MultiChainSwapProps) {
  const reduce = useReducedMotion();
  const [fromId, setFromId] = useState(defaultFromId);
  const [toId, setToId] = useState(defaultToId);
  const [amount, setAmount] = useState("1");
  const [flipRot, setFlipRot] = useState(0);
  const [quoting, setQuoting] = useState(false);
  const [picking, setPicking] = useState<TokenSide | null>(null);
  const [showDest, setShowDest] = useState(false);
  const [destAddress, setDestAddress] = useState("");

  const from = findToken(tokens, fromId);
  const to = findToken(tokens, toId);
  const fromChain = findChain(chains, from.chainId);
  const toChain = findChain(chains, to.chainId);

  const numericAmount = Number(amount) || 0;
  const quoteKey = `${numericAmount}:${fromId}:${toId}`;
  const rate = useMemo(() => {
    if (!from.usd || !to.usd) return 1;
    return from.usd / to.usd;
  }, [from.usd, to.usd]);
  const toAmount = numericAmount * rate;

  useEffect(() => {
    if (!quoteKey) return;
    if (numericAmount === 0) return;
    setQuoting(true);
    const id = setTimeout(() => setQuoting(false), 450);
    return () => clearTimeout(id);
  }, [numericAmount, quoteKey]);

  const flip = () => {
    setFlipRot((r) => r + 180);
    setFromId(toId);
    setToId(fromId);
  };

  const pickToken = (id: string) => {
    if (!picking) return;

    if (picking === "from") {
      if (id === toId) setToId(fromId);
      setFromId(id);
    } else {
      if (id === fromId) setFromId(toId);
      setToId(id);
    }

    setPicking(null);
  };

  return (
    <div
      className={cn(
        "relative isolate w-full max-w-[420px] overflow-hidden rounded-3xl",
        "border border-border/20 bg-card",
        className,
      )}
    >
      <div className="flex h-12 items-center justify-between border-b border-border/50 px-3">
        <span className="px-2 text-sm font-semibold tracking-tight text-foreground">
          Swap
        </span>
        <button
          type="button"
          aria-label="Settings"
          className="inline-flex h-8 w-8 items-center justify-center rounded-full text-muted-foreground transition-transform hover:bg-primary/5 hover:text-foreground active:scale-[0.97]"
        >
          <Settings className="h-4 w-4" />
        </button>
      </div>

      <div className="flex flex-col gap-1.5 p-4">
        <Field
          side="from"
          token={from}
          chain={fromChain}
          amount={amount}
          onAmount={setAmount}
          editable
          quoting={false}
          onOpenPicker={() => setPicking("from")}
        />

        <FlipButton rotation={flipRot} reduce={!!reduce} onClick={flip} />

        <Field
          side="to"
          token={to}
          chain={toChain}
          amount={toAmount > 0 ? formatAmount(toAmount) : ""}
          editable={false}
          quoting={quoting}
          onOpenPicker={() => setPicking("to")}
        />

        <QuoteRow
          from={from}
          to={to}
          rate={rate}
          fee={0.42}
          slippage={0.5}
          eta="≈ 24s"
          quoting={quoting}
        />

        <DestinationRow
          show={showDest}
          onToggle={() => {
            if (showDest) setDestAddress("");
            setShowDest((v) => !v);
          }}
          address={destAddress}
          onAddress={setDestAddress}
          reduce={!!reduce}
        />

        <ActionButton
          from={from}
          to={to}
          amount={numericAmount}
          destAddress={destAddress}
        />
      </div>

      <TokenPicker
        open={picking !== null}
        side={picking}
        chains={chains}
        tokens={tokens}
        selectedId={picking === "from" ? fromId : toId}
        onPick={pickToken}
        onClose={() => setPicking(null)}
        reduce={!!reduce}
      />
    </div>
  );
}

function findToken(tokens: Token[], id: string) {
  const token = tokens.find((t) => t.id === id);
  if (!token) throw new Error(`Unknown token id: ${id}`);
  return token;
}

function findChain(chains: Chain[], id: string) {
  const chain = chains.find((c) => c.id === id);
  if (!chain) throw new Error(`Unknown chain id: ${id}`);
  return chain;
}

components/motion/swap/constants.ts
// Swap-local aliases for the shared motion tokens in @/lib/ease.
export { EASE_OUT as EASE, EASE_DRAWER as SWAP_DRAWER_EASE } from "@/lib/ease";

components/motion/swap/controls.tsx
"use client";

import { useEffect, useRef } from "react";
import { AnimatePresence, motion, useReducedMotion } from "motion/react";
import { ArrowDownUp, Check, ChevronDown, Send, X } from "lucide-react";
import { SPRING_PRESS } from "@/lib/ease";
import { cn } from "@/lib/utils";
import type { Token } from "./types";
import { EASE } from "./constants";
import { isValidAddress, truncateAddress } from "./utils";

export function FlipButton({
  rotation,
  reduce,
  onClick,
}: {
  rotation: number;
  reduce: boolean;
  onClick: () => void;
}) {
  return (
    <div className="relative -my-4 flex justify-center" style={{ zIndex: 1 }}>
      <motion.button
        type="button"
        onClick={onClick}
        aria-label="Reverse direction"
        whileTap={reduce ? undefined : { scale: 0.9 }}
        animate={reduce ? undefined : { rotate: rotation }}
        transition={{ type: "spring", stiffness: 380, damping: 26, mass: 0.6 }}
        className="inline-flex h-9 w-9 items-center justify-center rounded-full border-[3px] border-card bg-primary/10 text-foreground backdrop-blur"
      >
        <ArrowDownUp className="h-3.5 w-3.5" />
      </motion.button>
    </div>
  );
}

export function ActionButton({
  from,
  to,
  amount,
  destAddress,
}: {
  from: Token;
  to: Token;
  amount: number;
  destAddress: string;
}) {
  const reduce = useReducedMotion();
  const noAmount = amount <= 0;
  const overBalance = from.balance !== undefined && amount > from.balance;
  const validDest = destAddress && isValidAddress(destAddress);
  const label = noAmount
    ? "Enter an amount"
    : overBalance
      ? `Insufficient ${from.symbol}`
      : validDest
        ? `Swap + Send to ${truncateAddress(destAddress)}`
        : `Swap ${from.symbol} → ${to.symbol}`;
  const disabled = noAmount || overBalance;

  return (
    <motion.button
      type="button"
      whileTap={disabled || reduce ? undefined : { scale: 0.97 }}
      transition={SPRING_PRESS}
      disabled={disabled}
      className={cn(
        "mt-3 inline-flex h-12 w-full items-center justify-center rounded-2xl text-sm font-semibold transition-colors",
        disabled
          ? "cursor-not-allowed bg-primary/10 text-muted-foreground"
          : "bg-primary text-primary-foreground hover:bg-primary/90",
      )}
    >
      <AnimatePresence mode="wait" initial={false}>
        <motion.span
          key={label}
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          exit={{ opacity: 0 }}
          transition={{ duration: 0.14, ease: EASE }}
        >
          {label}
        </motion.span>
      </AnimatePresence>
    </motion.button>
  );
}

export function DestinationRow({
  show,
  onToggle,
  address,
  onAddress,
  reduce,
}: {
  show: boolean;
  onToggle: () => void;
  address: string;
  onAddress: (v: string) => void;
  reduce: boolean;
}) {
  const inputRef = useRef<HTMLInputElement>(null);
  const hasAddress = address.length > 0;
  const valid = isValidAddress(address);

  useEffect(() => {
    if (!show) return;
    requestAnimationFrame(() =>
      inputRef.current?.focus({ preventScroll: true }),
    );
  }, [show]);

  return (
    <div className="mt-1 overflow-hidden rounded-xl border border-border/50 bg-background/40">
      <button
        type="button"
        onClick={onToggle}
        className="flex w-full items-center justify-between gap-2 px-3.5 py-2.5 text-[12px] text-muted-foreground hover:text-foreground"
      >
        <span className="flex items-center gap-2">
          <Send className="h-3.5 w-3.5 shrink-0" />
          <span>
            {show && hasAddress && valid
              ? `To: ${truncateAddress(address)}`
              : "Send to different address"}
          </span>
        </span>
        <motion.span
          animate={reduce ? {} : { rotate: show ? 180 : 0 }}
          transition={{ duration: 0.2, ease: EASE }}
          style={{ display: "inline-flex" }}
        >
          <ChevronDown className="h-3.5 w-3.5" />
        </motion.span>
      </button>

      <AnimatePresence>
        {show ? (
          <motion.div
            key="dest-input"
            initial={reduce ? { opacity: 0 } : { height: 0, opacity: 0 }}
            animate={reduce ? { opacity: 1 } : { height: "auto", opacity: 1 }}
            exit={reduce ? { opacity: 0 } : { height: 0, opacity: 0 }}
            transition={{ duration: 0.22, ease: EASE }}
            style={{ overflow: "hidden" }}
          >
            <div className="border-t border-border/50 px-3.5 pb-3 pt-2.5">
              <div
                className={cn(
                  "flex items-center gap-2 rounded-lg border px-2.5 py-2 transition-colors",
                  hasAddress && !valid
                    ? "border-destructive/40"
                    : "border-border",
                )}
              >
                <input
                  ref={inputRef}
                  value={address}
                  onChange={(e) => onAddress(e.target.value.trim())}
                  placeholder="0x... or name.eth"
                  spellCheck={false}
                  className="min-w-0 flex-1 bg-transparent font-mono text-[12px] text-foreground outline-none placeholder:text-muted-foreground/60"
                />
                <AnimatePresence mode="wait">
                  {hasAddress ? (
                    valid ? (
                      <motion.span
                        key="check"
                        initial={{ opacity: 0, scale: 0.7 }}
                        animate={{ opacity: 1, scale: 1 }}
                        exit={{ opacity: 0, scale: 0.7 }}
                        transition={{ duration: 0.14, ease: EASE }}
                      >
                        <Check className="h-3.5 w-3.5 shrink-0 text-primary" />
                      </motion.span>
                    ) : (
                      <motion.button
                        key="clear"
                        type="button"
                        onClick={() => onAddress("")}
                        aria-label="Clear address"
                        initial={{ opacity: 0, scale: 0.7 }}
                        animate={{ opacity: 1, scale: 1 }}
                        exit={{ opacity: 0, scale: 0.7 }}
                        transition={{ duration: 0.14, ease: EASE }}
                        className="shrink-0 text-muted-foreground hover:text-foreground"
                      >
                        <X className="h-3.5 w-3.5" />
                      </motion.button>
                    )
                  ) : null}
                </AnimatePresence>
              </div>
            </div>
          </motion.div>
        ) : null}
      </AnimatePresence>
    </div>
  );
}

components/motion/swap/data.ts
import type { Chain, Token } from "./types";

export const CHAINS: Chain[] = [
  { id: "eth", name: "Ethereum", tone: "bg-primary text-primary-foreground", symbol: "Ξ" },
  { id: "sol", name: "Solana", tone: "bg-secondary text-secondary-foreground", symbol: "◎" },
  { id: "base", name: "Base", tone: "bg-accent text-accent-foreground", symbol: "B" },
  { id: "arb", name: "Arbitrum", tone: "bg-muted text-muted-foreground", symbol: "A" },
  { id: "op", name: "Optimism", tone: "bg-destructive text-primary-foreground", symbol: "O" },
  { id: "poly", name: "Polygon", tone: "bg-primary/80 text-primary-foreground", symbol: "P" },
  { id: "bnb", name: "BNB", tone: "bg-secondary text-secondary-foreground", symbol: "B" },
  { id: "avax", name: "Avalanche", tone: "bg-destructive/80 text-primary-foreground", symbol: "A" },
];

export const TOKENS: Token[] = [
  {
    id: "eth-eth",
    symbol: "ETH",
    name: "Ether",
    chainId: "eth",
    balance: 1.245,
    usd: 3142,
    popular: true,
  },
  {
    id: "eth-weth",
    symbol: "WETH",
    name: "Wrapped Ether",
    chainId: "eth",
    balance: 0.5,
    usd: 3142,
    popular: true,
  },
  {
    id: "eth-usdc",
    symbol: "USDC",
    name: "USD Coin",
    chainId: "eth",
    balance: 4521,
    usd: 1,
    popular: true,
  },
  {
    id: "eth-usdt",
    symbol: "USDT",
    name: "Tether",
    chainId: "eth",
    balance: 2100,
    usd: 1,
    popular: true,
  },
  {
    id: "eth-dai",
    symbol: "DAI",
    name: "Dai Stablecoin",
    chainId: "eth",
    balance: 800,
    usd: 1,
    popular: true,
  },
  {
    id: "eth-wbtc",
    symbol: "WBTC",
    name: "Wrapped Bitcoin",
    chainId: "eth",
    balance: 0.04,
    usd: 96000,
    popular: true,
  },
  {
    id: "eth-link",
    symbol: "LINK",
    name: "ChainLink Token",
    chainId: "eth",
    balance: 120,
    usd: 22,
    address: "0x51...86ca",
    trending: true,
  },
  {
    id: "eth-aztec",
    symbol: "AZTEC",
    name: "AZTEC",
    chainId: "eth",
    balance: 0,
    address: "0xa2...62d2",
    trending: true,
  },
  {
    id: "eth-cfg",
    symbol: "CFG",
    name: "Centrifuge",
    chainId: "eth",
    balance: 0,
    address: "0xcc...8a94",
    trending: true,
  },
  {
    id: "eth-ondo",
    symbol: "ONDO",
    name: "Ondo",
    chainId: "eth",
    balance: 0,
    address: "0xfa...9be3",
    trending: true,
  },
  {
    id: "sol-sol",
    symbol: "SOL",
    name: "Solana",
    chainId: "sol",
    balance: 12.4,
    usd: 168,
  },
  {
    id: "sol-usdc",
    symbol: "USDC",
    name: "USD Coin",
    chainId: "sol",
    balance: 985.32,
    usd: 1,
  },
  {
    id: "base-eth",
    symbol: "ETH",
    name: "Ether",
    chainId: "base",
    balance: 0.65,
    usd: 3142,
  },
  {
    id: "base-usdc",
    symbol: "USDC",
    name: "USD Coin",
    chainId: "base",
    balance: 1200,
    usd: 1,
  },
  {
    id: "arb-arb",
    symbol: "ARB",
    name: "Arbitrum",
    chainId: "arb",
    balance: 800,
    usd: 0.95,
  },
  {
    id: "arb-eth",
    symbol: "ETH",
    name: "Ether",
    chainId: "arb",
    balance: 0.32,
    usd: 3142,
  },
  {
    id: "op-op",
    symbol: "OP",
    name: "Optimism",
    chainId: "op",
    balance: 450,
    usd: 2.8,
  },
  {
    id: "poly-matic",
    symbol: "MATIC",
    name: "Polygon",
    chainId: "poly",
    balance: 1500,
    usd: 0.75,
  },
  {
    id: "bnb-bnb",
    symbol: "BNB",
    name: "BNB",
    chainId: "bnb",
    balance: 0,
    usd: 620,
  },
  {
    id: "avax-avax",
    symbol: "AVAX",
    name: "Avalanche",
    chainId: "avax",
    balance: 0,
    usd: 38,
  },
];

components/motion/swap/field.tsx
"use client";

import { useId } from "react";
import { AnimatePresence, motion } from "motion/react";
import { ChevronDown, Loader2, Wallet } from "lucide-react";
import type { Chain, Token, TokenSide } from "./types";
import { TokenDot } from "./token-badges";
import { EASE } from "./constants";
import { formatAmount, sanitizeAmount } from "./utils";

export function Field({
  side,
  token,
  chain,
  amount,
  onAmount,
  editable,
  quoting,
  onOpenPicker,
}: {
  side: TokenSide;
  token: Token;
  chain: Chain;
  amount: string;
  onAmount?: (v: string) => void;
  editable: boolean;
  quoting: boolean;
  onOpenPicker: () => void;
}) {
  const id = useId();
  const usdValue = (Number(amount) || 0) * (token.usd ?? 0);

  return (
    <div className="relative rounded-2xl border border-border/50 bg-background/40 p-3.5">
      <label
        htmlFor={id}
        className="mb-2 block text-[11px] font-medium uppercase tracking-wider text-muted-foreground"
      >
        {side === "from" ? "You pay" : "You get"}
      </label>

      <div className="flex items-center justify-between gap-3">
        <div className="min-w-0 flex-1">
          {editable ? (
            <input
              id={id}
              inputMode="decimal"
              value={amount}
              onChange={(e) => onAmount?.(sanitizeAmount(e.target.value))}
              placeholder="0"
              className="w-full bg-transparent text-2xl font-semibold tracking-tight text-foreground tabular-nums outline-none placeholder:text-muted-foreground/60"
            />
          ) : (
            <div className="flex h-9 items-center gap-2 text-2xl font-semibold tracking-tight tabular-nums">
              <motion.span
                animate={{
                  opacity: quoting ? 0.55 : 1,
                  filter: quoting ? "blur(2px)" : "blur(0px)",
                }}
                transition={{ duration: 0.18, ease: EASE }}
                className="text-foreground"
              >
                {amount || "0"}
              </motion.span>
              <AnimatePresence>
                {quoting ? (
                  <motion.span
                    initial={{ opacity: 0, scale: 0.85 }}
                    animate={{ opacity: 1, scale: 1 }}
                    exit={{ opacity: 0, scale: 0.85 }}
                    transition={{ duration: 0.14, ease: EASE }}
                  >
                    <Loader2 className="h-4 w-4 animate-spin text-muted-foreground" />
                  </motion.span>
                ) : null}
              </AnimatePresence>
            </div>
          )}
          <p className="mt-1 text-[11px] text-muted-foreground tabular-nums">
            ≈ ${formatAmount(usdValue, 2)}
          </p>
        </div>

        <button
          type="button"
          onClick={onOpenPicker}
          className="group inline-flex h-10 items-center gap-2 rounded-full border border-border bg-card pl-1 pr-2.5 text-sm font-semibold text-foreground transition-transform hover:border-border active:scale-[0.97]"
        >
          <TokenDot token={token} chain={chain} />
          <span>{token.symbol}</span>
          <ChevronDown className="h-3.5 w-3.5 text-muted-foreground" />
        </button>
      </div>

      <div className="mt-2 flex items-center justify-between text-[11px] text-muted-foreground">
        <span className="inline-flex items-center gap-1">
          <Wallet className="h-3 w-3" />
          <span className="tabular-nums">
            {token.balance ? formatAmount(token.balance) : "0.00"}
          </span>
          <span>· {chain.name}</span>
        </span>
        {side === "from" && token.balance ? (
          <button
            type="button"
            onClick={() => onAmount?.(String(token.balance))}
            className="rounded px-1.5 py-0.5 text-[10px] font-semibold uppercase tracking-wider text-muted-foreground hover:bg-primary/5 hover:text-foreground"
          >
            Max
          </button>
        ) : null}
      </div>
    </div>
  );
}

components/motion/swap/quote-row.tsx
import { Loader2 } from "lucide-react";
import type { Token } from "./types";
import { formatAmount } from "./utils";

export function QuoteRow({
  from,
  to,
  rate,
  fee,
  slippage,
  eta,
  quoting,
}: {
  from: Token;
  to: Token;
  rate: number;
  fee: number;
  slippage: number;
  eta: string;
  quoting: boolean;
}) {
  return (
    <div className="mt-3 grid grid-cols-2 gap-x-4 gap-y-1.5 rounded-xl border border-border/50 bg-background/40 px-3.5 py-2.5 text-[11px]">
      <span className="text-muted-foreground">Rate</span>
      <span className="text-right tabular-nums text-foreground">
        {quoting ? (
          <Loader2 className="ml-auto inline h-3 w-3 animate-spin text-muted-foreground" />
        ) : (
          <>
            1 {from.symbol} ≈ {formatAmount(rate)} {to.symbol}
          </>
        )}
      </span>
      <span className="text-muted-foreground">Network fee</span>
      <span className="text-right tabular-nums text-foreground">
        ${fee.toFixed(2)}
      </span>
      <span className="text-muted-foreground">Slippage</span>
      <span className="text-right tabular-nums text-foreground">
        {slippage.toFixed(2)}%
      </span>
      <span className="text-muted-foreground">ETA</span>
      <span className="text-right text-foreground">{eta}</span>
    </div>
  );
}

components/motion/swap/token-picker.tsx
"use client";

import { useEffect, useMemo, useRef, useState } from "react";
import { AnimatePresence, motion } from "motion/react";
import { Search, X } from "lucide-react";
import { SPRING_PANEL } from "@/lib/ease";
import { cn } from "@/lib/utils";
import { EASE } from "./constants";
import { ChainChip, TokenDot } from "./token-badges";
import type { Chain, Token, TokenSide } from "./types";
import { formatAmount } from "./utils";

export function TokenPicker({
  open,
  side,
  chains,
  tokens,
  selectedId,
  onPick,
  onClose,
  reduce,
}: {
  open: boolean;
  side: TokenSide | null;
  chains: Chain[];
  tokens: Token[];
  selectedId: string;
  onPick: (id: string) => void;
  onClose: () => void;
  reduce: boolean;
}) {
  const [chainFilter, setChainFilter] = useState("all");
  const [q, setQ] = useState("");
  const inputRef = useRef<HTMLInputElement>(null);

  useEffect(() => {
    if (!open) return;
    setQ("");
    requestAnimationFrame(() =>
      inputRef.current?.focus({ preventScroll: true }),
    );
  }, [open]);

  useEffect(() => {
    if (!open) return;
    const onKey = (e: KeyboardEvent) => {
      if (e.key === "Escape") onClose();
    };
    window.addEventListener("keydown", onKey);
    return () => window.removeEventListener("keydown", onKey);
  }, [open, onClose]);

  const popular = useMemo(
    () => tokens.filter((t) => t.popular).slice(0, 6),
    [tokens],
  );
  const chainById = useMemo(
    () => new Map(chains.map((chain) => [chain.id, chain])),
    [chains],
  );

  const filtered = useMemo(() => {
    const needle = q.trim().toLowerCase();
    return tokens.filter((t) => {
      if (chainFilter !== "all" && t.chainId !== chainFilter) return false;
      if (!needle) return true;
      const chain = chains.find((c) => c.id === t.chainId);
      return [t.symbol, t.name, chain?.name, t.address].some((h) =>
        h?.toLowerCase().includes(needle),
      );
    });
  }, [q, chainFilter, tokens, chains]);

  return (
    <AnimatePresence>
      {open ? (
        <>
          <motion.button
            key="backdrop"
            type="button"
            aria-label="Close"
            onClick={onClose}
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
            transition={{ duration: 0.2, ease: EASE }}
            className="absolute inset-0 z-10 cursor-default bg-background/40 backdrop-blur-sm"
          />

          <motion.div
            key="sheet"
            initial={reduce ? { opacity: 0 } : { opacity: 0, y: "100%" }}
            animate={reduce ? { opacity: 1 } : { opacity: 1, y: 0 }}
            exit={reduce ? { opacity: 0 } : { opacity: 0, y: "100%" }}
            transition={
              reduce ? { duration: 0.18, ease: EASE } : SPRING_PANEL
            }
            className="absolute inset-x-0 bottom-0 z-20 flex max-h-[92%] flex-col rounded-t-3xl border-t border-border bg-card shadow-2xl"
            role="dialog"
            aria-modal="true"
            aria-label={`Select ${side === "from" ? "from" : "to"} token`}
          >
            <div className="flex justify-center pb-1 pt-2.5">
              <span className="h-1 w-9 rounded-full bg-primary/15" />
            </div>

            <div className="flex items-center gap-2 border-b border-border px-4 pb-3">
              <Search className="h-4 w-4 shrink-0 text-muted-foreground" />
              <input
                ref={inputRef}
                value={q}
                onChange={(e) => setQ(e.target.value)}
                placeholder="Search name or paste address"
                className="w-full bg-transparent text-sm text-foreground outline-none placeholder:text-muted-foreground/70"
              />
              <button
                type="button"
                onClick={onClose}
                aria-label="Close"
                className="inline-flex h-7 w-7 items-center justify-center rounded-full text-muted-foreground hover:bg-primary/5 hover:text-foreground"
              >
                <X className="h-3.5 w-3.5" />
              </button>
            </div>

            <div className="[scrollbar-width:none] [&::-webkit-scrollbar]:hidden flex items-center gap-1.5 overflow-x-auto border-b border-border px-3 py-5">
              <ChainChip
                active={chainFilter === "all"}
                onClick={() => setChainFilter("all")}
                label="All"
              />
              {chains.map((c) => (
                <ChainChip
                  key={c.id}
                  active={chainFilter === c.id}
                  onClick={() => setChainFilter(c.id)}
                  chain={c}
                />
              ))}
            </div>

            <div className="flex-1 overflow-y-auto px-3 pb-4 pt-3">
              {!q && chainFilter === "all" ? (
                <>
                  <p className="px-1 pb-2 text-[11px] font-semibold uppercase tracking-wider text-muted-foreground">
                    Most popular
                  </p>
                  <div className="[scrollbar-width:none] [&::-webkit-scrollbar]:hidden flex items-center gap-1.5 overflow-x-auto pb-3">
                    {popular.map((t) => {
                      const chain = chainById.get(t.chainId);
                      if (!chain) return null;
                      return (
                        <button
                          key={t.id}
                          type="button"
                          onClick={() => onPick(t.id)}
                          className="inline-flex shrink-0 items-center gap-2 rounded-full border border-border bg-background/50 py-1 pl-1 pr-3 text-sm font-semibold text-foreground transition-transform hover:border-border active:scale-[0.97]"
                        >
                          <TokenDot token={t} chain={chain} size={22} />
                          {t.symbol}
                        </button>
                      );
                    })}
                  </div>
                </>
              ) : null}

              <p className="px-1 pb-1.5 text-[11px] font-semibold uppercase tracking-wider text-muted-foreground">
                {q ? "Results" : chainFilter === "all" ? "Trending" : "Tokens"}
              </p>
              <ul className="flex flex-col gap-0.5">
                {filtered.length === 0 ? (
                  <li className="py-8 text-center text-xs text-muted-foreground">
                    No tokens found
                  </li>
                ) : null}
                {filtered.map((t) => {
                  const chain = chainById.get(t.chainId);
                  if (!chain) return null;
                  const active = t.id === selectedId;
                  return (
                    <li key={t.id}>
                      <button
                        type="button"
                        onClick={() => onPick(t.id)}
                        className={cn(
                          "flex w-full items-center justify-between rounded-xl px-2 py-2 text-left transition-colors active:scale-[0.97]",
                          active
                            ? "bg-primary/5"
                            : "hover:bg-primary/[0.04]",
                        )}
                      >
                        <span className="flex min-w-0 items-center gap-2.5">
                          <TokenDot token={t} chain={chain} size={32} />
                          <span className="flex min-w-0 flex-col">
                            <span className="truncate text-sm font-semibold text-foreground">
                              {t.name}
                            </span>
                            <span className="truncate text-[11px] text-muted-foreground">
                              {t.symbol}
                            </span>
                          </span>
                        </span>
                        <span className="shrink-0 text-right text-[11px] tabular-nums text-muted-foreground">
                          {t.address ??
                            (t.balance ? formatAmount(t.balance) : "")}
                        </span>
                      </button>
                    </li>
                  );
                })}
              </ul>
            </div>
          </motion.div>
        </>
      ) : null}
    </AnimatePresence>
  );
}

components/motion/swap/types.ts
export type Chain = {
  id: string;
  name: string;
  tone: string;
  symbol: string;
};

export type Token = {
  id: string;
  symbol: string;
  name: string;
  chainId: string;
  address?: string;
  balance?: number;
  usd?: number;
  trending?: boolean;
  popular?: boolean;
};

export type TokenSide = "from" | "to";

components/motion/swap/utils.ts
export function isValidAddress(v: string) {
  if (/^0x[0-9a-fA-F]{40}$/.test(v)) return true;
  if (/\.(eth|sol|bnb)$/.test(v) && v.length > 5) return true;
  return false;
}

export function truncateAddress(v: string) {
  if (v.startsWith("0x") && v.length === 42) {
    return `${v.slice(0, 6)}...${v.slice(-4)}`;
  }
  return v;
}

export function sanitizeAmount(v: string) {
  const cleaned = v.replace(/[^0-9.]/g, "");
  const parts = cleaned.split(".");
  if (parts.length <= 1) return cleaned;
  return `${parts[0]}.${parts.slice(1).join("")}`;
}

export function formatAmount(n: number, max = 6) {
  if (!Number.isFinite(n)) return "0";
  if (n === 0) return "0";
  if (n >= 1000) {
    return n.toLocaleString(undefined, { maximumFractionDigits: 2 });
  }
  return n.toLocaleString(undefined, { maximumFractionDigits: max });
}

lib/utils.ts
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}

lib/ease.ts
// Shared motion tokens. Easing curves mirror the CSS custom properties in
// globals.css; springs are the canonical physics used across components.
// Strong custom variants — defaults like `ease-in`/`ease-out` feel weak.

export const EASE_OUT = [0.16, 1, 0.3, 1] as const;
export const EASE_IN_OUT = [0.77, 0, 0.175, 1] as const;
export const EASE_DRAWER = [0.32, 0.72, 0, 1] as const;

/** CSS string form of EASE_OUT for inline style transitions. */
export const EASE_OUT_CSS = "cubic-bezier(0.16, 1, 0.3, 1)";

/** Press feedback on buttons and other tappable surfaces. */
export const SPRING_PRESS = {
  type: "spring",
  stiffness: 500,
  damping: 30,
  mass: 0.6,
} as const;

/** Content swaps — label/icon slots trading places inside a control. */
export const SPRING_SWAP = {
  type: "spring",
  stiffness: 460,
  damping: 30,
  mass: 0.55,
} as const;

/** Overlay panel entrances — modals and sheets summoned by pointer. */
export const SPRING_PANEL = {
  type: "spring",
  stiffness: 420,
  damping: 40,
  mass: 0.5,
} as const;

/** Shared-layout glides — pills, indicators and panels morphing between positions. */
export const SPRING_LAYOUT = {
  type: "spring",
  stiffness: 360,
  damping: 32,
  mass: 0.6,
} as const;

/** Cursor-follow physics for decorative mouse tracking (magnetic, tilt, dock). */
export const SPRING_MOUSE = {
  stiffness: 200,
  damping: 15,
  mass: 0.3,
} as const;

/** Dragged handles and fills (sliders) — critically damped `useSpring` config,
 * so the value follows the pointer butterily and never rebounds off an end. */
export const SPRING_GLIDE = {
  stiffness: 700,
  damping: 50,
  mass: 0.5,
} as const;

components/motion/swap/token-badges.tsx
import { cn } from "@/lib/utils";
import type { Chain, Token } from "./types";

export function ChainChip({
  active,
  onClick,
  chain,
  label,
}: {
  active: boolean;
  onClick: () => void;
  chain?: Chain;
  label?: string;
}) {
  return (
    <button
      type="button"
      onClick={onClick}
      aria-pressed={active}
      className={cn(
        "inline-flex h-9 shrink-0 items-center justify-center rounded-xl border transition-colors active:scale-[0.97] transition-transform",
        chain ? "w-9" : "px-3",
        active
          ? "border-primary/20 bg-primary/5 text-foreground"
          : "border-border/60 bg-background/40 text-foreground hover:border-border",
      )}
      title={chain?.name ?? label}
    >
      {chain ? (
        <ChainDot chain={chain} size={20} />
      ) : (
        <span className="text-xs font-semibold">{label}</span>
      )}
    </button>
  );
}

export function ChainDot({ chain, size = 16 }: { chain: Chain; size?: number }) {
  return (
    <span
      style={{ width: size, height: size }}
      className={cn(
        "inline-flex shrink-0 items-center justify-center rounded-full text-[9px] font-bold",
        chain.tone,
      )}
    >
      {chain.symbol}
    </span>
  );
}

export function TokenDot({
  token,
  chain,
  size = 28,
}: {
  token: Token;
  chain: Chain;
  size?: number;
}) {
  return (
    <span
      className="relative inline-flex shrink-0"
      style={{ width: size, height: size }}
    >
      <span className="absolute inset-0 inline-flex items-center justify-center rounded-full border border-border bg-background text-[11px] font-bold text-foreground">
        {token.symbol.slice(0, 2)}
      </span>
      <span
        style={{
          width: size * 0.42,
          height: size * 0.42,
        }}
        className={cn(
          "absolute -bottom-0.5 -right-0.5 inline-flex items-center justify-center rounded-full border-2 border-card text-[7px] font-bold",
          chain.tone,
        )}
      >
        {chain.symbol}
      </span>
    </span>
  );
}

demo.tsx
"use client";

import { MultiChainSwap } from "@/components/ui/be-ui-multi-chain-swap";

export default function SwapPreview() {
  return (
    <div className="flex w-full items-center justify-center">
      <MultiChainSwap />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install clsx lucide-react motion tailwind-merge
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
