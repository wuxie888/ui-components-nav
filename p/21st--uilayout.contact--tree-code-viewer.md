<!-- Tree Code Viewer · @uilayout.contact · https://21st.dev/@uilayout.contact/components/tree-code-viewer
     license: MIT · category: file-tree
     Interactive file-tree viewer with an expandable folder tree and a synced, syntax-highlighted multi-file code panel for docs and code examples. -->

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
components/ui/tree-view-code.tsx
'use client';

import {
  TreeExpander,
  TreeIcon,
  TreeLabel,
  TreeNode,
  TreeNodeContent,
  TreeNodeTrigger,
  TreeProvider,
  TreeView,
} from '@/components/ui/tree';
import { type TreeNodeData, buildSimpleTree, getFileIcon } from '@/lib/tree-structure';
import { useState } from 'react';
import { ClientPreCode } from '../website/code-components/client-pre-code';

export function TreeCodeViewer({
  files,
}: {
  files: {
    id: string;
    fileName: string;
    virtualPath: string[];
    ext: string;
    raw: string;
    html: string;
  }[];
}) {
  const tree = buildSimpleTree(files);
  console.log('tree', tree);
  const [selectedId, setSelectedId] = useState(files[0]?.id);
  const fileMap = Object.fromEntries(files.map((f) => [f.id, f]));
  const selectedFile = selectedId ? fileMap[selectedId] : null;

  return (
    <div className='grid grid-cols-[250px_1fr] border dark:border-neutral-800 bg-neutral-50 dark:bg-neutral-950 overflow-hidden'>
      {/* LEFT */}
      <TreeProvider
        selectable
        multiSelect={false}
        defaultExpandedIds={[tree[0].name, 'ui', 'lib']}
        onSelectionChange={(ids) => {
          if (ids[0]) setSelectedId(ids[0]);
        }}
        className='border-r dark:border-neutral-800 bg-neutral-50 dark:bg-neutral-900'
      >
        <TreeView>
          <RenderTree nodes={tree} />
        </TreeView>
      </TreeProvider>

      {/* RIGHT */}
      <div className='min-w-0 p-2'>
        {selectedFile ? (
          <ClientPreCode html={selectedFile.html} raw={selectedFile.raw} />
        ) : (
          <div className='p-6 text-sm text-muted-foreground'>Select a file</div>
        )}
      </div>
    </div>
  );
}

function RenderTree({ nodes, level = 0 }: { nodes: TreeNodeData[]; level?: number }) {
  return (
    <>
      {nodes.map((node, index) => {
        const isLast = index === nodes.length - 1;

        // 📁 FOLDER
        if (node.type === 'folder') {
          return (
            <TreeNode key={node.name} nodeId={node.name} level={level} isLast={isLast} isFolder>
              <TreeNodeTrigger>
                <TreeExpander hasChildren />
                <TreeIcon icon={getFileIcon('folder')} />
                <TreeLabel>{node.name}</TreeLabel>
              </TreeNodeTrigger>

              <TreeNodeContent hasChildren>
                <RenderTree nodes={node.children} level={level + 1} />
              </TreeNodeContent>
            </TreeNode>
          );
        }

        // 📄 FILE
        return (
          <TreeNode key={node.id} nodeId={node.id} level={level} isLast={isLast}>
            <TreeNodeTrigger>
              <TreeExpander />
              <TreeIcon icon={getFileIcon('file', node.ext, node.name)} />
              <TreeLabel>{node.name}</TreeLabel>
            </TreeNodeTrigger>
          </TreeNode>
        );
      })}
    </>
  );
}

components/ui/client-pre-code.tsx
'use client';

import { cn } from '@/lib/utils';
import { CopyButton } from './copy-button';

export function ClientPreCode({
  html,
  raw,
  className,
}: {
  html: string;
  raw: string;
  className?: string;
}) {
  return (
    <div className={cn('relative', className)}>
      <CopyButton code={raw} classname='right-2 top-2 bg-white dark:bg-neutral-800' />

      <div
        className='not-prose max-h-[550px] overflow-x-hidden rounded-md text-sm border dark:border-neutral-800'
        dangerouslySetInnerHTML={{ __html: html }}
      />
    </div>
  );
}

components/ui/copy-button.tsx
'use client';

import { cn } from '@/lib/utils';
import { Check, CheckCheck, Copy } from 'lucide-react';
import { useState } from 'react';

