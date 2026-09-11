<!-- Payments Table with Cell Selection · @bundui · https://21st.dev/@bundui/components/table11
     license: MIT · category: table
     A payments data table showing customer avatars, product, price, status badges, and highlighted selected cells. -->

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
"use client";

import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow
} from "@/components/ui/table";
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";
import { Badge } from "@/components/ui/badge";

const payments = [
  {
    id: "PAY-001",
    customer: { name: "John Doe", avatar: "https://i.pravatar.cc/150?img=1" },
    product: "Premium Plan",
    price: "$99.00",
    paymentType: "Credit Card",
    status: "Completed",
    date: "2024-01-15"
  },
  {
    id: "PAY-002",
    customer: { name: "Jane Smith", avatar: "https://i.pravatar.cc/150?img=2" },
    product: "Basic Plan",
    price: "$29.00",
    paymentType: "PayPal",
    status: "Completed",
    date: "2024-01-16"
  },
  {
    id: "PAY-003",
    customer: { name: "Mike Johnson", avatar: "https://i.pravatar.cc/150?img=3" },
    product: "Enterprise Plan",
    price: "$299.00",
    paymentType: "Bank Transfer",
    status: "Pending",
    date: "2024-01-17"
  },
  {
    id: "PAY-004",
    customer: { name: "Sarah Wilson", avatar: "https://i.pravatar.cc/150?img=4" },
    product: "Premium Plan",
    price: "$99.00",
    paymentType: "Stripe",
    status: "Completed",
    date: "2024-01-18"
  },
  {
    id: "PAY-005",
    customer: { name: "David Brown", avatar: "https://i.pravatar.cc/150?img=5" },
    product: "Basic Plan",
    price: "$29.00",
    paymentType: "Credit Card",
    status: "Failed",
    date: "2024-01-19"
  },
  {
    id: "PAY-006",
    customer: { name: "Emily Davis", avatar: "https://i.pravatar.cc/150?img=6" },
    product: "Premium Plan",
    price: "$99.00",
    paymentType: "PayPal",
    status: "Completed",
    date: "2024-01-20"
  }
];

export type Payment = (typeof payments)[number];

type ColumnKey = "id" | "customer" | "product" | "price" | "paymentType" | "status" | "date";

const selectedCells = new Set<string>(["PAY-002-price", "PAY-003-customer"]);

export default function TableComponent() {
  const getStatusVariant = (status: Payment["status"]) => {
    switch (status) {
      case "Completed":
        return "default";
      case "Pending":
        return "secondary";
      case "Failed":
        return "destructive";
      default:
        return "outline";
    }
  };

  const formatDate = (dateString: string) => {
    const date = new Date(dateString);
    return date.toLocaleDateString("en-US", {
      year: "numeric",
      month: "short",
      day: "numeric"
    });
  };

  return (
    <div className="w-full max-w-6xl space-y-4">
      <Table>
        <TableHeader>
          <TableRow>
            <TableHead>ID</TableHead>
            <TableHead>Customer</TableHead>
            <TableHead>Product</TableHead>
            <TableHead>Price</TableHead>
            <TableHead>Payment Type</TableHead>
            <TableHead>Status</TableHead>
            <TableHead>Date</TableHead>
          </TableRow>
        </TableHeader>
        <TableBody>
          {payments.map((payment) => (
            <TableRow key={payment.id}>
              <TableCell
                data-selected={selectedCells.has(`${payment.id}-id`)}
                className="data-[selected=true]:bg-amber-600/30 font-mono text-sm">
                {payment.id}
              </TableCell>
              <TableCell
                data-selected={selectedCells.has(`${payment.id}-customer`)}
                className="data-[selected=true]:bg-amber-600/30">
                <div className="flex items-center gap-2">
                  <Avatar>
                    <AvatarImage src={payment.customer.avatar} alt={payment.customer.name} />
                    <AvatarFallback>
                      {payment.customer.name
                        .split(" ")
                        .map((n) => n[0])
                        .join("")}
                    </AvatarFallback>
                  </Avatar>
                  <span>{payment.customer.name}</span>
                </div>
              </TableCell>
              <TableCell
                data-selected={selectedCells.has(`${payment.id}-product`)}
                className="data-[selected=true]:bg-amber-600/30">
                {payment.product}
              </TableCell>
              <TableCell
                data-selected={selectedCells.has(`${payment.id}-price`)}
                className="data-[selected=true]:bg-amber-600/30 font-medium">
                {payment.price}
              </TableCell>
              <TableCell
                data-selected={selectedCells.has(`${payment.id}-paymentType`)}
                className="data-[selected=true]:bg-amber-600/30">
                {payment.paymentType}
              </TableCell>
              <TableCell
                data-selected={selectedCells.has(`${payment.id}-status`)}
                className="data-[selected=true]:bg-amber-600/30">
                <Badge variant={getStatusVariant(payment.status)}>{payment.status}</Badge>
              </TableCell>
              <TableCell
                data-selected={selectedCells.has(`${payment.id}-date`)}
                className="text-muted-foreground data-[selected=true]:bg-amber-600/30">
                {formatDate(payment.date)}
              </TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>
    </div>
  );
}

demo.tsx
import TableComponent from "@/components/ui/table11";

export default function DemoDefault() {
  return (
    <div className="flex w-full justify-center p-6">
      <TableComponent />
    </div>
  );
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar badge table
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
