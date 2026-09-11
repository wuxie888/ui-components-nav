<!-- Users List Datatable · @shadcnstore · https://21st.dev/@shadcnstore/components/datatable-1
     license: no-license · category: pagination
     A user management data table with search, filters, export, pagination, row selection, avatars, role, last-login, two-factor status badges, and per-row action menus. -->

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
components/ui/page.tsx
import { Datatable1 } from "@/components/blocks/application/datatables/datatable-1/components/datatable-1"

export default function Page() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center">
      <div className="w-full">
        <Datatable1 />
      </div>
    </div>
  )
}

components/ui/datatable-1.tsx
'use client'

import { useState } from 'react'
import { ChevronDown } from 'lucide-react'

import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar'
import { Badge } from '@/components/ui/badge'
import { Button } from '@/components/ui/button'
import { Card, CardContent, CardHeader } from '@/components/ui/card'
import { CheckboxCheck } from '@/components/motion/checkbox-check'
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuGroup,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from '@/components/ui/dropdown-menu'
import { Table, TableBody, TableCell, TableHead, TableHeader, TableRow } from '@/components/ui/table'

import { usersData } from "@/components/data/datatable-1-data"
import { Datatable1Pagination } from './datatable-1-pagination'
import { Datatable1Toolbar } from './datatable-1-toolbar'

const ITEMS_PER_PAGE = 6

const HEAD_CLASS = 'p-4 font-medium text-sm text-muted-foreground uppercase tracking-wider'

function getInitials(name: string) {
  return name
    .split(' ')
    .map(part => part[0])
    .join('')
    .toUpperCase()
}

export function Datatable1() {
  const [currentPage, setCurrentPage] = useState(1)
  const [selectedUsers, setSelectedUsers] = useState<string[]>([])

  const currentUsers = usersData.slice((currentPage - 1) * ITEMS_PER_PAGE, currentPage * ITEMS_PER_PAGE)

  const toggleUserSelection = (userId: string) => {
    setSelectedUsers(prev => (prev.includes(userId) ? prev.filter(id => id !== userId) : [...prev, userId]))
  }

  const allSelected = selectedUsers.length === currentUsers.length && currentUsers.length > 0

  const toggleAllUsers = () => {
    setSelectedUsers(allSelected ? [] : currentUsers.map(user => user.id))
  }

  return (
    <div className="w-full max-w-7xl flex flex-col gap-6 my-8 mx-auto px-4 sm:px-6 lg:px-8">
      <Card className="pb-0 gap-0">
        <CardHeader className="border-b border-border gap-0">
          <Datatable1Toolbar />
        </CardHeader>
        <CardContent className="p-0">
          <Table>
            <TableHeader>
              <TableRow className="bg-muted/50 hover:bg-muted/50">
                <TableHead className="p-4">
                  <CheckboxCheck checked={allSelected} onCheckedChange={toggleAllUsers} aria-label="Select all users" />
                </TableHead>
                <TableHead className={HEAD_CLASS}>User</TableHead>
                <TableHead className={HEAD_CLASS}>Role</TableHead>
                <TableHead className={HEAD_CLASS}>Last Login</TableHead>
                <TableHead className={HEAD_CLASS}>Two-Step</TableHead>
                <TableHead className={HEAD_CLASS}>Joined Date</TableHead>
                <TableHead className={HEAD_CLASS}>Actions</TableHead>
              </TableRow>
            </TableHeader>
            <TableBody>
              {currentUsers.map(user => (
                <TableRow key={user.id} className="hover:bg-muted/30">
                  <TableCell className="p-4">
                    <CheckboxCheck
                      checked={selectedUsers.includes(user.id)}
                      onCheckedChange={() => toggleUserSelection(user.id)}
                      aria-label={`Select ${user.name}`}
                    />
                  </TableCell>
                  <TableCell className="p-4">
                    <div className="flex items-center gap-3">
                      <Avatar className="size-10 bg-muted">
                        <AvatarImage src={user.avatar} alt={user.name} />
                        <AvatarFallback className="bg-primary/10 text-primary font-semibold">
                          {getInitials(user.name)}
                        </AvatarFallback>
                      </Avatar>
                      <div>
                        <div className="font-medium text-foreground">{user.name}</div>
                        <div className="text-sm text-muted-foreground">{user.email}</div>
                      </div>
                    </div>
                  </TableCell>
                  <TableCell className="p-4 text-sm text-muted-foreground">{user.role}</TableCell>
                  <TableCell className="p-4 text-sm font-medium text-foreground">{user.lastLogin}</TableCell>
                  <TableCell className="p-4 text-center">
                    {user.twoStep ? (
                      <Badge
                        variant="outline"
                        className="px-2.5 py-0.5 font-semibold bg-green-50 text-green-700 border-green-200 dark:bg-green-950 dark:text-green-400 dark:border-green-800"
                      >
                        Enabled
                      </Badge>
                    ) : (
                      <span className="text-sm text-muted-foreground">-</span>
                    )}
                  </TableCell>
                  <TableCell className="p-4 text-sm text-muted-foreground">{user.joinedDate}</TableCell>
                  <TableCell className="p-4">
                    <DropdownMenu>
                      <DropdownMenuTrigger render={<Button variant="ghost" size="sm" className="h-8 px-3 text-xs cursor-pointer" />}>
                        Actions
                        <ChevronDown data-icon="inline-end" />
                      </DropdownMenuTrigger>
                      <DropdownMenuContent align="end">
                        <DropdownMenuGroup>
                          <DropdownMenuItem className="cursor-pointer py-2">Edit</DropdownMenuItem>
                          <DropdownMenuItem className="cursor-pointer py-2">View Details</DropdownMenuItem>
                          <DropdownMenuItem className="text-destructive cursor-pointer py-2">Delete</DropdownMenuItem>
                        </DropdownMenuGroup>
                      </DropdownMenuContent>
                    </DropdownMenu>
                  </TableCell>
                </TableRow>
              ))}
            </TableBody>
          </Table>

          <Datatable1Pagination
            page={currentPage}
            pageSize={ITEMS_PER_PAGE}
            total={usersData.length}
            onPageChange={setCurrentPage}
          />
        </CardContent>
      </Card>
    </div>
  )
}

