<!-- Table · @haydenbleasel · https://21st.dev/@haydenbleasel/components/data-table
     license: MIT · category: table
     Table views are used to display data in a tabular format. They are useful for displaying large amounts of data in a structured way. -->

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
import type {
  Cell,
  Column,
  ColumnDef,
  Header,
  HeaderGroup,
  Row,
  SortingState,
  Table,
} from "@tanstack/react-table";
import {
  flexRender,
  getCoreRowModel,
  getSortedRowModel,
  useReactTable,
} from "@tanstack/react-table";
import { atom, useAtom } from "jotai";
import { ArrowDownIcon, ArrowUpIcon, ChevronsUpDownIcon } from "lucide-react";
import type { HTMLAttributes, ReactNode } from "react";
import { createContext, memo, useCallback, useContext } from "react";
import { Button } from "@/components/ui/button";
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu";
import {
  TableBody as TableBodyRaw,
  TableCell as TableCellRaw,
  TableHeader as TableHeaderRaw,
  TableHead as TableHeadRaw,
  Table as TableRaw,
  TableRow as TableRowRaw,
} from "@/components/ui/table";
import { cn } from "@/lib/utils";

export type { ColumnDef } from "@tanstack/react-table";

const sortingAtom = atom<SortingState>([]);

export const TableContext = createContext<{
  data: unknown[];
  columns: ColumnDef<unknown, unknown>[];
  table: Table<unknown> | null;
}>({
  data: [],
  columns: [],
  table: null,
});

export type TableProviderProps<TData, TValue> = {
  columns: ColumnDef<TData, TValue>[];
  data: TData[];
  children: ReactNode;
  className?: string;
};

export function TableProvider<TData, TValue>({
  columns,
  data,
  children,
  className,
}: TableProviderProps<TData, TValue>) {
  const [sorting, setSorting] = useAtom(sortingAtom);
  const table = useReactTable({
    data,
    columns,
    getCoreRowModel: getCoreRowModel(),
    getSortedRowModel: getSortedRowModel(),
    onSortingChange: (updater) => {
      // @ts-expect-error updater is a function that returns a sorting object
      const newSorting = updater(sorting);

      setSorting(newSorting);
    },
    state: {
      sorting,
    },
  });

  return (
    <TableContext.Provider
      value={{
        data,
        columns: columns as never,
        table: table as never,
      }}
    >
      <TableRaw className={className}>{children}</TableRaw>
    </TableContext.Provider>
  );
}

export type TableHeadProps = {
  header: Header<unknown, unknown>;
  className?: string;
};

export const TableHead = memo(({ header, className }: TableHeadProps) => (
  <TableHeadRaw className={className} key={header.id}>
    {header.isPlaceholder
      ? null
      : flexRender(header.column.columnDef.header, header.getContext())}
  </TableHeadRaw>
));

TableHead.displayName = "TableHead";

export type TableHeaderGroupProps = {
  headerGroup: HeaderGroup<unknown>;
  children: (props: { header: Header<unknown, unknown> }) => ReactNode;
};

export const TableHeaderGroup = ({
  headerGroup,
  children,
}: TableHeaderGroupProps) => (
  <TableRowRaw key={headerGroup.id}>
    {headerGroup.headers.map((header) => children({ header }))}
  </TableRowRaw>
);

export type TableHeaderProps = {
  className?: string;
  children: (props: { headerGroup: HeaderGroup<unknown> }) => ReactNode;
};

export const TableHeader = ({ className, children }: TableHeaderProps) => {
  const { table } = useContext(TableContext);

  return (
    <TableHeaderRaw className={className}>
      {table?.getHeaderGroups().map((headerGroup) => children({ headerGroup }))}
    </TableHeaderRaw>
  );
};

export interface TableColumnHeaderProps<TData, TValue>
  extends HTMLAttributes<HTMLDivElement> {
  column: Column<TData, TValue>;
  title: string;
}

