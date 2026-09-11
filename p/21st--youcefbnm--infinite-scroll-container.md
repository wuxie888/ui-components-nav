<!-- Infinite scroll container · @youcefbnm · https://21st.dev/@youcefbnm/components/infinite-scroll-container
     license: MIT · category: scroll-area
     to used to manage large list and to automatically trigger next request when reaching the bottom of page
#### Props
- `items`: Array of data
- `isPending`: Boolean for loading state
- `itemsCount`: Total number of items available on the server ("COUNT" aggregation function in sql)
- `loadMore`: Function to call when more items need to be loaded
- `className`: Optional CSS class names
- `children`: React children elements -->

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
components/ui/index.tsx
'use client';
import { Spinner } from '@/components/systaliko-ui/shadcn/spinner';
import {
  AnimatePresence,
  motion,
  useInView,
  UseInViewOptions,
} from 'motion/react';
import React from 'react';

interface InfiniteScrollProps extends React.ComponentPropsWithRef<'div'> {
  isPending: boolean;
  currentItemsLength: number;
  allItemsCount: number | null | undefined;
  loadMore: () => void;
}
interface InfiniteScrollCellProps extends React.ComponentPropsWithRef<'div'> {
  skelton?: React.ReactNode;
  amount?: UseInViewOptions['amount'];
}

export function InfiniteScrollCell({
  skelton,
  amount,
  children,
  ...props
}: InfiniteScrollCellProps) {
  const ref = React.useRef<HTMLDivElement>(null);
  const isInView = useInView(ref, {
    once: true,
    amount,
  });

  return (
    <div ref={ref} {...props}>
      <AnimatePresence mode="wait">
        {!isInView ? (
          <motion.div
            key="skeleton"
            initial={{ opacity: 1 }}
            exit={{ opacity: 0 }}
          >
            {skelton}
          </motion.div>
        ) : (
          <motion.div
            key="content"
            initial={{ opacity: 0, y: 5 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{
              ease: 'easeOut',
            }}
          >
            {children}
          </motion.div>
        )}
      </AnimatePresence>
    </div>
  );
}

export function InfiniteScroll({
  currentItemsLength,
  isPending,
  allItemsCount,
  loadMore,
  children,
  ...props
}: InfiniteScrollProps) {
  const ref = React.useRef<HTMLDivElement | null>(null);
  const allLoaded = currentItemsLength === allItemsCount;
  const hasMore = !allLoaded && currentItemsLength > 0;

  React.useEffect(() => {
    const { current } = ref;

    if (isPending || allLoaded || !current) {
      return;
    }

    const observer = new IntersectionObserver(
      (entries) => {
        if (entries[0].isIntersecting && !isPending && !allLoaded) {
          loadMore();
        }
      },
      { rootMargin: '0px 0px', threshold: 0 },
    );

    observer.observe(current);

    return () => {
      observer.disconnect();
    };
  }, [isPending, allLoaded, currentItemsLength, loadMore]);

  return (
    <div {...props}>
      {children}
      {isPending && hasMore && (
        <div className="flex justify-center py-4">
          <Spinner />
        </div>
      )}
      {hasMore && <div ref={ref} />}
    </div>
  );
}

demo.tsx
"use client"

import * as React from "react"


import {InfiniteScrollContainer,InifniteScrollContainerCell} from "@/components/ui/infinite-scroll-container"
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from "@/components/ui/card"
interface Post {
  id: number
  userId: number
  title: string
  body: string
}

const BASE_URL = "https://jsonplaceholder.typicode.com/posts"
const LIMIT = 10

export function InfiniteScrollContainerDemo() {
    const [posts, setPosts] = React.useState<Post[]>([])
  const [page, setPage] = React.useState<number>(0)
  const [totalCount, setTotalCount] = React.useState<number | null>()
  const [isLoading, setIsLoading] = React.useState<boolean>(false)

  async function fetchData() {
    setIsLoading(true)
    const start = page * LIMIT
    try {
      const response = await fetch(
        `${BASE_URL}?_start=${start}&_limit=${LIMIT}`
      )
      const totalItems = response.headers.get("x-total-count")
      const data = await response.json()

      setTotalCount(Number(totalItems))
      setPosts((prevPosts) => [...prevPosts, ...data])
      setPage((prevPage) => prevPage + 1)
    } catch (error) {
      console.error("Error fetching data:", error)
    } finally {
      setIsLoading(false)
    }
  }

  React.useEffect(() => {
    fetchData()
  }, [])


  return (
    <InfiniteScrollContainer
      items={posts}
      isPending={isLoading}
      itemsCount={totalCount}
      loadMore={fetchData}
      className="container mx-auto grid grid-cols-[repeat(auto-fill,minmax(220px,1fr))] gap-2 p-12"
    >
      {posts.map((post, index) => (
        <InifniteScrollContainerCell isPending={isLoading} key={`${post.id}-${index}`}>
          <Card>
            <CardHeader>
              <CardTitle className="text-muted">#{post.id}</CardTitle>
              <CardDescription>{post.title}</CardDescription>
            </CardHeader>

            <CardContent className="text-sm text-foreground">
              <p>{post.body}</p>
            </CardContent>
          </Card>
        </InifniteScrollContainerCell>
      ))}
    </InfiniteScrollContainer>
  )
}
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add card skeleton skelton spinner
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
