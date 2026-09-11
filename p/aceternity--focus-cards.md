<!-- Focus Cards · Aceternity UI · https://ui.aceternity.com/components/focus-cards
     license: MIT · category: gallery
     Hover over the card to focus on it, blurring the rest of the cards. -->

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
components/ui/focus-cards.tsx
"use client";

import React, { useState } from "react";
import { cn } from "@/lib/utils";

export const Card = React.memo(
  ({
    card,
    index,
    hovered,
    setHovered,
  }: {
    card: any;
    index: number;
    hovered: number | null;
    setHovered: React.Dispatch<React.SetStateAction<number | null>>;
  }) => (
    <div
      onMouseEnter={() => setHovered(index)}
      onMouseLeave={() => setHovered(null)}
      className={cn(
        "rounded-lg relative bg-gray-100 dark:bg-neutral-900 overflow-hidden h-60 md:h-96 w-full transition-all duration-300 ease-out",
        hovered !== null && hovered !== index && "blur-sm scale-[0.98]"
      )}
    >
      <img
        src={card.src}
        alt={card.title}
        className="object-cover absolute inset-0"
      />
      <div
        className={cn(
          "absolute inset-0 bg-black/50 flex items-end py-8 px-4 transition-opacity duration-300",
          hovered === index ? "opacity-100" : "opacity-0"
        )}
      >
        <div className="text-xl md:text-2xl font-medium bg-clip-text text-transparent bg-gradient-to-b from-neutral-50 to-neutral-200">
          {card.title}
        </div>
      </div>
    </div>
  )
);

Card.displayName = "Card";

type Card = {
  title: string;
  src: string;
};

export function FocusCards({ cards }: { cards: Card[] }) {
  const [hovered, setHovered] = useState<number | null>(null);

  return (
    <div className="grid grid-cols-1 md:grid-cols-3 gap-10 max-w-5xl mx-auto md:px-8 w-full">
      {cards.map((card, index) => (
        <Card
          key={card.title}
          card={card}
          index={index}
          hovered={hovered}
          setHovered={setHovered}
        />
      ))}
    </div>
  );
}

demo.tsx
import { FocusCards } from "@/components/ui/focus-cards";

export function FocusCardsDemo() {
  const cards = [
    {
      title: "Forest Adventure",
      src: "https://cdn.21st.dev/assets/mirror/0b/0b19c5c1fabaec64306b1368da5b246f8ccd4d28d538ce6fea29d47374c0abc0.jpg",
    },
    {
      title: "Valley of life",
      src: "https://cdn.21st.dev/assets/mirror/c4/c43517f5768dc5dc981f7dbb4a91e549679a8bd9d3c81b0a6ad639d25bc930f1.jpg",
    },
    {
      title: "Sala behta hi jayega",
      src: "https://cdn.21st.dev/assets/mirror/77/773e007a928155eaec093cec79f4cd2adb718b6fb50b5b5469623d8b1492b076.jpg",
    },
    {
      title: "Camping is for pros",
      src: "https://cdn.21st.dev/assets/mirror/b0/b0a3b9eff623f3779d3543114c7a128d6269c7decb5aea47f5738fae078d37ec.jpg",
    },
    {
      title: "The road not taken",
      src: "https://cdn.21st.dev/assets/mirror/23/2397ae82f36c0da8c02a48ef12d073eca7c785c062c799add5329f4d7836423b.jpg",
    },
    {
      title: "The First Rule",
      src: "https://cdn.21st.dev/assets/mirror/55/55322fc4f6b2cf723261097a093416ebc513dba7d9c39173015e533c71695dbd.png",
    },
  ];

  return <FocusCards cards={cards} />;
}
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