export function TableColumnHeader<TData, TValue>({
  column,
  title,
  className,
}: TableColumnHeaderProps<TData, TValue>) {
  // Extract inline event handlers to prevent unnecessary re-renders
  const handleSortAsc = useCallback(() => {
    column.toggleSorting(false);
  }, [column]);

  const handleSortDesc = useCallback(() => {
    column.toggleSorting(true);
  }, [column]);

  if (!column.getCanSort()) {
    return <div className={cn(className)}>{title}</div>;
  }

  return (
    <div className={cn("flex items-center space-x-2", className)}>
      <DropdownMenu>
        <DropdownMenuTrigger asChild>
          <Button
            className="-ml-3 h-8 data-[state=open]:bg-accent"
            size="sm"
            variant="ghost"
          >
            <span>{title}</span>
            {column.getIsSorted() === "desc" ? (
              <ArrowDownIcon className="ml-2 h-4 w-4" />
            ) : column.getIsSorted() === "asc" ? (
              <ArrowUpIcon className="ml-2 h-4 w-4" />
            ) : (
              <ChevronsUpDownIcon className="ml-2 h-4 w-4" />
            )}
          </Button>
        </DropdownMenuTrigger>
        <DropdownMenuContent align="start">
          <DropdownMenuItem onClick={handleSortAsc}>
            <ArrowUpIcon className="mr-2 h-3.5 w-3.5 text-muted-foreground/70" />
            Asc
          </DropdownMenuItem>
          <DropdownMenuItem onClick={handleSortDesc}>
            <ArrowDownIcon className="mr-2 h-3.5 w-3.5 text-muted-foreground/70" />
            Desc
          </DropdownMenuItem>
        </DropdownMenuContent>
      </DropdownMenu>
    </div>
  );
}

export type TableCellProps = {
  cell: Cell<unknown, unknown>;
  className?: string;
};

export const TableCell = ({ cell, className }: TableCellProps) => (
  <TableCellRaw className={className}>
    {flexRender(cell.column.columnDef.cell, cell.getContext())}
  </TableCellRaw>
);

export type TableRowProps = {
  row: Row<unknown>;
  children: (props: { cell: Cell<unknown, unknown> }) => ReactNode;
  className?: string;
};

export const TableRow = ({ row, children, className }: TableRowProps) => (
  <TableRowRaw
    className={className}
    data-state={row.getIsSelected() && "selected"}
    key={row.id}
  >
    {row.getVisibleCells().map((cell) => children({ cell }))}
  </TableRowRaw>
);

export type TableBodyProps = {
  children: (props: { row: Row<unknown> }) => ReactNode;
  className?: string;
};

export const TableBody = ({ children, className }: TableBodyProps) => {
  const { columns, table } = useContext(TableContext);
  const rows = table?.getRowModel().rows;

  return (
    <TableBodyRaw className={className}>
      {rows?.length ? (
        rows.map((row) => children({ row }))
      ) : (
        <TableRowRaw>
          <TableCellRaw className="h-24 text-center" colSpan={columns.length}>
            No results.
          </TableCellRaw>
        </TableRowRaw>
      )}
    </TableBodyRaw>
  );
};

demo.tsx
"use client";

import {
  TableBody,
  TableCell,
  TableColumnHeader,
  TableHead,
  TableHeader,
  TableHeaderGroup,
  TableProvider,
  TableRow,
} from "@/components/ui/data-table";
import {
  addMonths,
  endOfMonth,
  startOfMonth,
  subDays,
  subMonths,
} from "date-fns";
import type { ColumnDef } from "@tanstack/react-table";
import { ChevronRightIcon } from "lucide-react";
import Image from "next/image";

const today = new Date();

const exampleStatuses = [
  { id: "1", name: "Planned", color: "#6B7280" },
  { id: "2", name: "In Progress", color: "#F59E0B" },
  { id: "3", name: "Done", color: "#10B981" },
]

