<!-- Shop Navigation Menu · @bundui · https://21st.dev/@bundui/components/navigation-menu4
     license: no-license · category: navigation-menu
     An e-commerce navigation menu with dropdown panels for collections and accessories, featuring featured imagery, category icons, and quick links. -->

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
components/ui/icons.tsx
import * as React from "react";

export const BagIcon = (props: React.SVGProps<SVGSVGElement>) => {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="24"
      height="24"
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      stroke-width="1"
      stroke-linecap="round"
      stroke-linejoin="round"
      {...props}>
      <path stroke="none" d="M0 0h24v24H0z" fill="none" />
      <path d="M3 7m0 2a2 2 0 0 1 2 -2h14a2 2 0 0 1 2 2v9a2 2 0 0 1 -2 2h-14a2 2 0 0 1 -2 -2z" />
      <path d="M8 7v-2a2 2 0 0 1 2 -2h4a2 2 0 0 1 2 2v2" />
      <path d="M12 12l0 .01" />
      <path d="M3 13a20 20 0 0 0 18 0" />
    </svg>
  );
};

export const JewelryIcon = (props: React.SVGProps<SVGSVGElement>) => {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="24"
      height="24"
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      stroke-width="1"
      stroke-linecap="round"
      stroke-linejoin="round"
      {...props}>
      <path stroke="none" d="M0 0h24v24H0z" fill="none" />
      <path d="M6 5h12l3 5l-8.5 9.5a.7 .7 0 0 1 -1 0l-8.5 -9.5l3 -5" />
      <path d="M10 12l-2 -2.2l.6 -1" />
    </svg>
  );
};

export const SunglassesIcon = (props: React.SVGProps<SVGSVGElement>) => {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="24"
      height="24"
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      stroke-width="1"
      stroke-linecap="round"
      stroke-linejoin="round"
      {...props}>
      <path stroke="none" d="M0 0h24v24H0z" fill="none" />
      <path d="M8 3h-2l-3 9" />
      <path d="M16 3h2l3 9" />
      <path d="M3 12v7a1 1 0 0 0 1 1h4.586a1 1 0 0 0 .707 -.293l2 -2a1 1 0 0 1 1.414 0l2 2a1 1 0 0 0 .707 .293h4.586a1 1 0 0 0 1 -1v-7h-18z" />
      <path d="M7 16h1" />
      <path d="M16 16h1" />
    </svg>
  );
};

export const HatIcon = (props: React.SVGProps<SVGSVGElement>) => {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="24"
      height="24"
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      stroke-width="1"
      stroke-linecap="round"
      stroke-linejoin="round"
      {...props}>
      <path stroke="none" d="M0 0h24v24H0z" fill="none" />
      <path d="M6 10.5l1.436 -4c.318 -.876 .728 -1.302 1.359 -1.302c.219 0 1.054 .365 1.88 .583c.825 .219 .733 -.329 .908 -.487c.176 -.158 .355 -.294 .61 -.294c.242 0 .553 .048 1.692 .448c.759 .267 1.493 .574 2.204 .922c1.175 .582 1.426 .913 1.595 1.507l.816 4.623c2.086 .898 3.5 2.357 3.5 3.682c0 1.685 -1.2 3.818 -5.957 3.818c-6.206 0 -14.043 -4.042 -14.043 -7.32c0 -1.044 1.333 -1.77 4 -2.18z" />
      <path d="M6 10.5c0 .969 4.39 3.5 9.5 3.5c1.314 0 3 .063 3 -1.5" />
    </svg>
  );
};

export const BeltIcon = (props: React.SVGProps<SVGSVGElement>) => {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="24"
      height="24"
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      stroke-width="1"
      stroke-linecap="round"
      stroke-linejoin="round"
      {...props}>
      <path stroke="none" d="M0 0h24v24H0z" fill="none" />
      <path d="M7 17m-3 0a3 3 0 1 0 6 0a3 3 0 1 0 -6 0" />
      <path d="M17 17m-3 0a3 3 0 1 0 6 0a3 3 0 1 0 -6 0" />
      <path d="M9.15 14.85l8.85 -10.85" />
      <path d="M6 4l8.85 10.85" />
    </svg>
  );
};

export const OtherIcon = (props: React.SVGProps<SVGSVGElement>) => {
  return (
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="24"
      height="24"
      viewBox="0 0 24 24"
      fill="none"
      stroke="currentColor"
      stroke-width="1"
      stroke-linecap="round"
      stroke-linejoin="round"
      {...props}>
      <path stroke="none" d="M0 0h24v24H0z" fill="none" />
      <path d="M6 4a4 4 0 0 1 4 3.8l0 .2v10.5a1.5 1.5 0 0 1 -3 0v-6.5h-1a4 4 0 0 1 -4 -3.8l0 -.2a4 4 0 0 1 4 -4z" />
      <path d="M18 4a4 4 0 0 0 -4 3.8l0 .2v10.5a1.5 1.5 0 0 0 3 0v-6.5h1a4 4 0 0 0 4 -3.8l0 -.2a4 4 0 0 0 -4 -4z" />
    </svg>
  );
};

