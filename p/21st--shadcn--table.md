<!-- Table · @shadcn · https://21st.dev/@shadcn/components/table
     license: MIT · category: table
     A responsive table component. -->

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
components/ui/table.tsx
"use client"

import * as React from "react"
import { cn } from "cn"

function Table({ className, ...props }: React.ComponentProps<"table">) {
  return (
    <div
      data-slot="table-container"
      className="relative w-full overflow-x-auto"
    >
      <table
        data-slot="table"
        className={cn("w-full caption-bottom text-sm", className)}
        {...props}
      />
    </div>
  )
}

function TableHeader({ className, ...props }: React.ComponentProps<"thead">) {
  return (
    <thead
      data-slot="table-header"
      className={cn("[&_tr]:border-b", className)}
      {...props}
    />
  )
}

function TableBody({ className, ...props }: React.ComponentProps<"tbody">) {
  return (
    <tbody
      data-slot="table-body"
      className={cn("[&_tr:last-child]:border-0", className)}
      {...props}
    />
  )
}

function TableFooter({ className, ...props }: React.ComponentProps<"tfoot">) {
  return (
    <tfoot
      data-slot="table-footer"
      className={cn(
        "border-t bg-muted/50 font-medium [&>tr]:last:border-b-0",
        className
      )}
      {...props}
    />
  )
}

function TableRow({ className, ...props }: React.ComponentProps<"tr">) {
  return (
    <tr
      data-slot="table-row"
      className={cn(
        "border-b transition-colors hover:bg-muted/50 has-aria-expanded:bg-muted/50 data-[state=selected]:bg-muted",
        className
      )}
      {...props}
    />
  )
}

function TableHead({ className, ...props }: React.ComponentProps<"th">) {
  return (
    <th
      data-slot="table-head"
      className={cn(
        "h-10 px-2 text-left align-middle font-medium whitespace-nowrap text-foreground [&:has([role=checkbox])]:pr-0 [&>[role=checkbox]]:translate-y-[2px]",
        className
      )}
      {...props}
    />
  )
}

function TableCell({ className, ...props }: React.ComponentProps<"td">) {
  return (
    <td
      data-slot="table-cell"
      className={cn(
        "p-2 align-middle whitespace-nowrap [&:has([role=checkbox])]:pr-0 [&>[role=checkbox]]:translate-y-[2px]",
        className
      )}
      {...props}
    />
  )
}

function TableCaption({
  className,
  ...props
}: React.ComponentProps<"caption">) {
  return (
    <caption
      data-slot="table-caption"
      className={cn("mt-4 text-sm text-muted-foreground", className)}
      {...props}
    />
  )
}

export {
  Table,
  TableHeader,
  TableBody,
  TableFooter,
  TableHead,
  TableRow,
  TableCell,
  TableCaption,
}

demo.tsx
"use client"

import { Badge } from "@/components/ui/badge"
import { Button } from "@/components/ui/button"
import { Input } from "@/components/ui/input"
import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "@/components/ui/table"
import { Pencil, Trash2 } from "lucide-react"
import { useMemo, useState } from "react"

type Bookmark = {
  id: number
  title: string
  url: string
  tags: string[]
  description: string
  createdAt: string
}

type SortColumn = keyof Bookmark

