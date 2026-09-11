<!-- Animate Table Motion · @tailwind-admin · https://21st.dev/@tailwind-admin/components/animate-table-motion
     license: no-license · category: table
     A data table with staggered row reveal animations, user avatars, and colored category badges built with Framer Motion. -->

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
components/animatedComponents/table/animate-table/AnimateTableMotion.tsx
"use client";

import { motion, Variants } from "framer-motion";
import {
    Table,
    TableHeader,
    TableRow,
    TableHead,
    TableCell,
} from "@/components/ui/table";
import Image from "next/image";

const tableVariants: Variants = {
    hidden: {},
    show: {
        transition: { staggerChildren: 0.25 },
    },
};

const rowVariants: Variants = {
    hidden: { opacity: 0, y: 20 },
    show: {
        opacity: 1,
        y: 0,
        transition: {
            duration: 0.5,
            ease: "easeOut",
        },
    },
};

const products = [
  {
    name: 'Apple MacBook Pro 17"',
    color: "Silver",
    category: "Laptop",
    badgeColor:"bg-primary/10 text-primary",
    price: "$2999",
    user: {
      name: "John Doe",
      email: "john@example.com",
      image: "/images/profile/user-1.jpg",
    },
  },
  {
    name: "Microsoft Surface Pro",
    color: "White",
    category: "Laptop PC",
    badgeColor:"bg-secondary/10 text-secondary",
    price: "$1999",
    user: {
      name: "Jane Smith",
      email: "jane@example.com",
      image: "/images/profile/user-2.jpg",
    },
  },
  {
    name: "Magic Mouse 2",
    color: "Black",
    category: "Accessories",
    badgeColor:"bg-success/10 text-success",
    price: "$99",
    user: {
      name: "Alice Johnson",
      email: "alice@example.com",
      image: "/images/profile/user-3.jpg",
    },
  },
  {
    name: "iPad Pro 12.9",
    color: "Space Gray",
    category: "Tablet",
    badgeColor:"bg-warning/10 text-warning",
    price: "$1099",
    user: {
      name: "Michael Brown",
      email: "michael@example.com",
      image: "/images/profile/user-4.jpg",
    },
  },
  {
    name: "AirPods Pro",
    color: "White",
    category: "Accessories",
    badgeColor:"bg-success/10 text-success",
    price: "$249",
    user: {
      name: "Emma Wilson",
      email: "emma@example.com",
      image: "/images/profile/user-5.jpg",
    },
  },
];


export default function AnimateTableMotion({replayAnimation=0}:any) {
    return (
        <motion.div
            initial={{ opacity: 0, scale: 0.98 }}
            animate={{ opacity: 1, scale: 1 }}
            transition={{ duration: 0.4, ease: "easeOut" }}
            className="rounded-md overflow-hidden shadow"
            key={replayAnimation}
        >
            <Table className="overflow-hidden">
                <TableHeader className="bg-primary/10">
                    <TableRow>
                        <TableHead>User Info</TableHead>
                        <TableHead>Product name</TableHead>
                        <TableHead> Color</TableHead>
                        <TableHead>Category</TableHead>
                        <TableHead>Price</TableHead>
                        <TableHead>
                            <span className="sr-only">Edit</span>
                        </TableHead>
                    </TableRow>
                </TableHeader>

                <motion.tbody
                    variants={tableVariants}
                    initial="hidden"
                    animate="show"
                >
                    {products.map((product, idx) => (
                        <motion.tr
                            key={idx}
                            variants={rowVariants}
                            className="bg-white dark:bg-gray-800"
                        >
                            <TableCell>
                                <div className="flex items-center gap-2">
                                    <Image src={product.user.image} width={36} height={36} className="rounded-full" alt="user" />
                                    <div className="flex flex-col">
                                        <span className="font-medium text-gray-900 dark:text-white">
                                            {product.user.name}
                                        </span>
                                        <span className="text-sm text-gray-500 dark:text-gray-400">
                                            {product.user.email}
                                        </span>
                                    </div>
                                </div>
                            </TableCell>
                            <TableCell className="font-medium text-gray-900 dark:text-white">
                                {product.name}
                            </TableCell>
                            <TableCell>{product.color}</TableCell>
                            <TableCell><span className={`py-1 text-xs px-3 rounded-full ${product.badgeColor}`} >{product.category}</span></TableCell>
                            <TableCell>{product.price}</TableCell>


                            <TableCell>
                                <a
                                    href="#"
                                    className="font-medium text-primary hover:underline dark:text-primary"
                                >
                                    Edit
                                </a>
                            </TableCell>
                        </motion.tr>
                    ))}
                </motion.tbody>
            </Table>
        </motion.div>
    );
}

demo.tsx
import AnimateTableMotion from "@/components/ui/animate-table-motion";

export default function Default() {
  return (
    <div className="w-full max-w-4xl mx-auto p-6">
      <AnimateTableMotion />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install framer-motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add table
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
