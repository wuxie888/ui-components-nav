<!-- Password Strength Meter · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-meter-7
     license: MIT · category: form
     A password field with a live strength meter that scores complexity and shows a colored progress bar with a strength label. -->

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
components/ui/v-meter-7.tsx
"use client";

import { useState } from "react";
import { Input } from "@/registry/default/ui/input";
import {
  Meter,
  MeterIndicator,
  MeterLabel,
  MeterTrack,
  MeterValue,
} from "@/registry/default/ui/meter";

function getStrength(password: string): {
  color: string;
  label: string;
  value: number;
} {
  if (password.length === 0) return { color: "", label: "", value: 0 };
  let score = 0;
  if (password.length >= 8) score++;
  if (password.length >= 12) score++;
  if (/[A-Z]/.test(password)) score++;
  if (/[0-9]/.test(password)) score++;
  if (/[^A-Za-z0-9]/.test(password)) score++;

  if (score <= 1) return { color: "bg-destructive", label: "Weak", value: 20 };
  if (score === 2) return { color: "bg-orange-500", label: "Fair", value: 40 };
  if (score === 3) return { color: "bg-yellow-500", label: "Good", value: 60 };
  if (score === 4) return { color: "bg-blue-500", label: "Strong", value: 80 };
  return { color: "bg-green-500", label: "Very strong", value: 100 };
}

export default function Particle() {
  const [password, setPassword] = useState("");
  const strength = getStrength(password);

  return (
    <div className="w-full max-w-sm space-y-3">
      <Input
        onChange={(e) => setPassword(e.target.value)}
        placeholder="Enter a password"
        type="password"
        value={password}
      />
      <Meter value={strength.value}>
        <div className="flex items-center justify-between gap-2">
          <MeterLabel>Password strength</MeterLabel>
          {strength.label && <MeterValue>{() => strength.label}</MeterValue>}
        </div>
        <MeterTrack>
          <MeterIndicator className={strength.color} />
        </MeterTrack>
      </Meter>
    </div>
  );
}

demo.tsx
import PasswordStrengthMeter from "@/components/ui/v-meter-7";

export default function Default() {
  return (
    <div className="flex min-h-[320px] w-full items-center justify-center p-8">
      <PasswordStrengthMeter />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add input meter
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
