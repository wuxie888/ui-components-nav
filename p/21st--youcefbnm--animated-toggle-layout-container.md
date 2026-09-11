<!-- Animated Toggle Layout Container · @youcefbnm · https://21st.dev/@youcefbnm/components/animated-toggle-layout-container
     license: MIT · category: grid
     - Container toggling and animating between multiple layouts, useful for your ecommerce store and large lists to enhance user experience. -->

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
import { HTMLMotionProps, LayoutGroup, motion } from 'motion/react';
import { cn } from '@/lib/utils';
import React from 'react';

const layout_config = {
  list: {
    mode: 'list',
    className: 'flex flex-col space-y-4',
    label: 'list view',
  },
  col2: {
    mode: 'col2',
    className: 'md:grid md:grid-cols-2 gap-4',
    label: '2 column view',
  },
  col3: {
    mode: 'col3',
    className: 'md:grid md:grid-cols-3 gap-4',
    label: '3 column view',
  },
  col4: {
    mode: 'col4',
    className: 'md:grid md:grid-cols-4 gap-4',
    label: '4 column view',
  },
};
const animation_variants = {
  container: {
    list: { transition: { staggerChildren: 0.02 } },
    col2: { transition: { staggerChildren: 0.1 } },
    col3: { transition: { staggerChildren: 0.15 } },
    col4: { transition: { staggerChildren: 0.2 } },
    auto: { transition: { staggerChildren: 0.25 } },
  },
  card: {
    hidden: { opacity: 0, y: 20 },
    visible: { opacity: 1, y: 0 },
  },
};
interface ToggleLayoutContextValue {
  modeIndex: number;
  setModeIndex: React.Dispatch<React.SetStateAction<number>>;
}
const ToggleLayoutContext = React.createContext<
  ToggleLayoutContextValue | undefined
>(undefined);
function useToggleLayoutContext() {
  const context = React.useContext(ToggleLayoutContext);
  if (context === undefined) {
    throw new Error(
      'useToggleLayoutContext must be used within a ToggleLayoutProvider',
    );
  }
  return context;
}
export function ToggleLayout({
  children,
  ...props
}: React.ComponentProps<typeof LayoutGroup>) {
  const [modeIndex, setModeIndex] = React.useState<number>(0);

  return (
    <ToggleLayoutContext.Provider value={{ modeIndex, setModeIndex }}>
      <LayoutGroup {...props}>{children}</LayoutGroup>
    </ToggleLayoutContext.Provider>
  );
}
export function ToggleLayoutContainer({
  className,
  ...props
}: HTMLMotionProps<'div'>) {
  const { modeIndex } = useToggleLayoutContext();
  const layout_config_values = [...Object.values(layout_config)];

  const currentConfig = layout_config_values[modeIndex];

  return (
    <motion.div
      layout
      variants={animation_variants.container}
      initial="hidden"
      animate={currentConfig.mode}
      className={cn(currentConfig.className, className)}
      {...props}
    />
  );
}