components/ui/index.tsx
"use client";

import * as React from "react";
import Link from "next/link";

import {
  NavigationMenu,
  NavigationMenuContent,
  NavigationMenuItem,
  NavigationMenuLink,
  NavigationMenuList,
  NavigationMenuTrigger,
  navigationMenuTriggerStyle,
} from "@/components/ui/navigation-menu";
import {
  BagIcon,
  BeltIcon,
  HatIcon,
  JewelryIcon,
  OtherIcon,
  SunglassesIcon,
} from "./icons";

type ListItemType = {
  title: string;
  href?: string;
  description?: string;
  icon?: React.ComponentType<React.SVGProps<SVGSVGElement>>;
};

const accessoriesMenuItems: ListItemType[] = [
  {
    title: "Bags",
    href: "#",
    icon: BagIcon,
  },
  {
    title: "Jewelry",
    href: "#",
    icon: JewelryIcon,
  },
  {
    title: "Sunglasses",
    href: "#",
    icon: SunglassesIcon,
  },
  {
    title: "Hats & Beanies",
    href: "#",
    icon: HatIcon,
  },
  {
    title: "Belts",
    href: "#",
    icon: BeltIcon,
  },
  {
    title: "All Accessories",
    href: "#",
    icon: OtherIcon,
  },
];

const collectionItems = [
  {
    title: "Trends",
    href: "#",
    description: "Discover this summer's trendy products.",
  },
  {
    title: "Best Sellers",
    href: "#",
    description: "We've collected the best-selling products for you.",
  },
  {
    title: "New Arrivals",
    href: "#",
    description: "Discover the most favorited products.",
  },
];

export default function NavigationMenuDemo() {
  return (
    <NavigationMenu>
      <NavigationMenuList>
        <NavigationMenuItem>
          <NavigationMenuTrigger>Collections</NavigationMenuTrigger>
          <NavigationMenuContent>
            <ul className="grid gap-1 md:w-[400px] lg:w-[600px] lg:grid-cols-2">
              {collectionItems.map((item, i) => (
                <li key={i}>
                  <NavigationMenuLink
                    asChild
                    className="flex-col items-start gap-1"
                  >
                    <Link
                      href={item.href ?? "#"}
                      onClick={(e) => e.preventDefault()}
                    >
                      <div className="text-sm leading-none font-medium">
                        {item.title}
                      </div>
                      <p className="text-muted-foreground line-clamp-2 text-sm leading-snug">
                        {item.description}
                      </p>
                    </Link>
                  </NavigationMenuLink>
                </li>
              ))}
              <li className="col-start-2 row-span-3 row-start-1">
                <NavigationMenuLink
                  asChild
                  className="h-full flex-col items-start gap-2"
                >
                  <Link href="#" onClick={(e) => e.preventDefault()}>
                    <img
                      src="https://images.unsplash.com/photo-1512436991641-6745cdb1723f?auto=format&fit=crop&w=600&q=80"
                      alt="Clothes on a rack"
                      className="aspect-4/3 w-full rounded-md object-cover"
                    />
                    <div className="space-y-1">
                      <div className="text-sm leading-none font-medium">
                        Timeless Classics
                      </div>
                      <p className="text-muted-foreground line-clamp-2 text-sm leading-snug">
                        Elevate your style with essentials
                      </p>
                    </div>
                  </Link>
                </NavigationMenuLink>
              </li>
            </ul>
          </NavigationMenuContent>
        </NavigationMenuItem>
        <NavigationMenuItem>
          <NavigationMenuTrigger>Accessories</NavigationMenuTrigger>
          <NavigationMenuContent>
            <ul className="grid w-[400px] grid-cols-2 gap-1 lg:w-[300px]">
              {accessoriesMenuItems.map((item, i) => (
                <li key={i}>
                  <NavigationMenuLink
                    asChild
                    className="flex-col items-center gap-2 p-4 text-center"
                  >
                    <Link
                      href={item.href ?? "#"}
                      onClick={(e) => e.preventDefault()}
                    >
                      {item.icon ? (
                        <item.icon className="text-muted-foreground size-8" />
                      ) : null}
                      <span className="text-sm leading-none font-medium">
                        {item.title}
                      </span>
                    </Link>
                  </NavigationMenuLink>
                </li>
              ))}
            </ul>
          </NavigationMenuContent>
        </NavigationMenuItem>
        <NavigationMenuItem>
          <NavigationMenuLink asChild>
            <Link
              href="#"
              className={navigationMenuTriggerStyle()}
              onClick={(e) => e.preventDefault()}
            >
              Women
            </Link>
          </NavigationMenuLink>
        </NavigationMenuItem>
        <NavigationMenuItem>
          <NavigationMenuLink asChild>
            <Link
              href="#"
              className={navigationMenuTriggerStyle()}
              onClick={(e) => e.preventDefault()}
            >
              Men
            </Link>
          </NavigationMenuLink>
        </NavigationMenuItem>
      </NavigationMenuList>
    </NavigationMenu>
  );
}

demo.tsx
"use client";

