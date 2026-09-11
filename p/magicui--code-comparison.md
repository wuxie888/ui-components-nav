<!-- Code Comparison · Magic UI · https://magicui.design/docs/components/code-comparison
     license: MIT · category: image
     A component which compares two code snippets. -->

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
components/ui/code-comparison.tsx
"use client"

import { useEffect, useMemo, useState } from "react"
import {
  transformerNotationDiff,
  transformerNotationFocus,
} from "@shikijs/transformers"
import { FileIcon } from "lucide-react"
import { useTheme } from "next-themes"

import { cn } from "@/lib/utils"

interface CodeComparisonProps {
  beforeCode: string
  afterCode: string
  language: string
  filename: string
  lightTheme: string
  darkTheme: string
  highlightColor?: string
}

export function CodeComparison({
  beforeCode,
  afterCode,
  language,
  filename,
  lightTheme,
  darkTheme,
  highlightColor = "#ff3333",
}: CodeComparisonProps) {
  const { theme, systemTheme } = useTheme()
  const [highlightedBefore, setHighlightedBefore] = useState("")
  const [highlightedAfter, setHighlightedAfter] = useState("")
  const [hasLeftFocus, setHasLeftFocus] = useState(false)
  const [hasRightFocus, setHasRightFocus] = useState(false)

  const selectedTheme = useMemo(() => {
    const currentTheme = theme === "system" ? systemTheme : theme
    return currentTheme === "dark" ? darkTheme : lightTheme
  }, [theme, systemTheme, darkTheme, lightTheme])

  useEffect(() => {
    if (highlightedBefore || highlightedAfter) {
      setHasLeftFocus(highlightedBefore.includes('class="line focused"'))
      setHasRightFocus(highlightedAfter.includes('class="line focused"'))
    }
  }, [highlightedBefore, highlightedAfter])

  useEffect(() => {
    async function highlightCode() {
      try {
        const { codeToHtml } = await import("shiki")
        const { transformerNotationHighlight } =
          await import("@shikijs/transformers")

        const before = await codeToHtml(beforeCode, {
          lang: language,
          theme: selectedTheme,
          transformers: [
            transformerNotationHighlight({ matchAlgorithm: "v3" }),
            transformerNotationDiff({ matchAlgorithm: "v3" }),
            transformerNotationFocus({ matchAlgorithm: "v3" }),
          ],
        })
        const after = await codeToHtml(afterCode, {
          lang: language,
          theme: selectedTheme,
          transformers: [
            transformerNotationHighlight({ matchAlgorithm: "v3" }),
            transformerNotationDiff({ matchAlgorithm: "v3" }),
            transformerNotationFocus({ matchAlgorithm: "v3" }),
          ],
        })
        setHighlightedBefore(before)
        setHighlightedAfter(after)
      } catch (error) {
        console.error("Error highlighting code:", error)
        setHighlightedBefore(`<pre>${beforeCode}</pre>`)
        setHighlightedAfter(`<pre>${afterCode}</pre>`)
      }
    }
    highlightCode()
  }, [beforeCode, afterCode, language, selectedTheme])

  const renderCode = (code: string, highlighted: string) => {
    if (highlighted) {
      return (
        <div
          style={{ "--highlight-color": highlightColor } as React.CSSProperties}
          className={cn(
            "bg-background h-full w-full overflow-auto font-mono text-xs",
            "[&>pre]:h-full [&>pre]:w-screen! [&>pre]:py-2",
            "[&>pre>code]:inline-block! [&>pre>code]:w-full!",
            "[&>pre>code>span]:inline-block! [&>pre>code>span]:w-full [&>pre>code>span]:px-4 [&>pre>code>span]:py-0.5",
            "[&>pre>code>.highlighted]:inline-block [&>pre>code>.highlighted]:w-full [&>pre>code>.highlighted]:bg-(--highlight-color)!",
            "group-hover/left:[&>pre>code>:not(.focused)]:opacity-100! group-hover/left:[&>pre>code>:not(.focused)]:blur-none!",
            "group-hover/right:[&>pre>code>:not(.focused)]:opacity-100! group-hover/right:[&>pre>code>:not(.focused)]:blur-none!",
            "[&>pre>code>.add]:bg-[rgba(16,185,129,.16)] [&>pre>code>.remove]:bg-[rgba(244,63,94,.16)]",
            "group-hover/left:[&>pre>code>:not(.focused)]:transition-all group-hover/left:[&>pre>code>:not(.focused)]:duration-300",
            "group-hover/right:[&>pre>code>:not(.focused)]:transition-all group-hover/right:[&>pre>code>:not(.focused)]:duration-300"
          )}
          dangerouslySetInnerHTML={{ __html: highlighted }}
        />
      )
    } else {
      return (
        <pre className="bg-background text-foreground h-full overflow-auto p-4 font-mono text-xs break-all">
          {code}
        </pre>
      )
    }
  }

  return (
    <div className="mx-auto w-full max-w-5xl">
      <div className="group border-border relative w-full overflow-hidden rounded-md border">
        <div className="relative grid md:grid-cols-2">
          <div
            className={cn(
              "leftside group/left border-primary/20 md:border-r",
              hasLeftFocus &&
                "[&>div>pre>code>:not(.focused)]:opacity-50! [&>div>pre>code>:not(.focused)]:blur-[0.095rem]!",
              "[&>div>pre>code>:not(.focused)]:transition-all [&>div>pre>code>:not(.focused)]:duration-300"
            )}
          >
            <div className="border-primary/20 bg-accent text-foreground flex items-center border-b p-2 text-sm">
              <FileIcon className="mr-2 h-4 w-4" />
              {filename}
              <span className="ml-auto hidden md:block">before</span>
            </div>
            {renderCode(beforeCode, highlightedBefore)}
          </div>
          <div
            className={cn(
              "rightside group/right border-primary/20 border-t md:border-t-0",
              hasRightFocus &&
                "[&>div>pre>code>:not(.focused)]:opacity-50! [&>div>pre>code>:not(.focused)]:blur-[0.095rem]!",
              "[&>div>pre>code>:not(.focused)]:transition-all [&>div>pre>code>:not(.focused)]:duration-300"
            )}
          >
            <div className="border-primary/20 bg-accent text-foreground flex items-center border-b p-2 text-sm">
              <FileIcon className="mr-2 h-4 w-4" />
              {filename}
              <span className="ml-auto hidden md:block">after</span>
            </div>
            {renderCode(afterCode, highlightedAfter)}
          </div>
        </div>
        <div className="border-primary/20 bg-accent text-foreground absolute top-1/2 left-1/2 hidden h-8 w-8 -translate-x-1/2 -translate-y-1/2 items-center justify-center rounded-md border text-xs md:flex">
          VS
        </div>
      </div>
    </div>
  )
}

