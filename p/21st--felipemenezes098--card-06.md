<!-- Login Form Card · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/card-06
     license: MIT · category: sign-in
     A login card with email and password fields plus sign-in and Google OAuth buttons. -->

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
components/ui/card-06.tsx
import { Button } from '@/components/ui/button'
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from '@/components/ui/card'
import {
  Field,
  FieldDescription,
  FieldGroup,
  FieldLabel,
} from '@/components/ui/field'
import { Input } from '@/components/ui/input'

export function Card06() {
  return (
    <Card className="w-full max-w-sm">
      <CardHeader>
        <CardTitle>Sign in</CardTitle>
        <CardDescription>
          Enter your credentials to access your account.
        </CardDescription>
      </CardHeader>
      <CardContent>
        <form>
          <FieldGroup>
            <Field>
              <FieldLabel htmlFor="card-06-email">Email</FieldLabel>
              <Input
                id="card-06-email"
                type="email"
                placeholder="you@example.com"
              />
            </Field>
            <Field>
              <div className="flex items-center">
                <FieldLabel htmlFor="card-06-password">Password</FieldLabel>
                <Button
                  variant="link"
                  className="ml-auto h-auto p-0 text-sm font-normal"
                >
                  Forgot?
                </Button>
              </div>
              <Input id="card-06-password" type="password" />
              <FieldDescription>Must be at least 8 characters.</FieldDescription>
            </Field>
          </FieldGroup>
        </form>
      </CardContent>
      <CardFooter className="flex-col gap-2">
        <Button type="submit" className="w-full">
          Sign in
        </Button>
        <Button variant="outline" className="w-full">
          Continue with Google
        </Button>
      </CardFooter>
    </Card>
  )
}

demo.tsx
import { Card06 } from "@/components/ui/card-06";

export default function Card06Demo() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Card06 />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button card field input
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
