<!-- YouTube Embed · @inference-sh · https://21st.dev/@inference-sh/components/youtube-embed
     license: no-license · category: video
     Responsive YouTube video embed with an aspect-ratio container and a loading spinner shown until the iframe finishes loading. -->

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
components/infsh/youtube-embed.tsx
import { cn } from '@/lib/utils'
import { useState } from 'react'

interface YouTubeEmbedProps {
  videoId: string
  title?: string
  className?: string
}

export function YouTubeEmbed({ videoId, title, className }: YouTubeEmbedProps) {
  const [isLoading, setIsLoading] = useState(true)

  return (
    <div className={cn(
      'flex flex-col space-y-4 w-full h-full relative rounded-xl overflow-hidden border border-border',
      className
    )}>
      {isLoading && (
        <div className="absolute inset-0 flex items-center justify-center bg-muted">
          <div className="w-8 h-8 border-2 border-pink-400 border-t-transparent rounded-full animate-spin" />
        </div>
      )}
      <iframe
        src={`https://www.youtube.com/embed/${videoId}`}
        title={title || 'YouTube video player'}
        frameBorder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
        referrerPolicy="strict-origin-when-cross-origin"
        allowFullScreen
        loading="lazy"
        onLoad={() => setIsLoading(false)}
        className={cn(
          'w-full aspect-video h-full bg-transparent transition-opacity duration-300',
          isLoading ? 'opacity-0' : 'opacity-100'
        )}
      />
    </div>
  )
}

demo.tsx
import { YouTubeEmbed } from '@/components/ui/youtube-embed'

export default function YouTubeEmbedDemo() {
  return (
    <div className="w-full max-w-2xl mx-auto p-4">
      <YouTubeEmbed videoId="dQw4w9WgXcQ" title="Example YouTube video" />
    </div>
  )
}
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
