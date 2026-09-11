<!-- Login Page 1 · @shadcnstore · https://21st.dev/@shadcnstore/components/login-page-1
     license: no-license · category: sign-in
     A responsive centered login page card with email and password fields, remember-me, forgot password, and social sign-in buttons. -->

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
components/ui/page.tsx
import { LoginPage1 } from "@/components/blocks/application/login-forms/login-page-1/components/login-page-1"

export default function Page() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center">
      <div className="w-full">
        <LoginPage1 />
      </div>
    </div>
  )
}

components/ui/login-page-1.tsx
'use client'

import { Button } from '@/components/ui/button'
import { Card } from '@/components/ui/card'
import { Input } from '@/components/ui/input'
import { CheckboxCheck } from '@/components/motion/checkbox-check'
import { EyeIcon, EyeOffIcon } from 'lucide-react'
import { useState } from 'react'
import { Mail, Lock } from 'lucide-react'
import { socialButtons } from "@/components/data/login-page-1-data"


export function LoginPage1() {
  const [showPassword, setShowPassword] = useState(false)

  return (
    <section className='from-background to-muted/50 relative isolate flex min-h-dvh w-full items-center justify-center overflow-hidden bg-linear-to-br'>
      <div className='relative z-10 container mx-auto flex min-h-dvh items-center justify-center px-4 py-12'>
        <Card className='relative w-full max-w-md ring-0 p-8 shadow-2xl'>
          <div className='mb-8 flex flex-col items-center'>
            <div className='my-4 flex justify-center'>
              <div className='bg-secondary relative size-14 rounded-full border'>
                <div className='flex h-full items-center justify-center'>
                  <svg width='32' height='32' viewBox='0 0 32 32' fill='none' xmlns='http://www.w3.org/2000/svg'>
                    <path
                      d='M26 24.75C26.4142 24.75 26.75 24.4142 26.75 24C26.75 23.5858 26.4142 23.25 26 23.25V24.75ZM26 23.25H11V24.75H26V23.25ZM8.75 21V15H7.25V21H8.75ZM11 23.25C9.75736 23.25 8.75 22.2426 8.75 21H7.25C7.25 23.0711 8.92893 24.75 11 24.75V23.25Z'
                      fill='currentColor'
                    />
                    <path
                      d='M1.5 3.25C1.08579 3.25 0.75 3.58579 0.75 4C0.75 4.41421 1.08579 4.75 1.5 4.75V3.25ZM1.5 4.75H6V3.25H1.5V4.75ZM7.25 6V21H8.75V6H7.25ZM6 4.75C6.69036 4.75 7.25 5.30964 7.25 6H8.75C8.75 4.48122 7.51878 3.25 6 3.25V4.75Z'
                      fill='currentColor'
                    />
                    <path
                      d='M22 21.75C22.4142 21.75 22.75 21.4142 22.75 21C22.75 20.5858 22.4142 20.25 22 20.25V21.75ZM22 20.25H11V21.75H22V20.25ZM8.75 18V12H7.25V18H8.75ZM11 20.25C9.75736 20.25 8.75 19.2426 8.75 18H7.25C7.25 20.0711 8.92893 21.75 11 21.75V20.25Z'
                      fill='currentColor'
                    />
                    <path
                      d='M27.2057 19.754C27.0654 20.1438 26.6357 20.346 26.246 20.2057C25.8562 20.0654 25.654 19.6357 25.7943 19.246L27.2057 19.754ZM30.0361 9.67744L29.3305 9.4234L29.3305 9.4234L30.0361 9.67744ZM25.7943 19.246L29.3305 9.4234L30.7418 9.93148L27.2057 19.754L25.7943 19.246ZM28.1543 7.75L8 7.75V6.25L28.1543 6.25V7.75ZM29.3305 9.4234C29.6237 8.60882 29.0201 7.75 28.1543 7.75V6.25C30.059 6.25 31.3869 8.13941 30.7418 9.93148L29.3305 9.4234Z'
                      fill='currentColor'
                    />
                    <path
                      d='M13.5 21.75C13.0858 21.75 12.75 21.4142 12.75 21C12.75 20.5858 13.0858 20.25 13.5 20.25V21.75ZM26.7111 19.009L27.4174 19.2613L27.4174 19.2613L26.7111 19.009ZM13.5 20.25H23.8858V21.75H13.5V20.25ZM26.0048 18.7568L27.7937 13.7477L29.2063 14.2523L27.4174 19.2613L26.0048 18.7568ZM23.8858 20.25C24.8367 20.25 25.6849 19.6522 26.0048 18.7568L27.4174 19.2613C26.8843 20.7537 25.4706 21.75 23.8858 21.75V20.25Z'
                      fill='currentColor'
                    />
                    <path d='M21.1694 10.5806L14.5651 17.1849' stroke='currentColor' />
                    <path d='M22.1694 14.5806L18.5632 18.1868' stroke='currentColor' />
                    <circle cx='13.1' cy='26.1' r='1.7' stroke='currentColor' />
                    <circle cx='22.1' cy='26.1' r='1.7' stroke='currentColor' />
                  </svg>
                </div>
              </div>
            </div>
            <h1 className='mb-2 text-center text-2xl font-bold tracking-tight'>Welcome Back!</h1>
            <p className='text-muted-foreground text-center text-sm'>Sign in to continue your journey</p>
          </div>

          {/* Login Form */}
          <form className='flex flex-col gap-6'>
            <div className='relative'>
              <Input type='email' placeholder='me@example.com' className='bg-transparent ps-10 h-9 text-sm' autoComplete='email' />
              <Mail className='text-muted-foreground absolute start-3 top-1/2 size-5 -translate-y-1/2' />
            </div>

            <div className='relative'>
              <Input
                type={showPassword ? 'text' : 'password'}
                placeholder='Password'
                className='bg-transparent ps-10 h-9 text-sm'
                autoComplete='current-password'
              />
              <Lock className='text-muted-foreground absolute start-3 top-1/2 size-5 -translate-y-1/2' />
              <Button
                type='button'
                variant='ghost'
                className='absolute end-0 top-0 h-full cursor-pointer px-3 py-2 hover:bg-transparent'
                onClick={() => setShowPassword(prev => !prev)}
              >
                {showPassword ? (
                  <EyeIcon className='text-muted-foreground' />
                ) : (
                  <EyeOffIcon className='text-muted-foreground' />
                )}
              </Button>
            </div>

            {/* Remember & Forgot */}
            <div className='flex items-center justify-between'>
              <div className='flex items-center gap-2'>
                <CheckboxCheck id='remember3' className='data-checked:bg-primary bg-transparent' />
                <label
                  htmlFor='remember3'
                  className='text-sm leading-none font-normal text-muted-foreground peer-disabled:cursor-not-allowed peer-disabled:opacity-70'
                >
                  Remember me
                </label>
              </div>
              <a href='#' className='ms-auto inline-block text-sm underline-offset-4 hover:underline'>
                Forgot password?
              </a>
            </div>

            {/* Login Button */}
            <Button className="h-9 px-4 py-2 w-full cursor-pointer" type='submit'>
              Sign In
            </Button>

            {/* Social Login */}
            <div className='relative my-6 text-center'>
              <div className='absolute inset-0 flex items-center'>
                <div className='w-full border-t'></div>
              </div>
              <div className='relative flex justify-center text-xs uppercase'>
                <span className='bg-card text-muted-foreground px-2'>Or continue with</span>
              </div>
            </div>

            <div className='grid grid-cols-3 gap-3'>
              {socialButtons.map(button => (
                <Button key={button.name} variant='outline' className="h-9 px-4 py-2 w-full cursor-pointer">
                  {button.icon}
                </Button>
              ))}
            </div>
          </form>

          {/* Sign Up Link */}
          <p className='mt-6 flex justify-center gap-1 text-center text-sm'>
            <span>Don't have an account?</span>
            <a href='#' className='underline underline-offset-4'>
              Create an account
            </a>
          </p>
        </Card>
      </div>
    </section>
  )
}