export function SelectLayoutGroup({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) {
  const { modeIndex, setModeIndex } = useToggleLayoutContext();
  const layout_config_values = [...Object.values(layout_config)];

  return (
    <div
      className={cn(
        'relative flex -space-x-px -space-y-px flex-col items-start md:flex-row justify-start   w-fit',
        className,
      )}
      {...props}
    >
      {layout_config_values.map((config, index) => (
        <div className="relative" id="layout-toggle-button" key={config.mode}>
          <button
            onClick={() => setModeIndex(index)}
            className="appearance-none border  bg-none text-nowrap text-sm px-2.5 font-medium py-2"
          >
            <span className="hover:underline-none">{config.label}</span>
          </button>
          {index === modeIndex && (
            <motion.div
              className="mix-blend-difference z-2 rounded-inherit absolute inset-0 bg-secondary"
              layoutId="layout-toggle-button"
            />
          )}
        </div>
      ))}
    </div>
  );
}

export function ToggleLayoutCell({ ...props }: HTMLMotionProps<'div'>) {
  return (
    <motion.div
      layout
      variants={animation_variants.card}
      initial="hidden"
      animate="visible"
      transition={{ type: 'spring', stiffness: 200, damping: 25 }}
      exit="hidden"
      {...props}
    />
  );
}

demo.tsx
import { ContainerToggle, CellToggle } from "@/components/blocks/animated-toggle-layout-container"
const PRODUCTS = [
  {
    id: "item-9",
    name: "adidas",
    imageUrl: "https://cdn.21st.dev/assets/mirror/73/7320ea5f4679ca3c77d4fc2ef5aa67ff446bcd5d6730c32ad98693051041a3ff.jpg",
    price: 120,
  },
  {
    id: "item-8",
    name: "nike",
    imageUrl: "https://cdn.21st.dev/assets/mirror/f9/f9d6f1ac5f0a79f6c8a2ec75d0a6d280481e69a625efac36a83e37407d5327b7.jpg",
    price: 120,
  },
  {
    id: "item-4",
    name: "brooks",
    imageUrl: "https://cdn.21st.dev/assets/mirror/2c/2c478b4da7a616a15e6be1593fffc874019ebead69b25d326b46f6c34da760ff.jpg",
    price: 95,
  },
  {
    id: "item-2",
    name: "nike",
    imageUrl: "https://cdn.21st.dev/assets/mirror/fc/fc4df115a545a004d0a1866d2975bff579195f6411787807b16f2c7b7e4c4eb4.jpg",
    price: 79.95,
  },
  {
    id: "item-5",
    name: "salomon",
    imageUrl: "https://cdn.21st.dev/assets/mirror/de/de7cd4fa278ccb802053c6a498b6b427a76e0ce373c38f2fd0a2bdad888cf921.jpg",
    price: 89.99,
  },
  {
    id: "item-7",
    name: "brooks",
    imageUrl: "https://cdn.21st.dev/assets/mirror/27/2747c67c386aa2b036b7574b61ae75ea3f33948fb20efe2329d356e8ebb6b3e2.jpg",
    price: 88,
  },
  {
    id: "item-1",
    name: "nike",
    imageUrl: "https://cdn.21st.dev/assets/mirror/f9/f99b6da6c79147688157a428b6d6121d87c689caefb982479a97b5d1f75398c7.jpg",
    price: 199.99,
  },
  {
    id: "item-6",
    name: "new balance",
    imageUrl: "https://cdn.21st.dev/assets/mirror/41/41a26e5db31cdfa971926a240aba75db68ae3d8f775f912d85732b8c5d392989.jpg",
    price: 70,
  },

  {
    id: "item-3",
    name: "under armour",
    imageUrl: "https://cdn.21st.dev/assets/mirror/30/30080492882c8a9d974dddc88a40b0fa2a0d26d78dea400f6e1e0a5580a1a86c.jpg",
    price: 85.99,
  },
]

export const LayoutToggleDemo = () => (
    <div className="p-12 md:px-8">
      <ContainerToggle className="bg-gray-50">
        {PRODUCTS.map((product) => (
          <CellToggle
            key={product.id}
            className="cursor-pointer space-y-4 overflow-hidden rounded-sm bg-white pb-6 shadow"
          >
            <div className="relative pb-8 pt-16"> 
              <img
                src={product.imageUrl}
                alt={product.name}
                className="mx-auto h-auto max-h-full max-w-[75%]"
              />
              <div className="absolute inset-0 z-10 bg-slate-950/5" />
            </div>
            <div className="flex items-center justify-between px-4">
              <h3 className="text-sm font-semibold capitalize tracking-tight">
                {product.name}
              </h3>
              <p className="text-xs tabular-nums leading-none tracking-tight text-slate-700">
                ${product.price}
              </p>
            </div>
          </CellToggle>
        ))}
      </ContainerToggle>
    </div>
)
```

Install NPM dependencies:
```bash
npm install motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