import * as React from "react";
import Link from "next/link";

import {
  NavigationMenu,
  NavigationMenuContent,
  NavigationMenuItem,
  NavigationMenuLink,
  NavigationMenuList,
  NavigationMenuTrigger,
  navigationMenuTriggerStyle,
} from "@/components/ui/navigation-menu";
import {
  BagIcon,
  BeltIcon,
  HatIcon,
  JewelryIcon,
  OtherIcon,
  SunglassesIcon,
} from "@/components/ui/navigation-menu4-utils/icons";

const accessoriesMenuItems = [
  { title: "Bags", href: "#", icon: BagIcon },
  { title: "Jewelry", href: "#", icon: JewelryIcon },
  { title: "Sunglasses", href: "#", icon: SunglassesIcon },
  { title: "Hats & Beanies", href: "#", icon: HatIcon },
  { title: "Belts", href: "#", icon: BeltIcon },
  { title: "All Accessories", href: "#", icon: OtherIcon },
];

const collectionItems = [
  {
    title: "Trends",
    href: "#",
    description: "Discover this summer's trendy products.",
  },
  {
    title: "Best Sellers",
    href: "#",
    description: "We've collected the best-selling products for you.",
  },
  {
    title: "New Arrivals",
    href: "#",
    description: "Discover the most favorited products.",
  },
];

export default function NavigationMenuShopDemo() {
  return (
    <div className="bg-background text-foreground flex min-h-[26rem] w-full items-start justify-center p-8">
      <NavigationMenu defaultValue="collections">
        <NavigationMenuList>
          <NavigationMenuItem value="collections">
            <NavigationMenuTrigger>Collections</NavigationMenuTrigger>
            <NavigationMenuContent>
              <ul className="grid w-[600px] gap-0 md:w-[500px] lg:grid-cols-2">
                {collectionItems.map((item, i) => (
                  <Link
                    key={i}
                    href={item.href}
                    className="hover:bg-accent hover:text-accent-foreground focus:bg-accent focus:text-accent-foreground gap-2 space-y-1 rounded-md p-3 leading-none no-underline transition-colors outline-none select-none"
                    onClick={(e) => e.preventDefault()}
                  >
                    <div className="text-sm leading-none font-medium">
                      {item.title}
                    </div>
                    <p className="text-muted-foreground line-clamp-2 text-sm leading-snug">
                      {item.description}
                    </p>
                  </Link>
                ))}
                <li className="col-start-2 row-span-3 row-start-1">
                  <NavigationMenuLink asChild>
                    <Link
                      href="#"
                      className="block space-y-2"
                      onClick={(e) => e.preventDefault()}
                    >
                      <img
                        src="https://cdn.21st.dev/assets/mirror/67/67b67e4f15d38d2832ed1345e5f8da0705064c9c6699f52007a548c345c455ce.jpg"
                        alt="Timeless Classics"
                        className="aspect-4/3 w-full rounded object-cover"
                      />
                      <div className="space-y-1">
                        <div className="text-sm leading-none font-medium">
                          Timeless Classics
                        </div>
                        <p className="text-muted-foreground line-clamp-2 text-sm leading-snug">
                          Elevate your style with essentials
                        </p>
                      </div>
                    </Link>
                  </NavigationMenuLink>
                </li>
              </ul>
            </NavigationMenuContent>
          </NavigationMenuItem>
          <NavigationMenuItem value="accessories">
            <NavigationMenuTrigger>Accessories</NavigationMenuTrigger>
            <NavigationMenuContent>
              <ul className="grid w-[400px] list-none grid-cols-2 gap-3 lg:w-[300px]">
                {accessoriesMenuItems.map((item, i) => (
                  <NavigationMenuLink asChild key={i}>
                    <Link
                      href={item.href}
                      className="hover:bg-accent hover:text-accent-foreground focus:bg-accent focus:text-accent-foreground flex flex-col items-center justify-center gap-2 rounded-md p-3 text-center leading-none no-underline transition-colors outline-none select-none"
                      onClick={(e) => e.preventDefault()}
                    >
                      <item.icon className="text-muted-foreground mx-auto size-8" />
                      <span className="block text-sm leading-none font-medium">
                        {item.title}
                      </span>
                    </Link>
                  </NavigationMenuLink>
                ))}
              </ul>
            </NavigationMenuContent>
          </NavigationMenuItem>
          <NavigationMenuItem>
            <NavigationMenuLink asChild>
              <Link
                href="#"
                className={navigationMenuTriggerStyle()}
                onClick={(e) => e.preventDefault()}
              >
                Women
              </Link>
            </NavigationMenuLink>
          </NavigationMenuItem>
          <NavigationMenuItem>
            <NavigationMenuLink asChild>
              <Link
                href="#"
                className={navigationMenuTriggerStyle()}
                onClick={(e) => e.preventDefault()}
              >
                Men
              </Link>
            </NavigationMenuLink>
          </NavigationMenuItem>
        </NavigationMenuList>
      </NavigationMenu>
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
npx shadcn@latest add badge button navigation-menu separator sheet
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
