<!-- Answer Router FAQ · @ziegfiroyt · https://21st.dev/@ziegfiroyt/components/faq92
     license: beste ui license · category: faq
     A help section that routes visitors to a written answer by asking two quick multiple-choice questions instead of a search box, showing a live assistant card beside the answer and a contact panel underneath. -->

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
components/ui/faq92.tsx
"use client";

import { ArrowUpRight, RotateCcw } from "lucide-react";
import Link from "next/link";
import { type FormEvent, type ReactNode, useState } from "react";
import { Badge23 } from "@/components/beste/component/badge23";
import { Button21 } from "@/components/beste/component/button21";
import { Chat34 } from "@/components/beste/piece/chat34";
import {
  Questionnaire,
  QuestionnaireActions,
  QuestionnaireChoice,
  QuestionnaireChoiceDescription,
  QuestionnaireChoices,
  QuestionnaireDescription,
  QuestionnaireError,
  QuestionnaireItem,
  QuestionnaireNext,
  QuestionnairePrevious,
  QuestionnaireProgress,
  QuestionnaireSubmit,
  QuestionnaireTitle,
} from "@/components/ui/questionnaire";
import { cn } from "@/lib/utils";

interface Badge {
  label: string;
}

interface ActionLink {
  label: string;
  href: string;
}

interface RouterImage {
  src: string;
  alt: string;
}

interface RouterChoice {
  value: string;
  label: string;
  description?: string;
}

interface RouterQuestion {
  /** Form field name. Must be unique across the flow. */
  name: string;
  title: string;
  description?: string;
  choices?: RouterChoice[];
}

interface Answer {
  /**
   * Field name to answer value. An answer is only shown when every pair
   * matches. Leave it out on the last entry, which is the catch-all.
   */
  match?: Record<string, string>;
  title: string;
  description: string;
  link?: ActionLink;
  /** Replaces the standing photograph once this answer is shown */
  image?: RouterImage;
  /** Piece floated over the photograph once this answer is shown */
  media?: ReactNode;
  caption?: string;
}

interface Contact {
  title: string;
  description: string;
  link: ActionLink;
}

interface Faq92Labels {
  previous?: string;
  next?: string;
  submit?: string;
  restart?: string;
  answerTitle?: string;
}

interface Faq92Props {
  badge?: Badge;
  heading?: string;
  description?: string;
  questions?: RouterQuestion[];
  /** Prints a scoped shortcut on every choice and binds it while the question is active */
  shortcuts?: "letters" | "numbers";
  /** Checked in order; the first one whose `match` fits the answers is shown. */
  answers?: Answer[];
  /** Standing photograph, shown until an answer is found */
  image?: RouterImage;
  /** Piece floated over the photograph before an answer is found */
  media?: ReactNode;
  caption?: string;
  /** Always shown under the photograph, for the times the routing was not enough */
  contact?: Contact;
  labels?: Faq92Labels;
  className?: string;
}

