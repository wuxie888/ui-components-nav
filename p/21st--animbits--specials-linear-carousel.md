<!-- Linear Carousel · @animbits · https://21st.dev/@animbits/components/specials-linear-carousel
     license: no-license · category: gallery
     A smooth-scrolling horizontal 3D card carousel with infinite loop, drag-to-scroll, autoplay, and shared-element card expansion inspired by Linear. -->

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
components/ui/linear-carousel.tsx
"use client";

import React, {
    useEffect,
    useRef,
    useState,
    createContext,
    useContext,
} from "react";

import { cn } from "@/lib/utils";
import { motion } from "motion/react";
import type { ImgHTMLAttributes } from "react";
import { ChevronLeft, ChevronRight } from "lucide-react";

interface CarouselProps {
    items: React.JSX.Element[];
    initialScroll?: number;
}

type Card = {
    src: string;
    title: string;
    category?: string;
    content: React.ReactNode;
};

export const CarouselContext = createContext<{
    onCardClose: (index: number) => void;
    currentIndex: number;
}>({
    onCardClose: () => { },
    currentIndex: 0,
});

export const Carousel = ({
    items,
    initialScroll = 0,
    autoplay = false,
    autoplaySpeed = 0.5,
}: CarouselProps & { autoplay?: boolean; autoplaySpeed?: number }) => {
    const carouselRef = React.useRef<HTMLDivElement>(null);
    const [canScrollLeft, setCanScrollLeft] = React.useState(false);
    const [canScrollRight, setCanScrollRight] = React.useState(true);
    const [currentIndex, setCurrentIndex] = useState(0);
    const [isHovered, setIsHovered] = useState(false);
    const animationRef = useRef<number>(null);
    // Duplicate items to create infinite effect
    const loopedItems = [
        ...items,
        ...items.map((item) => React.cloneElement(item, { key: item.key + "-duplicate", index: items.indexOf(item) + items.length }))
    ];

    useEffect(() => {
        if (carouselRef.current) {
            carouselRef.current.scrollLeft = initialScroll;
            checkScrollability();
        }
    }, [initialScroll]);

    // Auto-scroll logic
    useEffect(() => {
        if (!autoplay || isHovered) {
            if (animationRef.current) cancelAnimationFrame(animationRef.current!);
            return;
        }

        const scroll = () => {
            if (carouselRef.current) {
                // Scroll by speed
                carouselRef.current.scrollLeft += autoplaySpeed;

                const scrollWidth = carouselRef.current.scrollWidth;

                if (carouselRef.current.scrollLeft >= scrollWidth / 2) {
                    carouselRef.current.scrollLeft = 0;
                }

                checkScrollability();
                animationRef.current = requestAnimationFrame(scroll);
            }
        };

        animationRef.current = requestAnimationFrame(scroll);

        return () => {
            if (animationRef.current) cancelAnimationFrame(animationRef.current);
        };
    }, [autoplay, autoplaySpeed, isHovered]);

    const checkScrollability = () => {
        if (carouselRef.current) {
            const { scrollLeft, scrollWidth, clientWidth } = carouselRef.current;
            setCanScrollLeft(scrollLeft > 0);
            setCanScrollRight(scrollLeft < scrollWidth - clientWidth);
        }
    };

    const scrollLeft = () => {
        if (carouselRef.current) {
            carouselRef.current.scrollTo({ left: 0, behavior: "smooth" });
        }
    };

    const scrollRight = () => {
        if (carouselRef.current) {
            const container = carouselRef.current;
            container.scrollTo({ left: container.scrollWidth, behavior: "smooth" });
        }
    };

    const handleCardClose = (index: number) => {
        if (carouselRef.current) {
            const cardWidth = isMobile() ? 230 : 320; // (md:w-80)
            const gap = isMobile() ? 4 : 8;
            const scrollPosition = (cardWidth + gap) * (index + 1);
            carouselRef.current.scrollTo({
                left: scrollPosition,
                behavior: "smooth",
            });
            setCurrentIndex(index);
        }
    };

    const isMobile = () => {
        return window && window.innerWidth < 768;
    };

    // Drag to scroll logic
    const [isDragging, setIsDragging] = useState(false);
    const [startX, setStartX] = useState(0);
    const [scrollLeftState, setScrollLeftState] = useState(0);

    const handleMouseDown = (e: React.MouseEvent) => {
        setIsDragging(true);
        setStartX(e.pageX - (carouselRef.current?.offsetLeft || 0));
        setScrollLeftState(carouselRef.current?.scrollLeft || 0);
    };

    const handleMouseLeave = () => {
        setIsDragging(false);
    };

    const handleMouseUp = () => {
        setIsDragging(false);
    };

    const handleMouseMove = (e: React.MouseEvent) => {
        if (!isDragging) return;
        e.preventDefault();
        const x = e.pageX - (carouselRef.current?.offsetLeft || 0);
        const walk = (x - startX) * 2; // Scroll-fast
        if (carouselRef.current) {
            carouselRef.current.scrollLeft = scrollLeftState - walk;
        }
    };


    return (
        <CarouselContext.Provider
            value={{ onCardClose: handleCardClose, currentIndex }}
        >
            <div
                className="relative w-full mx-auto px-4 md:px-8"
                onTouchStart={() => setIsHovered(true)}
                onTouchEnd={() => setIsHovered(false)}
            >
                <div
                    className={cn(
                        "flex w-full overflow-x-scroll overscroll-x-auto scroll-smooth py-10 [scrollbar-width:none] md:py-20 cursor-grab active:cursor-grabbing",
                        isDragging && "cursor-grabbing scroll-auto"
                    )}
                    ref={carouselRef}
                    onScroll={checkScrollability}
                    onMouseDown={handleMouseDown}
                    onMouseLeave={handleMouseLeave}
                    onMouseUp={handleMouseUp}
                    onMouseMove={handleMouseMove}
                >
                    <div
                        className={cn(
                            "absolute right-0 z-1000 h-auto w-[5%] overflow-hidden bg-gradient-to-l from-white dark:from-background to-transparent pointer-events-none"
                        )}
                    ></div>

                    <div
                        className={cn("flex flex-row justify-start gap-4")}
                    >
                        {loopedItems.map((item, index) => (
                            <motion.div
                                initial={{
                                    opacity: 0,
                                    y: 20,
                                }}
                                animate={{
                                    opacity: 1,
                                    y: 0,
                                }}
                                transition={{
                                    duration: 0.5,
                                    delay: 0.2 * (index % items.length), // Stagger only relevant to original set length ideally
                                    ease: "easeOut",
                                }}
                                key={"card" + index}
                                className="rounded-3xl"
                                onMouseEnter={() => setIsHovered(true)}
                                onMouseLeave={() => setIsHovered(false)}
                            >
                                {item}
                            </motion.div>
                        ))}
                    </div>
                </div>
                <div className="flex justify-center gap-3 mt-4">
                    <button
                        className="relative z-40 flex h-10 w-10 items-center justify-center rounded-full bg-card border border-border hover:bg-muted disabled:opacity-50 transition-colors"
                        onClick={scrollLeft}
                        disabled={!canScrollLeft}
                        onMouseEnter={() => setIsHovered(true)}
                        onMouseLeave={() => setIsHovered(false)}
                    >
                        <ChevronLeft className="h-6 w-6 text-muted-foreground" />
                    </button>
                    <button
                        className="relative z-40 flex h-10 w-10 items-center justify-center rounded-full bg-card border border-border hover:bg-muted disabled:opacity-50 transition-colors"
                        onClick={scrollRight}
                        disabled={!canScrollRight}
                        onMouseEnter={() => setIsHovered(true)}
                        onMouseLeave={() => setIsHovered(false)}
                    >
                        <ChevronRight className="h-6 w-6 text-muted-foreground" />
                    </button>
                </div>
            </div>
        </CarouselContext.Provider>
    );
};