demo.tsx
import { CodeComparison } from "@/registry/magicui/code-comparison"

const beforeCode = `import { NextRequest } from 'next/server';

export const middleware = async (req: NextRequest) => {
  let user = undefined;
  let team = undefined;
  const token = req.headers.get('token'); 

  if(req.nextUrl.pathname.startsWith('/auth')) {
    user = await getUserByToken(token);

    if(!user) {
      return NextResponse.redirect('/login');
    }
  }

  if(req.nextUrl.pathname.startsWith('/team')) {
    user = await getUserByToken(token);

    if(!user) {
      return NextResponse.redirect('/login');
    }

    const slug = req.nextUrl.query.slug;
    team = await getTeamBySlug(slug); // [!code highlight]

    if(!team) { // [!code highlight]
      return NextResponse.redirect('/'); // [!code highlight]
    } // [!code highlight]
  } // [!code highlight]

  return NextResponse.next(); // [!code highlight]
}

export const config = {
  matcher: ['/((?!_next/|_static|_vercel|[\\w-]+\\.\\w+).*)'], // [!code highlight]
};`

const afterCode = `import { createMiddleware, type MiddlewareFunctionProps } from '@app/(auth)/auth/_middleware';
import { auth } from '@/app/(auth)/auth/_middleware'; // [!code --]
import { auth } from '@/app/(auth)/auth/_middleware'; // [!code ++]
import { team } from '@/app/(team)/team/_middleware';

const middlewares = {
  '/auth{/:path?}': auth,
  '/team{/:slug?}': [ auth, team ],
};

export const middleware = createMiddleware(middlewares); // [!code focus]

export const config = {
  matcher: ['/((?!_next/|_static|_vercel|[\\w-]+\\.\\w+).*)'],
};`

export default function CodeComparisonDemo() {
  return (
    <CodeComparison
      beforeCode={beforeCode}
      afterCode={afterCode}
      language="typescript"
      filename="middleware.ts"
      lightTheme="github-light"
      darkTheme="github-dark"
      highlightColor="rgba(101, 117, 133, 0.16)"
    />
  )
}
```

Install NPM dependencies:
```bash
npm install next-themes shiki
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
