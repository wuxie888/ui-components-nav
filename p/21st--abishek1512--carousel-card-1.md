<!-- Carousel card · @abishek1512 · https://21st.dev/@abishek1512/components/carousel-card-1
     license: MIT · category: gallery
     UI component for card with carousel effects -->

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
components/ui/carousel-card.tsx
"use client";

import React, { useEffect, useRef, useState } from "react";

export interface CardItem {
  id: number;
  imgUrl: string;
  content: string;
}

export interface CardProps {
  data: CardItem[];
  showCarousel?: boolean;
  cardsPerView?: number;
}

const Card = ({ data, showCarousel = true, cardsPerView = 3 }: CardProps) => {
  const [currentIndex, setCurrentIndex] = useState(0);
  const [isSingleCard, setIsSingleCard] = useState(false);
  const [isAnimating, setIsAnimating] = useState(false);
  const trackRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    setIsSingleCard(data.length === 1);
  }, [data]);

  // NOTE: bundle computes the slide distance as 75 / cardsPerView (percent of the track width)
  const slideWidth = 75 / cardsPerView;

  const handleNext = () => {
    if (isAnimating || !showCarousel || data.length <= cardsPerView) return;
    setIsAnimating(true);

    const nextIndex = (currentIndex + 1) % data.length;

    if (trackRef.current) {
      trackRef.current.style.transition = "transform 500ms ease";
      trackRef.current.style.transform = `translateX(-${slideWidth}%)`;

      setTimeout(() => {
        setCurrentIndex(nextIndex);
        if (trackRef.current) {
          trackRef.current.style.transition = "none";
          trackRef.current.style.transform = "translateX(0)";
          // Force reflow
          void trackRef.current.offsetWidth;
          setIsAnimating(false);
        }
      }, 500);
    }
  };

  const handlePrev = () => {
    if (isAnimating || !showCarousel || data.length <= cardsPerView) return;
    setIsAnimating(true);

    const prevIndex = (currentIndex - 1 + data.length) % data.length;

    if (trackRef.current) {
      trackRef.current.style.transition = "none";
      trackRef.current.style.transform = `translateX(-${slideWidth}%)`;
      setCurrentIndex(prevIndex);
      // Force reflow
      void trackRef.current.offsetWidth;
      trackRef.current.style.transition = "transform 500ms ease";
      trackRef.current.style.transform = "translateX(0)";

      setTimeout(() => {
        setIsAnimating(false);
      }, 500);
    }
  };

  const getVisibleCards = () => {
    if (!showCarousel) return data;

    const visible: CardItem[] = [];
    const total = data.length;
    for (let i = 0; i < cardsPerView + 1; i++) {
      const index = (currentIndex + i) % total;
      visible.push(data[index]);
    }
    return visible;
  };

  return (
    <div className="w-full px-4">
      <div className={`relative ${isSingleCard ? "max-w-sm mx-auto" : "w-full"}`}>
        {showCarousel && data.length > cardsPerView && (
          <>
            <button
              onClick={handlePrev}
              className="absolute left-0 top-1/2 -translate-y-1/2 z-10 bg-black/50 text-white p-2 rounded-full hover:bg-black/70 transition-all duration-300"
              disabled={isAnimating}
              aria-label="Previous slide"
            >
              ←
            </button>
            <button
              onClick={handleNext}
              className="absolute right-0 top-1/2 -translate-y-1/2 z-10 bg-black/50 text-white p-2 rounded-full hover:bg-black/70 transition-all duration-300"
              disabled={isAnimating}
              aria-label="Next slide"
            >
              →
            </button>
          </>
        )}

        <div className="overflow-hidden">
          <div
            ref={trackRef}
            className="flex"
            style={{
              transform: "translateX(0)",
              width: showCarousel
                ? `${((cardsPerView + 1) * 100) / cardsPerView}%`
                : "100%",
            }}
          >
            {getVisibleCards().map((card, index) => (
              <div
                key={`card-${currentIndex}-${index}`}
                style={{
                  width: showCarousel
                    ? `${100 / (cardsPerView + 1)}%`
                    : `${100 / Math.min(cardsPerView, data.length)}%`,
                }}
                className="px-2"
              >
                <div className="relative overflow-hidden rounded-lg shadow-md group h-full">
                  <div className="w-full h-64">
                    <img
                      src={card.imgUrl}
                      alt=""
                      className="w-full h-full object-cover transition-transform duration-300 group-hover:scale-105"
                    />
                  </div>
                  <div className="absolute inset-0 bg-black/80 text-white p-4 transition-transform duration-300 transform translate-y-full group-hover:translate-y-0 overflow-y-auto">
                    <p className="text-sm">{card.content}</p>
                  </div>
                </div>
              </div>
            ))}
          </div>
        </div>
      </div>
    </div>
  );
};

export default Card;

demo.tsx
import Card from "@/components/ui/carousel-card";

