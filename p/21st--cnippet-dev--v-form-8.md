<!-- Onboarding Wizard Form · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-form-8
     license: MIT · category: sign-up
     A three-step onboarding wizard form with account details, plan selection with a newsletter opt-in, a review summary, and a success confirmation state. -->

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
components/ui/v-form-8.tsx
"use client";

import { CheckCircle2Icon } from "lucide-react";
import { type FormEvent, useState } from "react";

import { Button } from "@/registry/default/ui/button";
import { Checkbox } from "@/registry/default/ui/checkbox";
import { Field, FieldError, FieldLabel } from "@/registry/default/ui/field";
import { Form } from "@/registry/default/ui/form";
import { Input } from "@/registry/default/ui/input";
import { Label } from "@/registry/default/ui/label";
import { Radio, RadioGroup } from "@/registry/default/ui/radio-group";

const STEPS = ["Account", "Plan", "Confirm"] as const;
type Step = 0 | 1 | 2;

const plans = [
  {
    description: "Free forever, up to 3 projects",
    id: "hobby",
    label: "Hobby",
  },
  { description: "$12/mo — unlimited projects", id: "pro", label: "Pro" },
  { description: "$49/mo — collaboration tools", id: "team", label: "Team" },
];

export function Pattern() {
  const [step, setStep] = useState<Step>(0);
  const [loading, setLoading] = useState(false);
  const [done, setDone] = useState(false);
  const [data, setData] = useState({
    email: "",
    name: "",
    newsletter: true,
    plan: "hobby",
  });

  const handleAccount = (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const fd = new FormData(e.currentTarget);
    setData((d) => ({
      ...d,
      email: fd.get("email") as string,
      name: fd.get("name") as string,
    }));
    setStep(1);
  };

  const handlePlan = (e: FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const fd = new FormData(e.currentTarget);
    setData((d) => ({
      ...d,
      newsletter: fd.get("newsletter") === "on",
      plan: fd.get("plan") as string,
    }));
    setStep(2);
  };

  const handleConfirm = async () => {
    setLoading(true);
    await new Promise((r) => setTimeout(r, 1000));
    setLoading(false);
    setDone(true);
  };

  if (done) {
    return (
      <div className="flex w-full max-w-sm flex-col items-center gap-4 py-10 text-center">
        <div className="flex size-12 items-center justify-center rounded-full bg-success/10">
          <CheckCircle2Icon className="size-6 text-success" />
        </div>
        <div className="space-y-1">
          <p className="font-semibold">Account created!</p>
          <p className="text-muted-foreground text-sm">
            Welcome, {data.name}. Check your inbox to verify your email.
          </p>
        </div>
      </div>
    );
  }

  return (
    <div className="w-full max-w-sm space-y-6">
      <div className="flex items-center gap-2">
        {STEPS.map((label, i) => (
          <div className="flex items-center gap-2" key={label}>
            <div className="flex items-center gap-1.5">
              <div
                className={`flex size-6 items-center justify-center rounded-full font-semibold text-xs transition-colors ${
                  i < step
                    ? "bg-primary text-primary-foreground"
                    : i === step
                      ? "border-2 border-primary text-primary"
                      : "border border-border text-muted-foreground"
                }`}
              >
                {i < step ? <CheckCircle2Icon className="size-3.5" /> : i + 1}
              </div>
              <span
                className={`font-medium text-xs ${i === step ? "text-foreground" : "text-muted-foreground"}`}
              >
                {label}
              </span>
            </div>
            {i < STEPS.length - 1 && (
              <div
                className={`h-px min-w-6 flex-1 ${i < step ? "bg-primary" : "bg-border"}`}
              />
            )}
          </div>
        ))}
      </div>

      {step === 0 && (
        <Form className="gap-4" onSubmit={handleAccount}>
          <Field name="name">
            <FieldLabel>Full name</FieldLabel>
            <Input
              defaultValue={data.name}
              placeholder="Alex Rivera"
              required
            />
            <FieldError />
          </Field>
          <Field name="email">
            <FieldLabel>Email</FieldLabel>
            <Input
              defaultValue={data.email}
              placeholder="you@example.com"
              required
              type="email"
            />
            <FieldError />
          </Field>
          <Button className="w-full" type="submit">
            Continue
          </Button>
        </Form>
      )}

      {step === 1 && (
        <Form className="gap-4" onSubmit={handlePlan}>
          <Field name="plan">
            <FieldLabel>Choose a plan</FieldLabel>
            <RadioGroup className="gap-3" defaultValue={data.plan} name="plan">
              {plans.map((p) => (
                <label
                  className="flex cursor-pointer items-start gap-3 rounded-lg border p-3 transition-colors hover:bg-accent/50 has-[input[data-checked]]:border-primary/40 has-[input[data-checked]]:bg-accent/50"
                  key={p.id}
                >
                  <Radio className="mt-0.5" value={p.id} />
                  <div className="flex flex-col gap-0.5">
                    <span className="font-medium text-sm">{p.label}</span>
                    <span className="text-muted-foreground text-xs">
                      {p.description}
                    </span>
                  </div>
                </label>
              ))}
            </RadioGroup>
          </Field>
          <Label className="flex items-center gap-2 font-normal text-sm">
            <Checkbox defaultChecked={data.newsletter} name="newsletter" />
            Send me product updates and tips
          </Label>
          <div className="flex gap-3">
            <Button
              className="flex-1"
              onClick={() => setStep(0)}
              type="button"
              variant="outline"
            >
              Back
            </Button>
            <Button className="flex-1" type="submit">
              Continue
            </Button>
          </div>
        </Form>
      )}

      {step === 2 && (
        <div className="space-y-4">
          <div className="divide-y rounded-lg border">
            {[
              { label: "Name", value: data.name },
              { label: "Email", value: data.email },
              {
                label: "Plan",
                value:
                  plans.find((p) => p.id === data.plan)?.label ?? data.plan,
              },
              { label: "Updates", value: data.newsletter ? "Yes" : "No" },
            ].map(({ label, value }) => (
              <div
                className="flex items-center justify-between px-4 py-3 text-sm"
                key={label}
              >
                <span className="text-muted-foreground">{label}</span>
                <span className="font-medium">{value}</span>
              </div>
            ))}
          </div>
          <div className="flex gap-3">
            <Button
              className="flex-1"
              onClick={() => setStep(1)}
              type="button"
              variant="outline"
            >
              Back
            </Button>
            <Button
              className="flex-1"
              loading={loading}
              onClick={handleConfirm}
            >
              Create account
            </Button>
          </div>
        </div>
      )}
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-form-8";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Pattern />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button checkbox form input label radio-group
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
