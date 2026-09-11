<!-- Blind Pull Toggle · @aicanvas · https://21st.dev/@aicanvas/components/blind-pull-toggle
     license: MIT · category: toggle
     A dark/light mode toggle styled as a window-blind pull cord, where clicking swings a cord to flip venetian-blind slats revealing the sun or moon icon. -->

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
components/aicanvas/blind-pull-toggle.tsx
const ACCOUNT_NOTICE =
  "Almost there! \"Blind Pull Toggle\" is free with an AI Canvas account (free, unlimited installs). Sign up at https://aicanvas.me/account/sign-up, then copy your personal install command from the component page. Already have an account? Sign in and copy it from https://aicanvas.me/account/settings"

export default function AccountRequired() {
  return (
    <div style={{ padding: 24, fontFamily: 'ui-monospace, monospace', fontSize: 13, lineHeight: 1.6, color: '#9aa3af' }}>
      {ACCOUNT_NOTICE}
    </div>
  )
}

demo.tsx
import BlindPullToggle from "@/components/ui/blind-pull-toggle";

export default function BlindPullToggleDemo() {
  return (
    <div className="flex h-screen w-full items-center justify-center">
      <BlindPullToggle />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @phosphor-icons/react framer-motion
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
