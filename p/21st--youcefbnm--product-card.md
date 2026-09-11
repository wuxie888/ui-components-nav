<!-- ProductCard · @youcefbnm · https://21st.dev/@youcefbnm/components/product-card
     license: MIT · category: gallery
     Component for displaying multiple product colors images, perfect for your next ecommerce project. -->

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

import * as React from 'react';
import { cn } from '@/lib/utils';
import { cva, VariantProps } from 'class-variance-authority';

export const cardVariants = cva('rounded-xl  flex flex-col border gap-6 p-6', {
  variants: {
    variant: {
      default: 'bg-card text-card-foreground shadow-sm',
      glass:
        'bg-gradient-to-b from-card/10 to-card/5 border-secondary/15 shadow-lg backdrop-blur-sm',
    },
  },
  defaultVariants: {
    variant: 'default',
  },
});

export function Card({
  className,
  variant,
  ...props
}: React.ComponentProps<'div'> & VariantProps<typeof cardVariants>) {
  return (
    <div
      data-slot="card"
      className={cn(cardVariants({ variant, className }))}
      {...props}
    />
  );
}

demo.tsx
import { ProductCard } from "@/components/ui/product-card"

const PRODUCT = {
  id: "product-1",
  colors: ["rgb(147, 171, 193)", "rgb(187, 203, 195)", "rgb(222, 156, 94)"],
  images: [
    {
      id: "product-1-color-1",
      color: "rgb(187, 195, 203)",
      images: [
        "https://cdn.21st.dev/assets/mirror/53/537d38061444bce4d091a697d923215748ce66d4165ef2734bc092f9050ef866.jpg",
        "https://cdn.21st.dev/assets/mirror/49/492dfa51cfa08901abf86b47cf23619ddc9efa54e854d3484b853dfd2b76d94b.jpg",
        "https://cdn.21st.dev/assets/mirror/3f/3fde04254fbccf8713ab5888b94cbc0ae44c3d675f4e5a0e8f5c48c9fce68750.jpg",
        "https://cdn.21st.dev/assets/mirror/ea/eaf8edefed933521cdf1271679ecb6f51e1345a0827e682a5f6d6ebd8c44f28b.jpg",
        "https://cdn.21st.dev/assets/mirror/7c/7c42acb8922525e008bcae3f949fd5e81736d3781f36a67ef367208e0be24251.jpg",
        "https://cdn.21st.dev/assets/mirror/50/50106e117a1f838642bdaae8366bdc9f06cf37579686212266977585a29bdcbc.jpg",
        "https://cdn.21st.dev/assets/mirror/22/2279da377567e58ee01cb45524c1c1577f680b290668de4767029f6ff6f4c203.jpg",
      ],
    },
    {
      id: "product-1-color-2",
      color: "rgb(187, 203, 195)",
      images: [
        "https://cdn.21st.dev/assets/mirror/7f/7fbd5da32ffa399ec27798c7cd89f3ea898f6a8761c556c11f6481963576caa0.jpg",
        "https://cdn.21st.dev/assets/mirror/4f/4fd107a57e8bc4df6d0c3d31b1ba8cb8bb96761d4448814719e4368933d234cf.jpg",
        "https://cdn.21st.dev/assets/mirror/92/925a717d9599f4602b23af84a61b70d69bb9ff266cddd638e52fa486c43dcac2.jpg",
        "https://cdn.21st.dev/assets/mirror/f5/f5b12310b9fdd698019d7bd1949b86ccf9b5e7772a6a241334ffd3bff9637c33.jpg",
        "https://cdn.21st.dev/assets/mirror/b0/b0b5aa39b82a5ca6db3b78403271e5d7171e71580ac642bb85fa1c219462b0e0.jpg",
        "https://cdn.21st.dev/assets/mirror/9d/9d8174b2f31edcde8871e4c185fa2d7c730f0fdc2604fd09551e1774e3b81563.jpg",
        "https://cdn.21st.dev/assets/mirror/da/dab18279454ec87249f940173277a0d550413d4e22c7e1998a76ca51f283504d.jpg",
      ],
    },
    {
      id: "product-1-color-3",
      color: "rgb(222, 156, 94)",
      images: [
        "https://cdn.21st.dev/assets/mirror/a6/a6b11957af2953c9f39a430c211e4a77770e1569beef68dfcbe45347e13f9756.jpg",
        "https://cdn.21st.dev/assets/mirror/26/263667639cf28c391378f316de1aca1f9a8fb8d273a563ee31efd62a0dc2b2d2.jpg",
        "https://cdn.21st.dev/assets/mirror/6c/6c90a47b466906814de5022e9e0b078c37b3bceef86dc6f0cd310d91d264f462.jpg",
        "https://cdn.21st.dev/assets/mirror/e8/e8b8559d829c2ae7273adeb587d7fa95d44ce5f3b0994f83ceaf813a544bb9d0.jpg",
        "https://cdn.21st.dev/assets/mirror/24/249f113549820f09f7a8b36415497fd18e79a282c0dcae715859acab66795eae.jpg",
        "https://cdn.21st.dev/assets/mirror/ff/ff3ead452be3ead0bab2b617ca4f1f251f90e3ef73b6ebad5c1f3f1653fca3c9.jpg",
        "https://cdn.21st.dev/assets/mirror/14/1429433cbca8274fe900adbeaf583b5e4d67e44a9b10c6a6c136bfd2921fc0ad.jpg",
      ],
    },
  ],
}


export function ProductCardDemo () {

  return (
    <div className="container min-h-svh place-content-center">
      <ProductCard
        id={PRODUCT.id}
        className="w-64"
        images={PRODUCT.images}
        colors={PRODUCT.colors}
      />
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install motion
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