export const Card = ({
    card,
    index,
    layout = false,
}: {
    card: Card;
    index: number;
    layout?: boolean;
}) => {
    return (
        <motion.button
            layoutId={layout ? `card-${card.title}-${index}` : undefined}
            className="relative z-10 flex h-60 w-56 flex-col items-start justify-end overflow-hidden rounded-3xl bg-gray-100 md:h-96 md:w-80 dark:bg-neutral-900"
        >
            <div className="pointer-events-none absolute inset-x-0 bottom-0 z-30 h-2/3 bg-gradient-to-t from-black/80 via-black/40 to-transparent" />
            <div className="relative z-40 p-8 w-full">
                {card.category && (
                    <motion.p
                        layoutId={layout ? `category-${card.category}-${index}` : undefined}
                        className="text-left font-mono text-sm font-medium text-white md:text-base"
                    >
                        {card.category}
                    </motion.p>
                )}
                <motion.p
                    layoutId={layout ? `title-${card.title}-${index}` : undefined}
                    className="mt-2 max-w-xs text-left font-mono text-xl font-semibold [text-wrap:balance] text-white md:text-3xl"
                >
                    {card.title}
                </motion.p>
            </div>
            <img
                src={card.src}
                alt={card.title}
                className="absolute inset-0 z-10 w-full h-full object-cover grayscale"
            />
        </motion.button>
    );
};