const CARD_DATA = [
  {
    id: 1,
    imgUrl: 'https://cdn.21st.dev/assets/mirror/32/327d86ff3c8677219d40f97a247835c0891937f937552d2b325ddd63b624ad52.jpg',
    content:
      'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce ultrices dolor ac massa maximus, blandit ullamcorper eros accumsan. Sed facilisis lacinia venenatis. Donec bibendum, eros ut porttitor consectetur, enim sapien vehicula mi, et consequat lacus turpis vel dolor. Vestibulum sagittis facilisis ipsum vitae suscipit. Proin in nisl sollicitudin, interdum erat eu, consequat odio. Sed auctor felis ac lorem molestie, a cursus elit malesuada. Etiam et varius erat. Aliquam pharetra convallis aliquet. Vestibulum eros ipsum, sodales ac imperdiet id, pellentesque sed tortor.',
  },
  {
    id: 2,
    imgUrl: 'https://cdn.21st.dev/assets/mirror/1c/1c98bce1616253e23af6f690181e6c2c84968ce51ecb9512e287920ec97c336a.jpg',
    content:
      'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce ultrices dolor ac massa maximus, blandit ullamcorper eros accumsan. Sed facilisis lacinia venenatis. Donec bibendum, eros ut porttitor consectetur, enim sapien vehicula mi, et consequat lacus turpis vel dolor. Vestibulum sagittis facilisis ipsum vitae suscipit. Proin in nisl sollicitudin, interdum erat eu, consequat odio. Sed auctor felis ac lorem molestie, a cursus elit malesuada. Etiam et varius erat. Aliquam pharetra convallis aliquet. Vestibulum eros ipsum, sodales ac imperdiet id, pellentesque sed tortor.',
  },
  {
    id: 3,
    imgUrl: 'https://cdn.21st.dev/assets/mirror/1c/1cfeaad844afe4dafa6c97fc14bff442b61f17a95e195e6b97638f380b018b01.jpg',
    content:
      'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce ultrices dolor ac massa maximus, blandit ullamcorper eros accumsan. Sed facilisis lacinia venenatis. Donec bibendum, eros ut porttitor consectetur, enim sapien vehicula mi, et consequat lacus turpis vel dolor. Vestibulum sagittis facilisis ipsum vitae suscipit. Proin in nisl sollicitudin, interdum erat eu, consequat odio. Sed auctor felis ac lorem molestie, a cursus elit malesuada. Etiam et varius erat. Aliquam pharetra convallis aliquet. Vestibulum eros ipsum, sodales ac imperdiet id, pellentesque sed tortor.',
  },
  {
    id: 4,
    imgUrl: 'https://cdn.21st.dev/assets/mirror/72/72dd368074e81932714c055cdda66d748cbd3e54716717a0f566087c25ffac31.jpg',
    content:
      'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce ultrices dolor ac massa maximus, blandit ullamcorper eros accumsan. Sed facilisis lacinia venenatis. Donec bibendum, eros ut porttitor consectetur, enim sapien vehicula mi, et consequat lacus turpis vel dolor. Vestibulum sagittis facilisis ipsum vitae suscipit. Proin in nisl sollicitudin, interdum erat eu, consequat odio. Sed auctor felis ac lorem molestie, a cursus elit malesuada. Etiam et varius erat. Aliquam pharetra convallis aliquet. Vestibulum eros ipsum, sodales ac imperdiet id, pellentesque sed tortor.',
  },
  {
    id: 5,
    imgUrl: 'https://cdn.21st.dev/assets/mirror/61/61fcb9b5b171e9aca51c658ffd3b0f4d61d1efea33a3acb80e1e5bffb7ecf740.jpg',
    content:
      'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce ultrices dolor ac massa maximus, blandit ullamcorper eros accumsan. Sed facilisis lacinia venenatis. Donec bibendum, eros ut porttitor consectetur, enim sapien vehicula mi, et consequat lacus turpis vel dolor. Vestibulum sagittis facilisis ipsum vitae suscipit. Proin in nisl sollicitudin, interdum erat eu, consequat odio. Sed auctor felis ac lorem molestie, a cursus elit malesuada. Etiam et varius erat. Aliquam pharetra convallis aliquet. Vestibulum eros ipsum, sodales ac imperdiet id, pellentesque sed tortor.',
  },
  {
    id: 6,
    imgUrl: 'https://cdn.21st.dev/assets/mirror/58/582a4459bcc56fecf623fef73a326cfa18ae0b963fe4972980b82bdec7ac9185.jpg',
    content:
      'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce ultrices dolor ac massa maximus, blandit ullamcorper eros accumsan. Sed facilisis lacinia venenatis. Donec bibendum, eros ut porttitor consectetur, enim sapien vehicula mi, et consequat lacus turpis vel dolor. Vestibulum sagittis facilisis ipsum vitae suscipit. Proin in nisl sollicitudin, interdum erat eu, consequat odio. Sed auctor felis ac lorem molestie, a cursus elit malesuada. Etiam et varius erat. Aliquam pharetra convallis aliquet. Vestibulum eros ipsum, sodales ac imperdiet id, pellentesque sed tortor.',
  },
];

const DemoOne = () => {
  return (
    <div className="flex w-full h-screen justify-center items-center">
      <Card data={CARD_DATA}/>
    </div>
  );
};

export { DemoOne };
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
