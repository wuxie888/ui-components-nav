<!-- Stacked Cards Interaction · @chetanverma16 · https://21st.dev/@chetanverma16/components/stacked-cards-interaction
     license: MIT · category: gallery
     a simple stacked cards interaction component made with framer motion and tailwind css. -->

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
components/ui/stacked-cards-interaction.tsx
"use client";

import React, { useState } from "react";
import { motion } from "framer-motion";
import { cn } from "@/lib/utils";

interface CardProps {
  className?: string;
  image?: string;
  children?: React.ReactNode;
}

const Card = ({ className, image, children }: CardProps) => {
  return (
    <div
      className={cn(
        "w-[350px] cursor-pointer h-[400px] overflow-hidden bg-white rounded-2xl shadow-[0_0_10px_rgba(0,0,0,0.02)] border border-gray-200/80",
        className
      )}
    >
      {image && (
        <div className="relative h-72 rounded-xl shadow-lg overflow-hidden w-[calc(100%-1rem)] mx-2 mt-2">
          <img
            src={image}
            alt="card"
            className="object-cover mt-0 w-full h-full"
          />
        </div>
      )}
      {children && (
        <div className="px-4 p-2 flex flex-col gap-y-2">{children}</div>
      )}
    </div>
  );
};

interface CardData {
  image: string;
  title: string;
  description: string;
}

interface StackedCardsInteractionProps {
  cards: CardData[];
  spreadDistance?: number;
  rotationAngle?: number;
  animationDelay?: number;
}

const StackedCardsInteraction = ({
  cards,
  spreadDistance = 40,
  rotationAngle = 5,
  animationDelay = 0.1,
}: StackedCardsInteractionProps) => {
  const [isHovered, setIsHovered] = useState(false);

  const limitedCards = cards.slice(0, 3);

  return (
    <div className="relative w-full h-full flex items-center justify-center">
      <div className="relative w-[350px] h-[400px]">
        {limitedCards.map((card, index) => {
          const isFirst = index === 0;

          let xOffset = 0;
          let rotation = 0;

          if (limitedCards.length > 1) {
            if (index === 1) {
              xOffset = -spreadDistance;
              rotation = -rotationAngle;
            } else if (index === 2) {
              xOffset = spreadDistance;
              rotation = rotationAngle;
            }
          }

          return (
            <motion.div
              key={index}
              className={cn("absolute", isFirst ? "z-10" : "z-0")}
              initial={{ x: 0, rotate: 0 }}
              animate={{
                x: isHovered ? xOffset : 0,
                rotate: isHovered ? rotation : 0,
                zIndex: isFirst ? 10 : 0,
              }}
              transition={{
                duration: 0.3,
                ease: "easeInOut",
                delay: index * animationDelay,
                type: "spring",
              }}
              {...(isFirst && {
                onHoverStart: () => setIsHovered(true),
                onHoverEnd: () => setIsHovered(false),
              })}
            >
              <Card
                className={isFirst ? "z-10 cursor-pointer" : "z-0"}
                image={card.image}
              >
                <h2>{card.title}</h2>
                <p>{card.description}</p>
              </Card>
            </motion.div>
          );
        })}
      </div>
    </div>
  );
};

export { StackedCardsInteraction, Card };
export type { StackedCardsInteractionProps, CardData, CardProps };

demo.tsx
import { StackedCardsInteraction } from "@/components/ui/stacked-cards-interaction"

const StackedCardsInteractionDemo = () => {
    return(
          <StackedCardsInteraction
    cards={[
      {
        image:
          "https://cdn.21st.dev/assets/mirror/19/1991f46523d08e01b25feb3bd63dd633b6615aa1633c390d330d23ed5d4e151b.jpg",
        title: "Card 1",
        description: "This is the first card",
      },
      {
        image:
          "https://cdn.21st.dev/assets/mirror/47/4787cedc2d76c785a9dff04702b115561a9aedab29788f8ee5c331aba294013d.jpg",
        title: "Card 2",
        description: "This is the second card",
      },
      {
        image:
          "https://cdn.21st.dev/assets/mirror/e0/e069490e98dcec696dec62acb183ea27181575cc47857611011bcfc0004ee881.jpg",
        title: "Card 3",
        description: "This is the third card",
      },
    ]}
  />
    )
}

export { StackedCardsInteractionDemo }
```

Install NPM dependencies:
```bash
npm install framer-motion
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
