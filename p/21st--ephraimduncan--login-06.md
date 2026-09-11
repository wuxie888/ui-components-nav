<!-- Login with Magic Link and SSO · @ephraimduncan · https://21st.dev/@ephraimduncan/components/login-06
     license: MIT · category: form
     A centered card login form offering magic-link email sign-in, password fallback, and single sign-on (SSO). -->

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
components/ui/login-06.tsx
import type { JSX, SVGProps } from 'react';
import { Button } from '@/components/ui/button';
import { Card, CardContent } from '@/components/ui/card';
import { Input } from '@/components/ui/input';
import { Separator } from '@/components/ui/separator';

const Logo = (props: JSX.IntrinsicAttributes & SVGProps<SVGSVGElement>) => (
  <svg
    fill="currentColor"
    height="48"
    viewBox="0 0 40 48"
    width="40"
    {...props}
  >
    <clipPath id="a">
      <path d="m0 0h40v48h-40z" />
    </clipPath>
    <g clipPath="url(#a)">
      <path d="m25.0887 5.05386-3.933-1.05386-3.3145 12.3696-2.9923-11.16736-3.9331 1.05386 3.233 12.0655-8.05262-8.0526-2.87919 2.8792 8.83271 8.8328-10.99975-2.9474-1.05385625 3.933 12.01860625 3.2204c-.1376-.5935-.2104-1.2119-.2104-1.8473 0-4.4976 3.646-8.1436 8.1437-8.1436 4.4976 0 8.1436 3.646 8.1436 8.1436 0 .6313-.0719 1.2459-.2078 1.8359l10.9227 2.9267 1.0538-3.933-12.0664-3.2332 11.0005-2.9476-1.0539-3.933-12.0659 3.233 8.0526-8.0526-2.8792-2.87916-8.7102 8.71026z" />
      <path d="m27.8723 26.2214c-.3372 1.4256-1.0491 2.7063-2.0259 3.7324l7.913 7.9131 2.8792-2.8792z" />
      <path d="m25.7665 30.0366c-.9886 1.0097-2.2379 1.7632-3.6389 2.1515l2.8794 10.746 3.933-1.0539z" />
      <path d="m21.9807 32.2274c-.65.1671-1.3313.2559-2.0334.2559-.7522 0-1.4806-.102-2.1721-.2929l-2.882 10.7558 3.933 1.0538z" />
      <path d="m17.6361 32.1507c-1.3796-.4076-2.6067-1.1707-3.5751-2.1833l-7.9325 7.9325 2.87919 2.8792z" />
      <path d="m13.9956 29.8973c-.9518-1.019-1.6451-2.2826-1.9751-3.6862l-10.95836 2.9363 1.05385 3.933z" />
    </g>
  </svg>
);

export default function Login06() {
  return (
    <div className="flex min-h-dvh items-center justify-center">
      <Card className="w-full max-w-sm rounded-4xl px-6 py-10 pt-14 shadow-2xs">
        <CardContent className="">
          <div className="flex flex-col items-center space-y-8">
            <Logo />

            <div className="space-y-2 text-center">
              <h1 className="text-balance font-semibold text-3xl text-foreground">
                Welcome back!
              </h1>
              <p className="text-pretty text-muted-foreground text-sm">
                First time here?{' '}
                <a className="text-foreground hover:underline" href="#">
                  Sign up for free
                </a>
              </p>
            </div>

            <div className="w-full space-y-4">
              <Input
                className="w-full rounded-xl"
                placeholder="Your email"
                type="email"
              />
              <div className="flex flex-col gap-2">
                <Button className="w-full rounded-xl" size="lg">
                  Send me the magic link
                </Button>
                <Button
                  className="w-full text-muted-foreground text-sm"
                  variant="link"
                >
                  Sign in using password
                </Button>
              </div>

              <div className="flex items-center gap-4 py-2">
                <Separator className="flex-1" />
                <span className="text-muted-foreground text-sm">OR</span>
                <Separator className="flex-1" />
              </div>

              <Button className="w-full rounded-xl" size="lg" variant="outline">
                Single sign-on (SSO)
              </Button>
            </div>

            <p className="w-11/12 text-pretty text-center text-muted-foreground text-xs">
              You acknowledge that you read, and agree, to our{' '}
              <a className="underline hover:text-foreground" href="#">
                Terms of Service
              </a>{' '}
              and our{' '}
              <a className="underline hover:text-foreground" href="#">
                Privacy Policy
              </a>
              .
            </p>
          </div>
        </CardContent>
      </Card>
    </div>
  );
}

demo.tsx
import Login06 from "@/components/ui/login-06";

export default function Default() {
  return <Login06 />;
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button card input separator
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
