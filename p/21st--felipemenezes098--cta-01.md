<!-- Call to Action · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/cta-01
     license: agpl-3.0 · category: hero
     A centered call-to-action section with a balanced title, description, and optional button. -->

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
components/ui/cta-01.tsx
import Balancer from 'react-wrap-balancer'

import type { CtaProps } from '../../shared/cta'
import { Cta } from '../../shared/cta'

export interface Cta01Props {
  title: string
  description: string
  cta?: CtaProps
}

export function Cta01({ title, description, cta }: Readonly<Cta01Props>) {
  return (
    <div className="bg-muted/50 flex min-h-120 items-center justify-center rounded-xl p-5">
      <div className="flex flex-col items-center space-y-4 self-center">
        <div className="space-y-2">
          {title && (
            <h2 className="text-center text-2xl font-bold md:max-w-200">
              <Balancer balance={0.5}>{title}</Balancer>
            </h2>
          )}
          {description && (
            <p className="text-muted-foreground text-center whitespace-pre-line md:max-w-200">
              <Balancer balance={0.5}>{description}</Balancer>
            </p>
          )}
        </div>
        {cta && <Cta cta={cta} />}
      </div>
    </div>
  )
}

components/ui/cta-01-example.tsx
import { Cta01, type Cta01Props } from './cta-01'

export const values = {
  title: 'Simple & Elegant',
  description: 'Display content in a minimal and visually appealing way.',
  cta: {
    ctaEnabled: true,
    text: 'Click here',
    link: '',
    variant: 'default',
  },
} satisfies Cta01Props

export function Cta01Example() {
  return (
    <Cta01
      title={values.title}
      description={values.description}
      cta={values.cta}
    />
  )
}

demo.tsx
import { Cta01 } from '@/components/ui/cta-01'

export default function Cta01Example() {
  return (
    <Cta01
      title="Simple & Elegant"
      description="Display content in a minimal and visually appealing way."
      cta={{
        ctaEnabled: true,
        text: 'Click here',
        link: '',
        variant: 'default',
      }}
    />
  )
}
```

Install NPM dependencies:
```bash
npm install react-wrap-balancer
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button cta
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
