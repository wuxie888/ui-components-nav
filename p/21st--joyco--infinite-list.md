<!-- Infinite List · @joyco · https://21st.dev/@joyco/components/infinite-list
     license: no-license · category: scroll-area
     A bias-based infinite scroll wrapper and hook that manages pagination and masks child items for progressive "load more" lists. -->

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
components/infinite-list.tsx
'use client'

import { useState, useMemo, useCallback, Children } from 'react'

export function useInfiniteList({
  pageSize,
  initialItems,
  initialPage = 1,
}: {
  pageSize: number
  initialItems?: unknown[]
  initialPage?: number
}) {
  const [requestedPage, setRequestedPage] = useState(initialPage)

  const bias = useMemo(() => {
    const bias = Math.max(0, (initialItems?.length ?? 0) - pageSize)
    if (bias !== pageSize && bias !== 0) {
      console.warn(
        `[useInfiniteList] bias (${bias}) differs from pageSize (${pageSize}). ` +
          `This will cause instantly displayed items to be appended with more items when the next page loads.`
      )
    }
    return bias
  }, [initialItems?.length, pageSize])

  const displayLimit = requestedPage * pageSize

  const nextPage = useCallback(() => {
    setRequestedPage((current) => current + 1)
  }, [])

  return {
    offset: requestedPage * pageSize + bias,
    displayLimit,
    pageSize,
    nextPage,
  }
}

export function MaskedList({
  children,
  displayLimit,
}: ReturnType<typeof useInfiniteList> & { children: React.ReactNode }) {
  const childArray = Children.toArray(children)
  const visibleChildren = childArray.slice(0, displayLimit)
  return <>{visibleChildren}</>
}

demo.tsx
'use client'

import { MaskedList, useInfiniteList } from '@/components/ui/infinite-list'
import Image from 'next/image'
import { useEffect, useState, useTransition } from 'react'
import { Button } from '@/components/ui/button'
import { Card, CardFooter } from '@/components/ui/card'

type Pokemon = { name: string; url: string; image: string }

async function fetchPokemon(offset: number, limit: number): Promise<Pokemon[]> {
  const res = await fetch(
    `https://pokeapi.co/api/v2/pokemon?offset=${offset}&limit=${limit}`
  ).then((r) => r.json())
  return Promise.all(
    res.results.map(async (p: { url: string }) => {
      const data = await fetch(p.url).then((r) => r.json())
      return { name: data.name, url: p.url, image: data.sprites.front_default }
    })
  )
}

export default function InfiniteListDemo() {
  const pageSize = 12
  const [pokemon, setPokemon] = useState<Pokemon[]>([])
  const [isPending, startTransition] = useTransition()
  const list = useInfiniteList({ pageSize, initialItems: pokemon })

  useEffect(() => {
    fetchPokemon(0, pageSize * 2).then(setPokemon)
  }, [])

  const loadMore = () => {
    list.nextPage()
    startTransition(async () => {
      const newPokemon = await fetchPokemon(list.offset, pageSize)
      setPokemon((prev) => [...prev, ...newPokemon])
    })
  }

  return (
    <div className="relative h-[500px] w-full overflow-auto">
      <div className="bg-background/90 border-border sticky top-4 right-8 z-10 ml-auto w-fit rounded-lg border px-3 py-2 font-mono text-xs">
        DISPLAYED: {Math.min(list.displayLimit, pokemon.length)} | LOADED:{' '}
        {pokemon.length}
      </div>
      <div className="flex flex-col gap-6 p-6">
        <h2 className="text-2xl font-semibold">Pokemon Infinite List</h2>
        <div className="mb-6 grid grid-cols-2 gap-3 md:grid-cols-3">
          <MaskedList {...list}>
            {pokemon.map((p) => (
              <Card key={p.url} className="gap-0 overflow-hidden p-0">
                <div className="bg-muted aspect-square">
                  <Image
                    src={p.image}
                    alt={p.name}
                    width={500}
                    height={500}
                    className="h-full w-full object-cover [image-rendering:pixelated]"
                    unoptimized
                  />
                </div>
                <CardFooter className="p-4">
                  <h3 className="text-base font-semibold capitalize">
                    {p.name}
                  </h3>
                </CardFooter>
              </Card>
            ))}
          </MaskedList>
        </div>

        <Button
          onClick={loadMore}
          disabled={isPending}
          variant="outline"
          className="self-center shadow-none transition-none"
        >
          {isPending ? 'Loading...' : 'Load More'}
        </Button>
      </div>
    </div>
  )
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button card
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
