<!-- Inline Anchored Comment Thread · @felipemenezes098 · https://21st.dev/@felipemenezes098/components/comment-thread-1
     license: no-license · category: avatar
     A comment thread card anchored to a quoted text selection, with threaded replies, a reply composer, and resolve/reopen actions. -->

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
components/ui/comment-thread-1.tsx
'use client'

import { useState } from 'react'
import { Check, MessageSquare } from 'lucide-react'

import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar'
import { Button } from '@/components/ui/button'
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card'
import { Textarea } from '@/components/ui/textarea'

type Reply = {
  id: string
  name: string
  avatar: string
  time: string
  text: string
}

const REPLIES: Reply[] = [
  {
    id: 'ada',
    name: 'Ada Lovelace',
    avatar:
      'https://images.unsplash.com/photo-1494790108377-be9c29b29330?w=128&h=128&fit=crop&crop=faces',
    time: '2h',
    text: 'Should this number be Q3 or Q4? The chart above says Q4.',
  },
  {
    id: 'grace',
    name: 'Grace Hopper',
    avatar:
      'https://images.unsplash.com/photo-1438761681033-6461ffad8d80?w=128&h=128&fit=crop&crop=faces',
    time: '1h',
    text: 'Good catch, it is Q4. Fixing now.',
  },
]

export function CommentThread1() {
  const [replies, setReplies] = useState<Reply[]>(REPLIES)
  const [draft, setDraft] = useState('')
  const [resolved, setResolved] = useState(false)

  const send = () => {
    const text = draft.trim()
    if (!text) return
    setReplies((prev) => [
      ...prev,
      { id: `me-${prev.length}`, name: 'You', avatar: '', time: 'now', text },
    ])
    setDraft('')
  }

  return (
    <Card>
      <CardHeader>
        <CardTitle className="flex items-center gap-2">
          <MessageSquare className="size-4" />
          Comment
        </CardTitle>
      </CardHeader>
      <CardContent className="flex flex-col gap-4">
        <p className="border-primary bg-primary/5 border-l-2 py-1 pl-3 text-sm italic">
          &ldquo;revenue grew 38% year over year&rdquo;
        </p>

        {resolved ? (
          <div className="text-muted-foreground flex items-center gap-2 rounded-lg border border-dashed p-3 text-sm">
            <Check className="size-4" />
            Thread resolved
            <Button
              variant="link"
              size="sm"
              className="ml-auto h-auto p-0"
              onClick={() => setResolved(false)}
            >
              Reopen
            </Button>
          </div>
        ) : (
          <>
            <div className="flex flex-col gap-4">
              {replies.map((reply) => (
                <div key={reply.id} className="flex gap-3">
                  <Avatar className="size-7">
                    <AvatarImage src={reply.avatar} alt={reply.name} />
                    <AvatarFallback className="bg-muted text-xs">
                      {reply.name
                        .split(' ')
                        .map((n) => n[0])
                        .join('')}
                    </AvatarFallback>
                  </Avatar>
                  <div className="flex flex-1 flex-col gap-0.5">
                    <div className="flex items-baseline gap-2">
                      <span className="text-sm font-medium">{reply.name}</span>
                      <span className="text-muted-foreground text-xs">
                        {reply.time}
                      </span>
                    </div>
                    <p className="text-sm">{reply.text}</p>
                  </div>
                </div>
              ))}
            </div>

            <Textarea
              placeholder="Reply..."
              value={draft}
              onChange={(e) => setDraft(e.target.value)}
              className="min-h-16 resize-none"
            />
            <div className="flex justify-end gap-2">
              <Button
                variant="ghost"
                size="sm"
                onClick={() => setResolved(true)}
              >
                <Check className="size-4" />
                Resolve
              </Button>
              <Button size="sm" onClick={send} disabled={!draft.trim()}>
                Reply
              </Button>
            </div>
          </>
        )}
      </CardContent>
    </Card>
  )
}

demo.tsx
import CommentThread1 from "@/components/ui/comment-thread-1";

export default function Default() {
  return (
    <div className="bg-background flex min-h-svh w-full items-center justify-center p-6">
      <div className="w-full max-w-md">
        <CommentThread1 />
      </div>
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
npx shadcn@latest add avatar button card textarea
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