export function CopyButton({ code, classname }: { code: string; classname?: string }) {
  const [hasCheckIcon, setHasCheckIcon] = useState(false);

  const onCopy = () => {
    navigator.clipboard.writeText(code);
    setHasCheckIcon(true);

    setTimeout(() => {
      setHasCheckIcon(false);
    }, 1000);
  };

  return (
    <>
      <div
        className={cn(
          'absolute right-2 top-2 cursor-pointer dark:hover:shadow-[0px_1px_10px_5px_#3f7ef3] hover:shadow-[0px_1px_10px_5px_#9abaf7] dark:hover:border-blue-500 hover:border-blue-300  dark:bg-zinc-800 backdrop-blur-2xl bg-white rounded-md border-2',
          classname
        )}
        onClick={onCopy}
      >
        <div
          className={` inset-0 transform transition-all duration-300  w-9 h-8 grid place-content-center  ${
            hasCheckIcon ? 'scale-0 opacity-0' : 'scale-100 opacity-100'
          }`}
        >
          <Copy className='h-4 w-4 text-foreground/80' />
        </div>
        <div
          className={`absolute inset-0 transform transition-all duration-300 w-8 h-8 grid place-content-center  ${
            hasCheckIcon ? 'scale-100 opacity-100' : 'scale-0 opacity-0'
          }`}
        >
          <CheckCheck className='h-4 w-4 text-foreground/80' />
        </div>
      </div>
    </>
  );
}

demo.tsx
"use client";

import TreeCodeViewer from "@/components/ui/tree-code-viewer";

function highlight(code: string) {
  const escaped = code
    .replace(/&/g, "&amp;")
    .replace(/</g, "&lt;")
    .replace(/>/g, "&gt;");
  const tokens = escaped
    .replace(/('[^']*'|"[^"]*")/g, '<span style="color:#a5d6ff">$1</span>')
    .replace(
      /\b(import|from|export|function|const|let|return|type|interface|default)\b/g,
      '<span style="color:#ff7b72">$1</span>',
    );
  return `<pre style="background-color:#0d1117;color:#e6edf3;margin:0;padding:16px;overflow:auto;font-size:13px;line-height:1.6;font-family:ui-monospace,SFMono-Regular,Menlo,monospace"><code>${tokens}</code></pre>`;
}

const raw = {
  button: `import { cn } from '@/lib/utils';\n\ntype ButtonProps = React.ButtonHTMLAttributes<HTMLButtonElement>;\n\nexport function Button({ className, ...props }: ButtonProps) {\n  return (\n    <button\n      className={cn('rounded-md bg-primary px-4 py-2 text-primary-foreground', className)}\n      {...props}\n    />\n  );\n}`,
  input: `import { cn } from '@/lib/utils';\n\nexport function Input(props: React.InputHTMLAttributes<HTMLInputElement>) {\n  return (\n    <input\n      className={cn('h-9 w-full rounded-md border px-3 text-sm')}\n      {...props}\n    />\n  );\n}`,
  page: `import { Button } from '@/components/ui/button';\n\nexport default function Page() {\n  return (\n    <main className='p-6'>\n      <Button>Click me</Button>\n    </main>\n  );\n}`,
  utils: `import { clsx, type ClassValue } from 'clsx';\nimport { twMerge } from 'tailwind-merge';\n\nexport function cn(...inputs: ClassValue[]) {\n  return twMerge(clsx(inputs));\n}`,
  pkg: `{\n  "name": "my-app",\n  "version": "1.0.0",\n  "dependencies": {\n    "react": "^19.0.0",\n    "motion": "^12.0.0"\n  }\n}`,
};

const files = [
  {
    id: "button",
    fileName: "button.tsx",
    virtualPath: ["components", "ui"],
    ext: "tsx",
    raw: raw.button,
    html: highlight(raw.button),
  },
  {
    id: "input",
    fileName: "input.tsx",
    virtualPath: ["components", "ui"],
    ext: "tsx",
    raw: raw.input,
    html: highlight(raw.input),
  },
  {
    id: "page",
    fileName: "page.tsx",
    virtualPath: ["components"],
    ext: "tsx",
    raw: raw.page,
    html: highlight(raw.page),
  },
  {
    id: "utils",
    fileName: "utils.ts",
    virtualPath: ["lib"],
    ext: "ts",
    raw: raw.utils,
    html: highlight(raw.utils),
  },
  {
    id: "pkg",
    fileName: "package.json",
    virtualPath: [],
    ext: "json",
    raw: raw.pkg,
    html: highlight(raw.pkg),
  },
];

export default function TreeCodeViewerDemo() {
  return (
    <div className="mx-auto w-full max-w-4xl p-4">
      <TreeCodeViewer files={files} />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install shiki
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
