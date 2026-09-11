<!-- Login with Email and Password · @ephraimduncan · https://21st.dev/@ephraimduncan/components/login-03
     license: no-license · category: sign-in
     A centered login form with email and password fields, a sign-in button, and a reset-password link. -->

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
components/ui/login-03.tsx
'use client';

import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';

export default function Login03() {
  return (
    <div className="flex min-h-dvh items-center justify-center">
      <div className="flex flex-1 flex-col justify-center px-4 py-10 lg:px-6">
        <div className="sm:mx-auto sm:w-full sm:max-w-sm">
          <h3 className="text-balance text-center font-semibold text-foreground text-lg dark:text-foreground">
            Welcome Back
          </h3>
          <p className="text-pretty text-center text-muted-foreground text-sm dark:text-muted-foreground">
            Enter your credentials to access your account.
          </p>
          <form action="#" className="mt-6 space-y-4" method="post">
            <div>
              <Label
                className="font-medium text-foreground text-sm dark:text-foreground"
                htmlFor="email-login-03"
              >
                Email
              </Label>
              <Input
                autoComplete="email"
                className="mt-2"
                id="email-login-03"
                name="email-login-03"
                placeholder="ephraim@blocks.so"
                type="email"
              />
            </div>
            <div>
              <Label
                className="font-medium text-foreground text-sm dark:text-foreground"
                htmlFor="password-login-03"
              >
                Password
              </Label>
              <Input
                autoComplete="password"
                className="mt-2"
                id="password-login-03"
                name="password-login-03"
                placeholder="**************"
                type="password"
              />
            </div>
            <Button className="mt-4 w-full py-2 font-medium" type="submit">
              Sign in
            </Button>
          </form>
          <p className="mt-6 text-pretty text-muted-foreground text-sm dark:text-muted-foreground">
            Forgot your password?{' '}
            <a
              className="font-medium text-primary hover:text-primary/90 dark:text-primary dark:hover:text-primary/90"
              href="#"
            >
              Reset password
            </a>
          </p>
        </div>
      </div>
    </div>
  );
}

demo.tsx
import Login03 from "@/components/ui/login-03";

export default function DemoLogin03() {
  return <Login03 />;
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button input label
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
