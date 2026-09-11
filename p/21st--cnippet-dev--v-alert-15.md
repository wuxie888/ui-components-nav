<!-- Email Verification Alert · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-alert-15
     license: no-license · category: notification
     An alert prompting the user to verify their email address, with a resend button that counts down 60 seconds before another email can be sent. -->

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
components/ui/v-alert-15.tsx
"use client";

import { MailIcon } from "lucide-react";
import { useEffect, useState } from "react";
import {
  Alert,
  AlertAction,
  AlertDescription,
  AlertTitle,
} from "@/registry/default/ui/alert";
import { Button } from "@/registry/default/ui/button";

export function Pattern() {
  const [sent, setSent] = useState(false);
  const [countdown, setCountdown] = useState(0);

  useEffect(() => {
    if (countdown <= 0) return;
    const t = setTimeout(() => setCountdown((c) => c - 1), 1000);
    return () => clearTimeout(t);
  }, [countdown]);

  const handleResend = () => {
    setSent(true);
    setCountdown(60);
  };

  return (
    <div className="w-full max-w-lg">
      <Alert>
        <MailIcon />
        <AlertTitle>Verify your email address</AlertTitle>
        <AlertAction>
          <Button
            disabled={countdown > 0}
            onClick={handleResend}
            size="xs"
            variant="outline"
          >
            {countdown > 0 ? `Resend in ${countdown}s` : "Resend"}
          </Button>
        </AlertAction>
        <AlertDescription>
          {sent
            ? "A new verification email has been sent to your inbox."
            : "We sent a confirmation link to hello@example.com. Check your inbox and click the link to activate your account."}
        </AlertDescription>
      </Alert>
    </div>
  );
}

demo.tsx
import Pattern from "@/components/ui/v-alert-15";

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
npx shadcn@latest add alert button
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