export const faq92Demo: Faq92Props = {
  badge: { label: "Help" },
  heading: "Two questions instead of a search box",
  description:
    "Most support tickets are one of a dozen questions wearing different words. Tell us where you are and what happened, and we will point at the page that answers it.",
  shortcuts: "numbers",
  image: {
    src: "https://images.unsplash.com/photo-1579508750794-1959a573f5e6?q=80&w=2940&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
    alt: "An open notebook with a pen and a pair of glasses resting on it",
  },
  media: (
    <Chat34
      question="How do I change the card on the account?"
      answer="Any admin can update it in billing settings. The change applies to the next invoice rather than the one already issued."
      sourcesLabel="Answered from"
      sources={["Billing", "Roles"]}
    />
  ),
  caption: "Four people answer these, and they wrote the pages the router points at.",
  labels: {
    submit: "Show me the answer",
    restart: "Ask about something else",
    answerTitle: "Start here",
  },
  questions: [
    {
      name: "area",
      title: "What is this about?",
      description: "Pick the closest one. Nothing here creates a ticket.",
      choices: [
        {
          value: "billing",
          label: "Billing and invoices",
          description: "Charges, receipts, plans, and refunds.",
        },
        {
          value: "access",
          label: "Getting into the account",
          description: "Passwords, devices, and team access.",
        },
        {
          value: "data",
          label: "Data going in or out",
          description: "Imports, exports, and integrations.",
        },
      ],
    },
    {
      name: "state",
      title: "Where did it stop working?",
      choices: [
        { value: "before", label: "Before I could start" },
        { value: "during", label: "Halfway through, with an error" },
        { value: "after", label: "It finished, but the result is wrong" },
      ],
    },
  ],
  answers: [
    {
      match: { area: "billing", state: "after" },
      title: "The invoice does not match what you expected",
      description:
        "Mid-cycle plan changes are prorated to the day, so the next invoice carries both the credit and the new rate. The breakdown on the invoice shows each line separately.",
      link: { label: "Read how proration is calculated", href: "https://beste.co" },
      image: {
        src: "https://images.unsplash.com/photo-1554224155-6726b3ff858f?w=1200&h=1400&fit=crop",
        alt: "Printed forms spread across a desk with a pen and a calculator",
      },
      media: (
        <Chat34
          question="Why is this invoice higher than the plan price?"
          answer="You changed plan on the 14th, so this one carries nine days at the old rate and the rest at the new one."
          sourcesLabel="Answered from"
          sources={["Invoices", "Proration"]}
        />
      ),
      caption: "Every invoice line is itemised, including the credit from the day you switched.",
    },
    {
      match: { area: "billing" },
      title: "Payments, plans, and receipts",
      description:
        "Card updates, VAT details, and past receipts all live in the billing settings, and any admin on the workspace can reach them.",
      link: { label: "Open the billing guide", href: "https://beste.co" },
    },
    {
      match: { area: "access", state: "before" },
      title: "You cannot get past the sign-in screen",
      description:
        "Reset links expire after fifteen minutes and only work once. If yours has run out, ask for a new one and open it in the same browser.",
      link: { label: "Reset your password", href: "https://beste.co" },
      image: {
        src: "https://images.unsplash.com/photo-1555421689-d68471e189f2?w=1200&h=1400&fit=crop",
        alt: "Somebody sitting at a desktop computer, waiting on the screen",
      },
      media: (
        <Chat34
          question="My reset link says it has already been used."
          answer="They last fifteen minutes and work once. Ask for a new one and open it in the browser you asked from."
          sourcesLabel="Answered from"
          sources={["Sign-in", "Sessions"]}
        />
      ),
      caption: "Reset links are single use, which is why the second click never works.",
    },
    {
      match: { area: "access" },
      title: "Access, devices, and who can see what",
      description:
        "Roles decide what each person can open. An admin can change a role at any time, and the change applies the next time that person loads the page.",
      link: { label: "See the roles table", href: "https://beste.co" },
    },
    {
      match: { area: "data", state: "during" },
      title: "An import stopped partway",
      description:
        "Nothing is written until the whole file validates, so a failed import leaves your data untouched. The error names the first row it could not read.",
      link: { label: "Fix a rejected import", href: "https://beste.co" },
      image: {
        src: "https://images.unsplash.com/photo-1543286386-713bdd548da4?w=1200&h=1400&fit=crop",
        alt: "A line chart drawn on graph paper beside a ruler and two pens",
      },
      media: (
        <Chat34
          question="The import stopped on row 412."
          answer="Nothing was written. Row 412 carries a date in a format we do not read, and the whole file goes through once it is fixed."
          sourcesLabel="Answered from"
          sources={["Imports", "Field mapping"]}
        />
      ),
      caption: "A rejected file changes nothing. The row number in the error is where to look.",
    },
    {
      title: "Moving data in and out",
      description:
        "Exports run in the background and arrive by email as a signed link. Imports are checked against your existing records before anything is created.",
      link: { label: "Read the data guide", href: "https://beste.co" },
    },
  ],
  contact: {
    title: "Still not it?",
    description: "Send us the answers you just gave and a sentence about what you expected.",
    link: { label: "Write to support", href: "https://beste.co" },
  },
};