export default Datatable1

components/ui/datatable-1-pagination.tsx
'use client'

import { ChevronLeft, ChevronRight } from 'lucide-react'

import { Button } from '@/components/ui/button'

type Datatable1PaginationProps = {
  page: number
  pageSize: number
  total: number
  onPageChange: (page: number) => void
}

export function Datatable1Pagination({ page, pageSize, total, onPageChange }: Datatable1PaginationProps) {
  const totalPages = Math.ceil(total / pageSize)

  return (
    <div className="flex items-center justify-between p-4 border-t border-border">
      <div className="text-sm text-muted-foreground">
        Showing {(page - 1) * pageSize + 1} to {Math.min(page * pageSize, total)} of {total} entries
      </div>
      <div className="flex items-center gap-2">
        <Button
          variant="outline"
          size="icon"
          onClick={() => onPageChange(Math.max(1, page - 1))}
          disabled={page === 1}
          aria-label="Go to previous page"
          className="size-9 cursor-pointer"
        >
          <ChevronLeft />
        </Button>
        {Array.from({ length: totalPages }, (_, index) => index + 1).map(pageNumber => (
          <Button
            key={pageNumber}
            variant={page === pageNumber ? 'default' : 'outline'}
            size="icon"
            onClick={() => onPageChange(pageNumber)}
            aria-label={`Go to page ${pageNumber}`}
            aria-current={page === pageNumber ? 'page' : undefined}
            className="size-9 cursor-pointer"
          >
            {pageNumber}
          </Button>
        ))}
        <Button
          variant="outline"
          size="icon"
          onClick={() => onPageChange(Math.min(totalPages, page + 1))}
          disabled={page === totalPages}
          aria-label="Go to next page"
          className="size-9 cursor-pointer"
        >
          <ChevronRight />
        </Button>
      </div>
    </div>
  )
}

components/ui/datatable-1-toolbar.tsx
'use client'

import { ChevronDown, Download, Filter, Plus, Search } from 'lucide-react'

import { Button } from '@/components/ui/button'
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuGroup,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from '@/components/ui/dropdown-menu'
import { InputGroup, InputGroupAddon, InputGroupInput } from '@/components/ui/input-group'