export default LoginPage1

components/ui/login-page-1-data.tsx
export const socialButtons = [
  {
    name: 'Apple',
    icon: (
      <svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'>
        <path
          d='M12.152 6.896c-.948 0-2.415-1.078-3.96-1.04-2.04.027-3.91 1.183-4.961 3.014-2.117 3.675-.546 9.103 1.519 12.09 1.013 1.454 2.208 3.09 3.792 3.039 1.52-.065 2.09-.987 3.935-.987 1.831 0 2.35.987 3.96.948 1.637-.026 2.676-1.48 3.676-2.948 1.156-1.688 1.636-3.325 1.662-3.415-.039-.013-3.182-1.221-3.22-4.857-.026-3.04 2.48-4.494 2.597-4.559-1.429-2.09-3.623-2.324-4.39-2.376-2-.156-3.675 1.09-4.61 1.09zM15.53 3.83c.843-1.012 1.4-2.427 1.245-3.83-1.207.052-2.662.805-3.532 1.818-.78.896-1.454 2.338-1.273 3.714 1.338.104 2.715-.688 3.559-1.701'
          fill='currentColor'
        />
      </svg>
    ),
  },
  {
    name: 'Google',
    icon: (
      <svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'>
        <path
          d='M12.48 10.92v3.28h7.84c-.24 1.84-.853 3.187-1.787 4.133-1.147 1.147-2.933 2.4-6.053 2.4-4.827 0-8.6-3.893-8.6-8.72s3.773-8.72 8.6-8.72c2.6 0 4.507 1.027 5.907 2.347l2.307-2.307C18.747 1.44 16.133 0 12.48 0 5.867 0 .307 5.387.307 12s5.56 12 12.173 12c3.573 0 6.267-1.173 8.373-3.36 2.16-2.16 2.84-5.213 2.84-7.667 0-.76-.053-1.467-.173-2.053H12.48z'
          fill='currentColor'
        />
      </svg>
    ),
  },
  {
    name: 'Meta',
    icon: (
      <svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'>
        <path
          d='M6.915 4.03c-1.968 0-3.683 1.28-4.871 3.113C.704 9.208 0 11.883 0 14.449c0 .706.07 1.369.21 1.973a6.624 6.624 0 0 0 .265.86 5.297 5.297 0 0 0 .371.761c.696 1.159 1.818 1.927 3.593 1.927 1.497 0 2.633-.671 3.965-2.444.76-1.012 1.144-1.626 2.663-4.32l.756-1.339.186-.325c.061.1.121.196.183.3l2.152 3.595c.724 1.21 1.665 2.556 2.47 3.314 1.046.987 1.992 1.22 3.06 1.22 1.075 0 1.876-.355 2.455-.843a3.743 3.743 0 0 0 .81-.973c.542-.939.861-2.127.861-3.745 0-2.72-.681-5.357-2.084-7.45-1.282-1.912-2.957-2.93-4.716-2.93-1.047 0-2.088.467-3.053 1.308-.652.57-1.257 1.29-1.82 2.05-.69-.875-1.335-1.547-1.958-2.056-1.182-.966-2.315-1.303-3.454-1.303zm10.16 2.053c1.147 0 2.188.758 2.992 1.999 1.132 1.748 1.647 4.195 1.647 6.4 0 1.548-.368 2.9-1.839 2.9-.58 0-1.027-.23-1.664-1.004-.496-.601-1.343-1.878-2.832-4.358l-.617-1.028a44.908 44.908 0 0 0-1.255-1.98c.07-.109.141-.224.211-.327 1.12-1.667 2.118-2.602 3.358-2.602zm-10.201.553c1.265 0 2.058.791 2.675 1.446.307.327.737.871 1.234 1.579l-1.02 1.566c-.757 1.163-1.882 3.017-2.837 4.338-1.191 1.649-1.81 1.817-2.486 1.817-.524 0-1.038-.237-1.383-.794-.263-.426-.464-1.13-.464-2.046 0-2.221.63-4.535 1.66-6.088.454-.687.964-1.226 1.533-1.533a2.264 2.264 0 0 1 1.088-.285z'
          fill='currentColor'
        />
      </svg>
    ),
  },
]

demo.tsx
import LoginPage1 from "@/components/ui/login-page-1";

export default function Default() {
  return <LoginPage1 />;
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button card checkbox input motion-checkbox-check
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