export function Faq92({
  badge,
  heading,
  description,
  questions = [],
  shortcuts,
  answers = [],
  image,
  media,
  caption,
  contact,
  labels = {},
  className,
}: Faq92Props) {
  const {
    previous: previousLabel,
    next: nextLabel,
    submit: submitLabel,
    restart: restartLabel,
    answerTitle,
  } = labels;

  const [answer, setAnswer] = useState<Answer | null>(null);

  const handleSubmit = (event: FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    const data = new FormData(event.currentTarget);
    const given: Record<string, string> = {};
    for (const question of questions) {
      given[question.name] = String(data.get(question.name) ?? "");
    }

    // The last entry stands in when nothing fits, so a submit never dead-ends.
    const matched = answers.find((entry) =>
      Object.entries(entry.match ?? {}).every(([name, value]) => given[name] === value)
    );
    setAnswer(matched ?? answers[answers.length - 1] ?? null);
  };

  const restart = () => setAnswer(null);

  const shownImage = answer?.image ?? image;
  const shownMedia = answer?.media ?? media;
  const shownCaption = answer?.caption ?? caption;

  return (
    <section className={cn("w-full bg-background py-16 md:py-24", className)}>
      <div className="mx-auto max-w-6xl px-4 md:px-6">
        {badge && <Badge23 label={badge.label} />}

        <div className="mt-6 border-t border-border pt-8 md:pt-10">
          <div className="grid gap-6 md:grid-cols-2 md:gap-12">
            {heading && (
              <h2 className="text-3xl font-light leading-[1.1] tracking-tight text-foreground md:text-5xl">
                {heading}
              </h2>
            )}
            {description && (
              <div className="flex md:justify-end">
                <p className="max-w-md text-base leading-relaxed text-muted-foreground">
                  {description}
                </p>
              </div>
            )}
          </div>
        </div>

        <div className="mt-12 grid gap-8 md:mt-16 lg:grid-cols-[minmax(0,1.1fr)_minmax(0,1fr)] lg:gap-12">
          <div>
            {answer ? (
              <>
                {answerTitle && <p className="text-sm text-muted-foreground">{answerTitle}</p>}
                <p className="mt-3 text-2xl font-light tracking-tight text-foreground md:text-3xl">
                  {answer.title}
                </p>
                <p className="mt-4 text-base leading-relaxed text-muted-foreground">
                  {answer.description}
                </p>

                {answer.link && (
                  <Link
                    href={answer.link.href}
                    className="group/faq92 mt-6 inline-flex items-center gap-2 border-b border-primary/40 pb-1 text-base text-foreground transition-colors hover:text-primary"
                  >
                    {answer.link.label}
                    <ArrowUpRight
                      className="size-4 transition-transform motion-safe:group-hover/faq92:-translate-y-0.5 motion-safe:group-hover/faq92:translate-x-0.5"
                      aria-hidden="true"
                    />
                  </Link>
                )}

                <div className="mt-8">
                  <Button21
                    label={restartLabel ?? "Ask about something else"}
                    tone="outline"
                    icon={RotateCcw}
                    onClick={restart}
                  />
                </div>
              </>
            ) : (
              <Questionnaire
                defaultItem={questions[0]?.name}
                shortcuts={shortcuts}
                onSubmit={handleSubmit}
                className="gap-6"
              >
                {questions.length > 0 && (
                  <QuestionnaireProgress className="text-sm font-normal tracking-normal" />
                )}

                {questions.map((question, index) => (
                  <QuestionnaireItem key={index} name={question.name} required>
                    <QuestionnaireTitle className="text-xl font-medium text-foreground">
                      {question.title}
                    </QuestionnaireTitle>
                    {question.description && (
                      <QuestionnaireDescription className="text-base leading-relaxed">
                        {question.description}
                      </QuestionnaireDescription>
                    )}
                    <QuestionnaireChoices>
                      {question.choices?.map((choice, choiceIndex) => (
                        <QuestionnaireChoice
                          key={choiceIndex}
                          value={choice.value}
                          className="rounded-md border-border p-4 text-base"
                        >
                          <span className="font-medium text-foreground">{choice.label}</span>
                          {choice.description && (
                            <QuestionnaireChoiceDescription className="text-sm">
                              {choice.description}
                            </QuestionnaireChoiceDescription>
                          )}
                        </QuestionnaireChoice>
                      ))}
                    </QuestionnaireChoices>
                    <QuestionnaireError className="text-sm" />
                  </QuestionnaireItem>
                ))}

                <QuestionnaireActions>
                  <QuestionnairePrevious className="cursor-pointer rounded-md border-border text-sm font-medium">
                    {previousLabel}
                  </QuestionnairePrevious>
                  <QuestionnaireNext className="cursor-pointer rounded-md bg-primary text-sm font-medium text-primary-foreground hover:bg-primary/90">
                    {nextLabel}
                  </QuestionnaireNext>
                  <QuestionnaireSubmit className="cursor-pointer rounded-md bg-primary text-sm font-medium text-primary-foreground hover:bg-primary/90">
                    {submitLabel}
                  </QuestionnaireSubmit>
                </QuestionnaireActions>
              </Questionnaire>
            )}
          </div>

          <div>
            <div className="relative flex h-72 items-center justify-center overflow-hidden rounded-md bg-muted lg:h-[24rem]">
              {shownImage && (
                <img
                  className="absolute inset-0 size-full object-cover"
                  src={shownImage.src}
                  alt={shownImage.alt}
                />
              )}
              <div className="relative z-10 size-full">{shownMedia}</div>
            </div>

            {shownCaption && (
              <p className="mt-5 border-t border-border pt-5 text-sm leading-relaxed text-muted-foreground">
                {shownCaption}
              </p>
            )}
          </div>
        </div>

        {contact && (
          <div className="mt-10 flex flex-col gap-4 rounded-md bg-muted p-6 sm:flex-row sm:items-center sm:justify-between md:p-8">
            <div className="max-w-xl">
              <p className="text-lg font-medium text-foreground">{contact.title}</p>
              <p className="mt-2 text-base leading-relaxed text-muted-foreground">
                {contact.description}
              </p>
            </div>
            <Button21 asChild label={contact.link.label} tone="outline">
              <Link href={contact.link.href} />
            </Button21>
          </div>
        )}
      </div>
    </section>
  );
}