const exampleFeatures = [
  {
    id: "1",
    name: "AI Scene Analysis",
    startAt: startOfMonth(subMonths(today, 6)),
    endAt: subDays(endOfMonth(today), 5),
    status: exampleStatuses[0],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "1",
      image: "https://cdn.21st.dev/assets/mirror/54/54484be9004d64fc7ae6a2bdccc791f5f9502fbd23841ada359a8d3d7c1aedc8.svg",
      name: "Alice Johnson",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "1", name: "v1.0" },
  },
  {
    id: "2",
    name: "Collaborative Editing",
    startAt: startOfMonth(subMonths(today, 5)),
    endAt: subDays(endOfMonth(today), 5),
    status: exampleStatuses[1],
    group: { id: "2", name: "Collaboration Tools" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "2",
      image: "https://cdn.21st.dev/assets/mirror/a2/a25e21cd9d9f92c7447a525c9e0230bcbe10d447791ebce65d6127cfb40f2698.svg",
      name: "Bob Smith",
    },
    initiative: { id: "2", name: "Real-time Collaboration" },
    release: { id: "1", name: "v1.0" },
  },
  {
    id: "3",
    name: "AI-Powered Color Grading",
    startAt: startOfMonth(subMonths(today, 4)),
    endAt: subDays(endOfMonth(today), 5),
    status: exampleStatuses[2],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "3",
      image: "https://cdn.21st.dev/assets/mirror/97/97261900dab8e48c557cd76aba91b73bc989b48c386504341100a7b28bb9a701.svg",
      name: "Charlie Brown",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "2", name: "v1.1" },
  },
  {
    id: "4",
    name: "Real-time Video Chat",
    startAt: startOfMonth(subMonths(today, 3)),
    endAt: subDays(endOfMonth(today), 12),
    status: exampleStatuses[0],
    group: { id: "2", name: "Collaboration Tools" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "4",
      image: "https://cdn.21st.dev/assets/mirror/60/6093366096f9348ceb484ab6f77c7eb9f2f5de67a712d9d0ce3402a2a69b0aeb.svg",
      name: "Diana Prince",
    },
    initiative: { id: "2", name: "Real-time Collaboration" },
    release: { id: "2", name: "v1.1" },
  },
  {
    id: "5",
    name: "AI Voice-to-Text Subtitles",
    startAt: startOfMonth(subMonths(today, 2)),
    endAt: subDays(endOfMonth(today), 5),
    status: exampleStatuses[1],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "5",
      image: "https://cdn.21st.dev/assets/mirror/6c/6c654fd6db0027932893f8f274741c473490e3f2192d9b315b2a40eb7c9c3ef0.svg",
      name: "Ethan Hunt",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "2", name: "v1.1" },
  },
  {
    id: "6",
    name: "Cloud Asset Management",
    startAt: startOfMonth(subMonths(today, 1)),
    endAt: endOfMonth(today),
    status: exampleStatuses[2],
    group: { id: "3", name: "Cloud Infrastructure" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "6",
      image: "https://cdn.21st.dev/assets/mirror/56/566957498a7dccbf0db4884dd0169b586b30210cbea1a43678491c0c9c037738.svg",
      name: "Fiona Gallagher",
    },
    initiative: { id: "3", name: "Cloud Migration" },
    release: { id: "3", name: "v1.2" },
  },
  {
    id: "7",
    name: "AI-Assisted Video Transitions",
    startAt: startOfMonth(today),
    endAt: endOfMonth(addMonths(today, 1)),
    status: exampleStatuses[0],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "7",
      image: "https://cdn.21st.dev/assets/mirror/79/7950d84bdee32e26a7eabfa117a3f976de3d9391cfee024689ab4a6c64a43fb0.svg",
      name: "George Lucas",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "3", name: "v1.2" },
  },
  {
    id: "8",
    name: "Version Control System",
    startAt: startOfMonth(addMonths(today, 1)),
    endAt: endOfMonth(addMonths(today, 2)),
    status: exampleStatuses[1],
    group: { id: "2", name: "Collaboration Tools" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "8",
      image: "https://cdn.21st.dev/assets/mirror/c8/c8890b07ab6c05166b29093e76604efee170144531d49cc42738329e469b14ca.svg",
      name: "Hannah Montana",
    },
    initiative: { id: "2", name: "Real-time Collaboration" },
    release: { id: "3", name: "v1.2" },
  },
  {
    id: "9",
    name: "AI Content-Aware Fill",
    startAt: startOfMonth(addMonths(today, 2)),
    endAt: endOfMonth(addMonths(today, 3)),
    status: exampleStatuses[2],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "9",
      image: "https://cdn.21st.dev/assets/mirror/db/dbba453df3acb5955303d7a7721e68bd29dd25474e7e2fbab40667d0a8764b6d.svg",
      name: "Ian Malcolm",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "4", name: "v1.3" },
  },
  {
    id: "10",
    name: "Multi-User Permissions",
    startAt: startOfMonth(addMonths(today, 3)),
    endAt: endOfMonth(addMonths(today, 4)),
    status: exampleStatuses[0],
    group: { id: "2", name: "Collaboration Tools" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "10",
      image: "https://cdn.21st.dev/assets/mirror/d7/d728a3a74b0be939c00e927f040a1c32715619e9e2f061ddce4396055d959b2a.svg",
      name: "Julia Roberts",
    },
    initiative: { id: "2", name: "Real-time Collaboration" },
    release: { id: "4", name: "v1.3" },
  },
  {
    id: "11",
    name: "AI-Powered Audio Enhancement",
    startAt: startOfMonth(addMonths(today, 4)),
    endAt: endOfMonth(addMonths(today, 5)),
    status: exampleStatuses[1],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "11",
      image: "https://cdn.21st.dev/assets/mirror/07/07827dea49bbb0dee76d8f59e963431dd9576a895e6dd9d56151d55488b4ad21.svg",
      name: "Kevin Hart",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "4", name: "v1.3" },
  },
  {
    id: "12",
    name: "Real-time Project Analytics",
    startAt: startOfMonth(addMonths(today, 5)),
    endAt: endOfMonth(addMonths(today, 6)),
    status: exampleStatuses[2],
    group: { id: "3", name: "Cloud Infrastructure" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "12",
      image: "https://cdn.21st.dev/assets/mirror/2f/2fc5d934df789da245d3cae0f882c51513b388cab0cc6754116d5a35a04122e2.svg",
      name: "Lara Croft",
    },
    initiative: { id: "3", name: "Cloud Migration" },
    release: { id: "5", name: "v1.4" },
  },
  {
    id: "13",
    name: "AI Scene Recommendations",
    startAt: startOfMonth(addMonths(today, 6)),
    endAt: endOfMonth(addMonths(today, 7)),
    status: exampleStatuses[0],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "13",
      image: "https://cdn.21st.dev/assets/mirror/f9/f9acc66b9b42a4f3396f879d06f6ee3439f7e1b3df5a9ac0649220defe3b2a8e.svg",
      name: "Michael Scott",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "5", name: "v1.4" },
  },
  {
    id: "14",
    name: "Collaborative Storyboarding",
    startAt: startOfMonth(addMonths(today, 7)),
    endAt: endOfMonth(addMonths(today, 8)),
    status: exampleStatuses[1],
    group: { id: "2", name: "Collaboration Tools" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "14",
      image: "https://cdn.21st.dev/assets/mirror/7f/7fbd157b0c0fbeb0b82a784d892589237a276fa946354752f6528522f474a4ae.svg",
      name: "Natalie Portman",
    },
    initiative: { id: "2", name: "Real-time Collaboration" },
    release: { id: "5", name: "v1.4" },
  },
  {
    id: "15",
    name: "AI-Driven Video Compression",
    startAt: startOfMonth(addMonths(today, 8)),
    endAt: endOfMonth(addMonths(today, 9)),
    status: exampleStatuses[2],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "15",
      image: "https://cdn.21st.dev/assets/mirror/d5/d57fe3ed7e3cafebc3a7e99ae4c99791cc3191e7de11f08a0cfa28597b140d6c.svg",
      name: "Oscar Isaac",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "6", name: "v1.5" },
  },
  {
    id: "16",
    name: "Global CDN Integration",
    startAt: startOfMonth(addMonths(today, 9)),
    endAt: endOfMonth(addMonths(today, 10)),
    status: exampleStatuses[0],
    group: { id: "3", name: "Cloud Infrastructure" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "16",
      image: "https://cdn.21st.dev/assets/mirror/04/04c06913139c19a4deccf50f9b03e81dcbe7db9f9c687e27bd172f4ff15933bb.svg",
      name: "Penelope Cruz",
    },
    initiative: { id: "3", name: "Cloud Migration" },
    release: { id: "6", name: "v1.5" },
  },
  {
    id: "17",
    name: "AI Object Tracking",
    startAt: startOfMonth(addMonths(today, 10)),
    endAt: endOfMonth(addMonths(today, 11)),
    status: exampleStatuses[1],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "17",
      image: "https://cdn.21st.dev/assets/mirror/40/40dced645e851c60735549ea8d161dc363f4a07e99560590cf528e330ecdb759.svg",
      name: "Quentin Tarantino",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "6", name: "v1.5" },
  },
  {
    id: "18",
    name: "Real-time Language Translation",
    startAt: startOfMonth(addMonths(today, 11)),
    endAt: endOfMonth(addMonths(today, 12)),
    status: exampleStatuses[2],
    group: { id: "2", name: "Collaboration Tools" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "18",
      image: "https://cdn.21st.dev/assets/mirror/45/45181d4d24c1463da2df183f4d31ba9b01e933a3a5dd3ae2f91dba03a933b4d8.svg",
      name: "Rachel Green",
    },
    initiative: { id: "2", name: "Real-time Collaboration" },
    release: { id: "7", name: "v1.6" },
  },
  {
    id: "19",
    name: "AI-Powered Video Summarization",
    startAt: startOfMonth(addMonths(today, 12)),
    endAt: endOfMonth(addMonths(today, 13)),
    status: exampleStatuses[0],
    group: { id: "1", name: "Core AI Features" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "19",
      image: "https://cdn.21st.dev/assets/mirror/bd/bd52b0341ad401c92dbac78e1cc3dd40e4d2e43293fb7c9709ce94757f2e04c8.svg",
      name: "Samuel L. Jackson",
    },
    initiative: { id: "1", name: "AI Integration" },
    release: { id: "7", name: "v1.6" },
  },
  {
    id: "20",
    name: "Blockchain-based Asset Licensing",
    startAt: startOfMonth(addMonths(today, 13)),
    endAt: endOfMonth(addMonths(today, 14)),
    status: exampleStatuses[1],
    group: { id: "3", name: "Cloud Infrastructure" },
    product: { id: "1", name: "Video Editor Pro" },
    owner: {
      id: "20",
      image: "https://cdn.21st.dev/assets/mirror/4e/4ec3497f95745482a1d68be2980df5e5f1ccc553de1b29acdeeb342887e1140d.svg",
      name: "Tom Hanks",
    },
    initiative: { id: "3", name: "Cloud Migration" },
    release: { id: "7", name: "v1.6" },
  },
];

