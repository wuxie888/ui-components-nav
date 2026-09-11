<!-- Auth Divider · @efferd · https://21st.dev/@efferd/components/auth-divider
     license: MIT · category: sign-in
     A horizontal divider with centered label text, typically used to separate social login buttons from an email form on authentication screens. -->

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
components/auth-divider.tsx
import type React from "react";

export function AuthDivider({
	children,
	...props
}: React.ComponentProps<"div">) {
	return (
		<div className="relative flex w-full items-center" {...props}>
			<div className="w-full border-t" />
			<div className="flex w-max justify-center text-nowrap px-2 text-muted-foreground text-xs">
				{children}
			</div>
			<div className="w-full border-t" />
		</div>
	);
}

demo.tsx
import { AuthDivider } from "@/components/ui/auth-divider";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";

export default function Default() {
  return (
    <div className="flex min-h-[420px] w-full items-center justify-center bg-background p-6">
      <div className="w-full max-w-sm space-y-6 rounded-xl border bg-card p-6 text-card-foreground shadow-sm">
        <div className="space-y-1 text-center">
          <h2 className="font-semibold text-xl tracking-tight">Welcome back</h2>
          <p className="text-muted-foreground text-sm">
            Sign in to continue to your account
          </p>
        </div>

        <div className="space-y-3">
          <Button variant="outline" className="w-full">
            Continue with Google
          </Button>
          <Button variant="outline" className="w-full">
            Continue with GitHub
          </Button>
        </div>

        <AuthDivider>OR CONTINUE WITH EMAIL</AuthDivider>

        <div className="space-y-3">
          <Input type="email" placeholder="name@example.com" />
          <Input type="password" placeholder="Password" />
          <Button className="w-full">Sign in</Button>
        </div>
      </div>
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button input
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
