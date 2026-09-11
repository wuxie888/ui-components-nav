<!-- File Trigger · @intentui · https://21st.dev/@intentui/components/file-trigger
     license: unspecified · category: upload-download
     Here is FIle Trigger component -->

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
components/ui/file-trigger.tsx
'use client'

import { CameraIcon, FolderIcon, PaperClipIcon } from '@heroicons/react/24/outline'
import {
  FileTrigger as FileTriggerPrimitive,
  type FileTriggerProps as FileTriggerPrimitiveProps,
} from 'react-aria-components/FileTrigger'
import type { VariantProps } from 'tailwind-variants'
import { Button, type buttonStyles } from './button'
import { Loader } from './loader'

export interface FileTriggerProps
  extends FileTriggerPrimitiveProps, VariantProps<typeof buttonStyles> {
  isDisabled?: boolean
  isPending?: boolean
  ref?: React.RefObject<HTMLInputElement>
  className?: string
}

export function FileTrigger({
  intent = 'outline',
  size = 'md',
  isCircle = false,
  ref,
  className,
  ...props
}: FileTriggerProps) {
  return (
    <FileTriggerPrimitive ref={ref} {...props}>
      <Button
        className={className}
        isDisabled={props.isDisabled}
        intent={intent}
        size={size}
        isCircle={isCircle}
      >
        {!props.isPending ? (
          props.defaultCamera ? (
            <CameraIcon />
          ) : props.acceptDirectory ? (
            <FolderIcon />
          ) : (
            <PaperClipIcon />
          )
        ) : (
          <Loader />
        )}
        {props.children ? (
          props.children
        ) : (
          <>
            {props.allowsMultiple
              ? 'Browse a files'
              : props.acceptDirectory
                ? 'Browse'
                : 'Browse a file'}
            ...
          </>
        )}
      </Button>
    </FileTriggerPrimitive>
  )
}

demo.tsx
"use client"

import { FileTrigger } from "@/components/ui/file-trigger"

export default function Component() {
  return <FileTrigger />
}
```

Install NPM dependencies:
```bash
npm install @heroicons/react @intentui/icons react-aria-components tailwind-variants
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button button-1 loader
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
