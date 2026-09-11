<!-- KBD in Button · @shadcnspace · https://21st.dev/@shadcnspace/components/kbd-01
     license: no-license · category: kbd
     Buttons with inline keyboard shortcut hints and a Mac/Windows platform switch. -->

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
components/shadcn-space/kbd/kbd-01.tsx
'use client'

import { useState } from 'react'
import { PrinterIcon, SaveIcon, Share2Icon } from 'lucide-react'
import { Button } from '@/components/ui/button'
import { Kbd, KbdGroup } from '@/components/ui/kbd'
import { Switch } from '@/components/ui/switch'

type Platform = 'mac' | 'win'

const KbdInButtonDemo = () => {
  const [platform, setPlatform] = useState<Platform>('mac')
  const isMac = platform === 'mac'

  return (
    <div className="flex flex-col items-center gap-4">
      {/* Platform switch */}
      <div className="flex items-center gap-2 text-sm">
        <span className="text-muted-foreground">MacOS</span>
        <Switch
          checked={platform === 'win'}
          onCheckedChange={(checked) => setPlatform(checked ? 'win' : 'mac')}
        />
        <span className="text-muted-foreground">Windows</span>
      </div>

      {/* Buttons */}
      <div className="flex flex-wrap justify-center gap-3">
        <Button variant="outline" className="hover:bg-muted/40">
          <SaveIcon aria-hidden="true" />
          Save
          <KbdGroup className="-me-1">
            <Kbd>{isMac ? '⌘' : 'Ctrl'}</Kbd>
            <Kbd>S</Kbd>
          </KbdGroup>
        </Button>

        <Button variant="outline" className="hover:bg-muted/40">
          <PrinterIcon aria-hidden="true" />
          Print
          <KbdGroup className="-me-1">
            <Kbd>{isMac ? '⌘' : 'Ctrl'}</Kbd>
            <Kbd>P</Kbd>
          </KbdGroup>
        </Button>

        <Button variant="outline" className="hover:bg-muted/40">
          <Share2Icon aria-hidden="true" />
          Share
          <KbdGroup className="-me-1">
            <Kbd>{isMac ? '⌘' : 'Ctrl'}</Kbd>
            <Kbd>{isMac ? '⇧' : 'Shift'}</Kbd>
            <Kbd>S</Kbd>
          </KbdGroup>
        </Button>
      </div>
    </div>
  )
}

export default KbdInButtonDemo

demo.tsx
import KbdInButton from "@/components/ui/kbd-01";

export default function Default() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center p-6">
      <KbdInButton />
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
npx shadcn@latest add button kbd switch
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