components/ui/chat34.tsx
"use client";

import { Sparkles } from "lucide-react";
import { cn } from "@/lib/utils";

interface Chat34Props {
  question?: string;
  answer?: string;
  sourcesLabel?: string;
  sources?: string[];
  className?: string;
}

export const chat34Demo: Chat34Props = {
  question: "How many rooms were unused last Tuesday?",
  answer:
    "Three, all at Kingsway between 13:00 and 16:00. Two were held for a clinic that was cancelled on the Friday before.",
  sourcesLabel: "Answered from",
  sources: ["Rota", "Bookings", "Cancellations"],
};

export function Chat34({ question, answer, sourcesLabel, sources = [], className }: Chat34Props) {
  return (
    <div className={cn("relative flex size-full items-center justify-center p-4", className)}>
      <div className="w-full max-w-96 rounded-md border border-border bg-card p-5 shadow-xl">
        {question && (
          <p className="ml-auto w-fit max-w-[85%] rounded-md bg-muted px-3 py-2 text-sm text-card-foreground">
            {question}
          </p>
        )}

        <div className="mt-3 flex gap-2.5">
          <span
            className="flex size-7 shrink-0 items-center justify-center rounded-md bg-primary/10 text-primary"
            aria-hidden="true"
          >
            <Sparkles className="size-3.5" />
          </span>
          {answer && (
            <p className="text-sm leading-relaxed text-card-foreground">{answer}</p>
          )}
        </div>

        {sources.length > 0 && (
          <div className="mt-4 flex flex-wrap items-center gap-2 border-t border-border pt-3">
            {sourcesLabel && (
              <span className="text-sm text-muted-foreground">{sourcesLabel}</span>
            )}
            {sources.map((source, index) => (
              <span
                key={index}
                className="rounded-full border border-border px-2 py-0.5 text-xs text-muted-foreground"
              >
                {source}
              </span>
            ))}
          </div>
        )}
      </div>
    </div>
  );
}