const TableExample = () => {
  const columns: ColumnDef<(typeof exampleFeatures)[number]>[] = [
    {
      accessorKey: "name",
      header: ({ column }) => (
        <TableColumnHeader column={column} title="Name" />
      ),
      cell: ({ row }) => (
        <div className="flex items-center gap-2">
          <div className="relative">
            <Image
              src={row.original.owner.image}
              alt={row.original.owner.name}
              width={24}
              height={24}
              unoptimized
              className="h-6 w-6 rounded-full"
            />
            <div
              className="absolute right-0 bottom-0 h-2 w-2 rounded-full ring-2 ring-background"
              style={{
                backgroundColor: row.original.status.color,
              }}
            />
          </div>
          <div>
            <span className="font-medium">{row.original.name}</span>
            <div className="flex items-center gap-1 text-muted-foreground text-xs">
              <span>{row.original.product.name}</span>
              <ChevronRightIcon size={12} />
              <span>{row.original.group.name}</span>
            </div>
          </div>
        </div>
      ),
    },
    {
      accessorKey: "startAt",
      header: ({ column }) => (
        <TableColumnHeader column={column} title="Start At" />
      ),
      cell: ({ row }) =>
        new Intl.DateTimeFormat("en-US", {
          dateStyle: "medium",
        }).format(row.original.startAt),
    },
    {
      accessorKey: "endAt",
      header: ({ column }) => (
        <TableColumnHeader column={column} title="End At" />
      ),
      cell: ({ row }) =>
        new Intl.DateTimeFormat("en-US", {
          dateStyle: "medium",
        }).format(row.original.endAt),
    },
    {
      id: "release",
      accessorFn: (row) => row.release.id,
      header: ({ column }) => (
        <TableColumnHeader column={column} title="Release" />
      ),
      cell: ({ row }) => row.original.release.name,
    },
  ];

  return (
    <div className="max-h-[40vh] overflow-auto">
      <TableProvider
        columns={columns}
        data={exampleFeatures}
        className="border rounded-lg"
      >
        <TableHeader>
          {({ headerGroup }) => (
            <TableHeaderGroup key={headerGroup.id} headerGroup={headerGroup}>
              {({ header }) => <TableHead key={header.id} header={header} />}
            </TableHeaderGroup>
          )}
        </TableHeader>
        <TableBody>
          {({ row }) => (
            <TableRow key={row.id} row={row}>
              {({ cell }) => <TableCell key={cell.id} cell={cell} />}
            </TableRow>
          )}
        </TableBody>
      </TableProvider>
    </div>
  );
};

export { TableExample };
```

Install NPM dependencies:
```bash
npm install @tanstack/react-table jotai lucide-react zustand
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button dropdown-menu table
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