export function Datatable1Toolbar() {
  return (
    <div className="flex flex-col sm:flex-row items-center gap-4">
      <InputGroup className="flex-1 max-w-sm">
        <InputGroupAddon>
          <Search />
        </InputGroupAddon>
        <InputGroupInput placeholder="Search user" aria-label="Search user" />
      </InputGroup>
      <div className="sm:ml-auto flex items-center gap-2 flex-wrap justify-center">
        <Button variant="outline" size="sm" className="h-8 px-3 text-xs cursor-pointer">
          <Filter data-icon="inline-start" />
          Filter
        </Button>
        <DropdownMenu>
          <DropdownMenuTrigger render={<Button variant="outline" size="sm" className="h-8 px-3 text-xs cursor-pointer" />}>
            <Download data-icon="inline-start" />
            Export
            <ChevronDown data-icon="inline-end" />
          </DropdownMenuTrigger>
          <DropdownMenuContent align="end">
            <DropdownMenuGroup>
              <DropdownMenuItem className="cursor-pointer">Export as CSV</DropdownMenuItem>
              <DropdownMenuItem className="cursor-pointer">Export as Excel</DropdownMenuItem>
              <DropdownMenuItem className="cursor-pointer">Export as PDF</DropdownMenuItem>
            </DropdownMenuGroup>
          </DropdownMenuContent>
        </DropdownMenu>
        <Button size="sm" className="h-8 px-3 text-xs cursor-pointer">
          <Plus data-icon="inline-start" />
          Add User
        </Button>
      </div>
    </div>
  )
}

components/ui/datatable-1-data.ts
export interface User {
  id: string;
  name: string;
  email: string;
  avatar?: string;
  role: string;
  lastLogin: string;
  twoStep: boolean;
  joinedDate: string;
}

export const usersData: User[] = [
  {
    id: '1',
    name: 'Emma Smith',
    email: 'smith@kpmg.com',
    avatar: 'https://notion-avatars.netlify.app/api/avatar?preset=female-1',
    role: 'Administrator',
    lastLogin: 'Yesterday',
    twoStep: false,
    joinedDate: '24 Jun 2025, 6:05 pm',
  },
  {
    id: '2',
    name: 'Melody Macy',
    email: 'melody@altbox.com',
    avatar: 'https://notion-avatars.netlify.app/api/avatar?preset=female-2',
    role: 'Analyst',
    lastLogin: '20 mins ago',
    twoStep: true,
    joinedDate: '22 Sep 2025, 10:30 am',
  },
  {
    id: '3',
    name: 'Max Smith',
    email: 'max@kt.com',
    avatar: 'https://notion-avatars.netlify.app/api/avatar?preset=male-1',
    role: 'Developer',
    lastLogin: '3 days ago',
    twoStep: false,
    joinedDate: '10 Nov 2025, 6:43 am',
  },
  {
    id: '4',
    name: 'Ana Crown',
    email: 'ana.cf@limtel.com',
    avatar: 'https://notion-avatars.netlify.app/api/avatar?preset=female-3',
    role: 'Developer',
    lastLogin: '2 days ago',
    twoStep: true,
    joinedDate: '20 Dec 2025, 6:43 am',
  },
  {
    id: '5',
    name: 'Robert Doe',
    email: 'robert@benko.com',
    avatar: 'https://notion-avatars.netlify.app/api/avatar?preset=male-2',
    role: 'Administrator',
    lastLogin: '5 days ago',
    twoStep: false,
    joinedDate: '19 Aug 2025, 2:40 pm',
  },
  {
    id: '6',
    name: 'John Miller',
    email: 'miller@mapple.com',
    avatar: 'https://notion-avatars.netlify.app/api/avatar?preset=male-3',
    role: 'Trial',
    lastLogin: '3 weeks ago',
    twoStep: false,
    joinedDate: '15 Apr 2025, 10:10 pm',
  },
  {
    id: '7',
    name: 'Sarah Johnson',
    email: 'sarah.j@techcorp.com',
    avatar: 'https://notion-avatars.netlify.app/api/avatar?preset=female-4',
    role: 'Manager',
    lastLogin: '1 hour ago',
    twoStep: true,
    joinedDate: '05 Jan 2025, 9:15 am',
  },
  {
    id: '8',
    name: 'Michael Brown',
    email: 'mbrown@devstudio.io',
    avatar: 'https://notion-avatars.netlify.app/api/avatar?preset=male-4',
    role: 'Designer',
    lastLogin: '2 hours ago',
    twoStep: false,
    joinedDate: '12 Feb 2025, 3:30 pm',
  },
];

demo.tsx
import { Datatable1 } from "@/components/ui/datatable-1";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center">
      <div className="w-full">
        <Datatable1 />
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
npx shadcn@latest add avatar badge button card checkbox dropdown-menu input-group motion-checkbox-check table
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