components/ui/badge23.tsx
"use client";

import { cn } from "@/lib/utils";

type Tone = "muted" | "foreground" | "primary";

interface Badge23Props {
  /** Eyebrow label (rendered uppercase) */
  label: string;
  /** Text/border tone */
  tone?: Tone;
  /** Additional classes merged onto the root */
  className?: string;
}

const toneStyles: Record<Tone, string> = {
  muted: "border-border text-muted-foreground",
  foreground: "border-foreground/30 text-foreground",
  primary: "border-primary/40 text-primary",
};

export const badge23Demo: Badge23Props = {
  label: "Product",
};

export function Badge23({ label, tone = "muted", className }: Badge23Props) {
  return (
    <span
      className={cn(
        "inline-flex items-center rounded-md border bg-transparent px-2.5 py-1 font-mono text-sm uppercase leading-none tracking-widest",
        toneStyles[tone],
        className
      )}
    >
      {label}
    </span>
  );
}

components/ui/button21.tsx
"use client";

import type { LucideIcon } from "lucide-react";
import * as React from "react";
import { Button } from "@/components/ui/button";
import { cn } from "@/lib/utils";

type Tone = "primary" | "neutral" | "outline";

interface Button21Props {
  /** Button label */
  label: string;
  /**
   * Compose the rendered element shadcn-style (radix asChild): your element
   * (e.g. a router Link) becomes the root and the button content is injected
   * as its children. Pass the element without children of its own.
   */
  asChild?: boolean;
  /** The element to render when `asChild` is set (e.g. your framework's Link) */
  children?: React.ReactElement;
  /**
   * Fallback link: renders a plain `<a>`. Prefer `asChild` with your
   * framework's Link component for client-side navigation.
   */
  href?: string;
  /** Optional trailing icon */
  icon?: LucideIcon;
  /** Surface tone: solid accent (default), soft neutral pill, or hairline outline */
  tone?: Tone;
  /** Click handler (used when there is no href and no asChild) */
  onClick?: React.MouseEventHandler<HTMLButtonElement>;
  /** Additional classes merged onto the button */
  className?: string;
}

const toneStyles: Record<Tone, string> = {
  primary: "bg-primary text-primary-foreground hover:bg-primary/90",
  neutral: "bg-muted text-foreground hover:bg-muted/70",
  outline: "border border-border bg-transparent text-foreground hover:bg-muted",
};

export const button21Demo: Button21Props = {
  label: "Book a demo",
};

export function Button21({
  label,
  asChild = false,
  children,
  href,
  icon: Icon,
  tone = "primary",
  onClick,
  className,
}: Button21Props) {
  const classes = cn(
    "group/button21 h-auto w-fit gap-2 rounded-md px-5 py-2.5 text-sm font-medium tracking-tight transition-colors",
    toneStyles[tone],
    className
  );

  const inner = (
    <>
      {label}
      {Icon && (
        <Icon className="size-4 transition-transform motion-safe:group-hover/button21:translate-x-0.5" />
      )}
    </>
  );

  if (asChild && React.isValidElement(children)) {
    return (
      <Button asChild className={classes}>
        {React.cloneElement(children, undefined, inner)}
      </Button>
    );
  }

  if (href) {
    return (
      <Button asChild className={classes}>
        <a href={href}>{inner}</a>
      </Button>
    );
  }

  return (
    <Button type="button" className={classes} onClick={onClick}>
      {inner}
    </Button>
  );
}

demo.tsx
"use client";

import { Faq92, faq92Demo } from "@/components/ui/faq92";

export default function Faq92Demo() {
  return <Faq92 {...faq92Demo} />;
}
```

Install NPM dependencies:
```bash
npm install clsx lucide-react tailwind-merge
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button questionnaire
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