export const BlurImage = ({
    height,
    width,
    src,
    className,
    alt,
    ...rest
}: ImgHTMLAttributes<HTMLImageElement> & { src: string; alt: string }) => {
    const [isLoading, setLoading] = useState(true);
    return (
        <img
            className={cn(
                "h-full w-full transition duration-300",
                isLoading ? "blur-sm" : "blur-0",
                className
            )}
            onLoad={() => setLoading(false)}
            src={src as string}
            width={width}
            height={height}
            loading="lazy"
            decoding="async"
            alt={alt ? alt : "Background of a beautiful view"}
            {...rest}
        />
    );
};

demo.tsx
import { Carousel, Card } from "@/components/ui/specials-linear-carousel";

const images = [
  {
    src: "https://cdn.21st.dev/assets/mirror/46/4640404badd4692a0956c124cad3f90ab4a01acb804c038caddddea9a98040f6.jpg",
    title: "Mountain Views",
    category: "Nature",
    content: (
      <p className="text-neutral-500">
        Breathtaking mountain landscapes from around the world.
      </p>
    ),
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/d0/d04cde6d6c11fae1caf86bf46e1ad92a26359da51787c6816d65ea328a6bb5b6.jpg",
    title: "Ocean Blue",
    category: "Nature",
    content: <p className="text-neutral-500">Calm and serene ocean views.</p>,
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/43/43a3af0c82593e850e9effa52335cd95ded0190f26efd68f24c7e37a38e28d57.jpg",
    title: "Forest Pathways",
    category: "Nature",
    content: (
      <p className="text-neutral-500">
        Mysterious paths through dense forests.
      </p>
    ),
  },
  {
    src: "https://cdn.21st.dev/assets/mirror/a2/a22bbbfe8bc239d8b456887dda7b9f733a81752830a3184dbbee181b5b71a2f9.jpg",
    title: "Solitude",
    category: "Nature",
    content: (
      <p className="text-neutral-500">
        Finding peace in the vastness of nature.
      </p>
    ),
  },
];

export default function LinearCarouselDemo() {
  const cards = images.map((card, index) => (
    <Card key={card.src} card={card} index={index} />
  ));

  return (
    <div className="w-full h-full py-20">
      <Carousel items={cards} />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install clsx lucide-react motion tailwind-merge
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