function Component() {
  const [bookmarks] = useState<Bookmark[]>([
    {
      id: 1,
      title: "Vercel",
      url: "https://vercel.com",
      tags: ["web", "deployment"],
      description:
        "Vercel is a cloud platform for static sites and serverless functions.",
      createdAt: "2023-05-01",
    },
    {
      id: 2,
      title: "Tailwind CSS",
      url: "https://tailwindcss.com",
      tags: ["css", "framework"],
      description:
        "Tailwind CSS is a utility-first CSS framework for rapidly building custom designs.",
      createdAt: "2023-04-15",
    },
    {
      id: 3,
      title: "React",
      url: "https://reactjs.org",
      tags: ["javascript", "library"],
      description:
        "React is a JavaScript library for building user interfaces.",
      createdAt: "2023-03-20",
    },
    {
      id: 4,
      title: "Next.js",
      url: "https://nextjs.org",
      tags: ["react", "framework"],
      description:
        "Next.js is a React framework that enables server-side rendering and more.",
      createdAt: "2023-02-10",
    },
    {
      id: 5,
      title: "Prisma",
      url: "https://www.prisma.io",
      tags: ["database", "orm"],
      description:
        "Prisma is an open-source database toolkit that includes an ORM.",
      createdAt: "2023-01-01",
    },
  ])
  const [searchTerm, setSearchTerm] = useState("")
  const [sortColumn, setSortColumn] = useState<SortColumn>("title")
  const [sortDirection, setSortDirection] = useState("asc")
  const filteredBookmarks = useMemo(() => {
    return bookmarks.filter((bookmark) =>
      bookmark.title.toLowerCase().includes(searchTerm.toLowerCase()),
    )
  }, [bookmarks, searchTerm])

  const sortedBookmarks = useMemo(() => {
    return filteredBookmarks.sort((a, b) => {
      if (a[sortColumn] < b[sortColumn]) return sortDirection === "asc" ? -1 : 1
      if (a[sortColumn] > b[sortColumn]) return sortDirection === "asc" ? 1 : -1
      return 0
    })
  }, [filteredBookmarks, sortColumn, sortDirection])

  const handleSort = (column: SortColumn) => {
    if (sortColumn === column) {
      setSortDirection(sortDirection === "asc" ? "desc" : "asc")
    } else {
      setSortColumn(column)
      setSortDirection("asc")
    }
  }
  return (
    <div className="mx-auto my-6 w-full max-w-6xl rounded border">
      <div className="flex flex-wrap items-center justify-between gap-4 border-b p-4 md:py-2">
        <h1 className="text-xl font-bold">Bookmarks</h1>
        <Input
          placeholder="Search bookmarks..."
          value={searchTerm}
          onChange={(e) => setSearchTerm(e.target.value)}
          className="md:w-96"
        />
      </div>
      <Table>
        <TableHeader>
          <TableRow>
            <TableHead
              className="cursor-pointer"
              onClick={() => handleSort("title")}
            >
              Title
              {sortColumn === "title" && (
                <span className="ml-1">
                  {sortDirection === "asc" ? "\u2191" : "\u2193"}
                </span>
              )}
            </TableHead>
            <TableHead
              className="cursor-pointer"
              onClick={() => handleSort("url")}
            >
              URL
              {sortColumn === "url" && (
                <span className="ml-1">
                  {sortDirection === "asc" ? "\u2191" : "\u2193"}
                </span>
              )}
            </TableHead>
            <TableHead
              className="cursor-pointer"
              onClick={() => handleSort("tags")}
            >
              Tags
              {sortColumn === "tags" && (
                <span className="ml-1">
                  {sortDirection === "asc" ? "\u2191" : "\u2193"}
                </span>
              )}
            </TableHead>
            <TableHead
              className="cursor-pointer"
              onClick={() => handleSort("description")}
            >
              Description
              {sortColumn === "description" && (
                <span className="ml-1">
                  {sortDirection === "asc" ? "\u2191" : "\u2193"}
                </span>
              )}
            </TableHead>
            <TableHead
              className="cursor-pointer"
              onClick={() => handleSort("createdAt")}
            >
              Created
              {sortColumn === "createdAt" && (
                <span className="ml-1">
                  {sortDirection === "asc" ? "\u2191" : "\u2193"}
                </span>
              )}
            </TableHead>
            <TableHead>Actions</TableHead>
          </TableRow>
        </TableHeader>
        <TableBody>
          {sortedBookmarks.map((bookmark) => (
            <TableRow key={bookmark.id}>
              <TableCell className="font-medium">{bookmark.title}</TableCell>
              <TableCell>
                <a
                  href="#"
                  target="_blank"
                  className="text-blue-500 hover:underline"
                >
                  {bookmark.url}
                </a>
              </TableCell>
              <TableCell className="flex flex-wrap gap-1">
                {bookmark.tags.map((tag, index) => (
                  <Badge variant="outline" key={index}>
                    {tag}
                  </Badge>
                ))}
              </TableCell>
              <TableCell>{bookmark.description}</TableCell>
              <TableCell>{bookmark.createdAt}</TableCell>
              <TableCell className="flex gap-1">
                <Button variant="ghost" size="icon">
                  <Pencil className="size-4" />
                </Button>
                <Button variant="ghost" size="icon">
                  <Trash2 className="size-4" />
                </Button>
              </TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>
    </div>
  )
}

export { Component }
```

Install NPM dependencies:
```bash
npm install cn
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
