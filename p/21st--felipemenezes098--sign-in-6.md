<!-- Sign In Split Screen · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/sign-in-6
     license: MIT · category: sign-in
     A split-screen sign-in card with a branded gradient panel, social login, and email/password form. -->

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
components/ui/sign-in-6.tsx
import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar'
import { Button } from '@/components/ui/button'
import { Card } from '@/components/ui/card'
import { Input } from '@/components/ui/input'
import { Label } from '@/components/ui/label'

function GoogleIcon() {
  return (
    <svg viewBox="0 0 24 24" className="size-4" aria-hidden>
      <path
        fill="#4285F4"
        d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92a5.06 5.06 0 0 1-2.2 3.32v2.77h3.57c2.08-1.92 3.27-4.74 3.27-8.1Z"
      />
      <path
        fill="#34A853"
        d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84A11 11 0 0 0 12 23Z"
      />
      <path
        fill="#FBBC05"
        d="M5.84 14.1a6.6 6.6 0 0 1 0-4.2V7.06H2.18a11 11 0 0 0 0 9.88l3.66-2.84Z"
      />
      <path
        fill="#EA4335"
        d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.06L5.84 9.9C6.71 7.31 9.14 5.38 12 5.38Z"
      />
    </svg>
  )
}

const proof = [
  {
    initials: 'JD',
    src: 'https://images.unsplash.com/photo-1494790108377-be9c29b29330?auto=format&fit=crop&facepad=2&w=80&h=80&q=80',
  },
  {
    initials: 'MK',
    src: 'https://images.unsplash.com/photo-1500648767791-00dcc994a43e?auto=format&fit=crop&facepad=2&w=80&h=80&q=80',
  },
  {
    initials: 'AR',
    src: 'https://images.unsplash.com/photo-1438761681033-6461ffad8d80?auto=format&fit=crop&facepad=2&w=80&h=80&q=80',
  },
]

export function SignIn6() {
  return (
    <Card className="grid w-full gap-0 p-0 md:grid-cols-2">
      <div className="from-primary to-primary/80 text-primary-foreground relative hidden flex-col justify-between overflow-hidden bg-gradient-to-b p-10 md:flex">
        <div className="bg-primary-foreground/10 pointer-events-none absolute -top-24 -right-24 size-64 rounded-full blur-3xl" />

        <div className="relative flex items-center gap-2.5">
          <div className="bg-primary-foreground/15 ring-primary-foreground/25 flex size-8 items-center justify-center rounded-lg ring-1">
            <div className="bg-primary-foreground size-3 rotate-45 rounded-[3px]" />
          </div>
          <span className="text-sm font-semibold tracking-tight">Acme</span>
        </div>

        <h2 className="relative mt-auto max-w-[15ch] text-[26px] leading-[1.15] font-semibold tracking-tight text-balance">
          Where teams ship together.
        </h2>

        <div className="relative mt-8 flex items-center gap-3">
          <div className="flex -space-x-2.5">
            {proof.map((p) => (
              <Avatar
                key={p.initials}
                className="ring-primary/60 size-7 ring-2"
              >
                <AvatarImage src={p.src} alt="" className="object-cover" />
                <AvatarFallback className="bg-primary-foreground text-primary text-[10px] font-medium">
                  {p.initials}
                </AvatarFallback>
              </Avatar>
            ))}
          </div>
          <span className="text-primary-foreground/85 text-xs">
            Join 40,000+ teams on Acme
          </span>
        </div>
      </div>

      <div className="flex flex-col justify-center gap-5 p-8">
        <div className="flex flex-col gap-1">
          <span className="text-lg font-semibold">Welcome back</span>
          <span className="text-muted-foreground text-xs">
            Sign in to your Acme workspace.
          </span>
        </div>

        <Button variant="outline" className="w-full justify-center gap-2">
          <GoogleIcon />
          Continue with Google
        </Button>

        <div className="flex items-center gap-3">
          <span className="bg-border h-px flex-1" />
          <span className="text-muted-foreground text-[11px] uppercase">
            or
          </span>
          <span className="bg-border h-px flex-1" />
        </div>

        <div className="flex flex-col gap-3">
          <div className="flex flex-col gap-1.5">
            <Label htmlFor="ss-email" className="text-xs">
              Email
            </Label>
            <Input
              id="ss-email"
              type="email"
              placeholder="you@acme.com"
              autoComplete="email"
            />
          </div>
          <div className="flex flex-col gap-1.5">
            <div className="flex items-center justify-between">
              <Label htmlFor="ss-password" className="text-xs">
                Password
              </Label>
              <a
                href="#"
                className="text-muted-foreground hover:text-foreground text-[11px]"
              >
                Forgot?
              </a>
            </div>
            <Input
              id="ss-password"
              type="password"
              placeholder="••••••••"
              autoComplete="current-password"
            />
          </div>
        </div>

        <Button className="w-full">Sign in</Button>

        <p className="text-muted-foreground text-center text-xs">
          No account?{' '}
          <a href="#" className="text-foreground font-medium hover:underline">
            Start free trial
          </a>
        </p>
      </div>
    </Card>
  )
}

demo.tsx
import { SignIn6 } from '@/components/ui/sign-in-6'

export default function Demo() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <div className="w-full max-w-3xl">
        <SignIn6 />
      </div>
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar button card input label
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
