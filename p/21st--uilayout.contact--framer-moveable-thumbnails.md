<!-- Framer Moveable Thumbnails · @uilayout.contact · https://21st.dev/@uilayout.contact/components/framer-moveable-thumbnails
     license: no-license · category: gallery
     A draggable image carousel with an animated filmstrip of thumbnails where the active thumbnail expands to a wide aspect ratio. -->

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
components/ui/framer-moveable-thumnbails.tsx
'use client';
import {
  AnimatePresence,
  animate,
  motion,
  useMotionTemplate,
  useMotionValue,
  useSpring,
} from 'motion/react';
import React, { useEffect, useRef, useState } from 'react';

export const items = [
  {
    id: 1,
    url: 'https://images.unsplash.com/photo-1761882835101-02ab45ac0726?ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&q=80&w=690',
    title: 'MAXX PHAM',
  },
  {
    id: 2,
    url: 'https://images.unsplash.com/photo-1661980494567-40a5e01b699b?ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&q=80&w=685',
    title: 'BOXIEN BAY',
  },
  {
    id: 3,
    url: 'https://images.unsplash.com/photo-1761882725885-d3d8bd2032d1?ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&q=80&w=687',
    title: 'AUSIZE MAM',
  },
  {
    id: 4,
    url: 'https://images.unsplash.com/photo-1761775915848-467e41c1c4db?ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&q=80&w=689',
    title: 'RECLKTIKA',
  },
  {
    id: 5,
    url: 'https://images.unsplash.com/photo-1761078980679-e89e25fe279b?ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&q=80&w=687',
    title: 'SONYPOO',
  },
  {
    id: 6,
    url: 'https://images.unsplash.com/photo-1760389005000-bf02bf24f463?ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&q=80&w=1123',
    title: 'DONM FLY',
  },
  {
    id: 7,
    url: 'https://images.unsplash.com/photo-1761165307495-56bd564d322f?ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&q=80&w=663',
    title: 'Snowy Mountain Highway',
  },
  {
    id: 8,
    url: 'https://images.unsplash.com/photo-1756299792672-157811bf1005?ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&q=80&w=1074',
    title: 'FOGGY FOLS',
  },
  {
    id: 9,
    url: 'https://images.unsplash.com/photo-1572851899646-a1f69c664e1e?ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&q=80&w=1170',
    title: 'DIM DARKO',
  },
  {
    id: 10,
    url: 'https://images.unsplash.com/photo-1759247178379-0e8eba83a4a6?ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&q=80&w=687',
    title: 'BEALIVE',
  },
  {
    id: 11,
    url: 'https://images.unsplash.com/photo-1754968230523-052635c98f99?ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&q=80&w=736',
    title: 'DOMEDOM ROME',
  },
  {
    id: 12,
    url: 'https://images.unsplash.com/photo-1643037508102-46fb319979c5?ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&q=80&w=764',
    title: 'IKEIMON POVE',
  },
  {
    id: 13,
    url: 'https://images.unsplash.com/photo-1555803741-1ac759ac2f53?q=80&w=880&auto=format&fit=crop',
    title: 'Wildflower Mountain Meadow',
  },
  {
    id: 14,
    url: 'https://images.unsplash.com/photo-1516705486637-7b01bf9b9d13?q=80&w=880&auto=format&fit=crop',
    title: 'Mountain Valley Vista',
  },
  {
    id: 15,
    url: 'https://images.unsplash.com/photo-1512045519129-eb9ceb788555?q=80&w=880&auto=format&fit=crop',
    title: 'Rugged Mountain Terrain',
  },
  {
    id: 16,
    url: 'https://images.unsplash.com/photo-1504198266287-1659872e6590?q=80&w=880&auto=format&fit=crop',
    title: 'Mountain Wildflower Bloom',
  },
  {
    id: 17,
    url: 'https://images.unsplash.com/photo-1611582450053-0f056a82a68e?q=80&w=735&auto=format&fit=crop',
    title: 'Mountain River Rapids',
  },
  {
    id: 18,
    url: 'https://images.unsplash.com/photo-1590872000386-4348c6393115?q=80&w=688&auto=format&fit=crop',
    title: 'Lush Mountain Valley',
  },
];

const FULL_ASPECT_RATIO = 16 / 9;
const COLLAPSED_ASPECT_RATIO = 1 / 3;
const MARGIN = 2;
const GAP = 2;

function FramerMoveableThumbnails() {
  const [index, setIndex] = useState<number>(0);
  const [isDragging, setIsDragging] = useState<boolean>(false);
  const containerRef = useRef<HTMLDivElement | null>(null);

  const x = useMotionValue(0);

  useEffect(() => {
    if (!isDragging && containerRef.current) {
      const containerWidth = containerRef.current.offsetWidth || 1;
      const targetX = -index * containerWidth;

      animate(x, targetX, {
        type: 'spring',
        stiffness: 300,
        damping: 30,
      });
    }
  }, [index, x, isDragging]);

  return (
    <div className='w-full lg:p-10 sm:p-4 p-2'>
      <div className='flex flex-col gap-3'>
        {/* Main Carousel */}
        <div className='relative overflow-hidden rounded-lg' ref={containerRef}>
          <motion.div
            className='flex'
            drag='x'
            dragElastic={0.2}
            dragMomentum={false}
            onDragStart={() => setIsDragging(true)}
            onDragEnd={(e, info) => {
              setIsDragging(false);
              const containerWidth = containerRef.current?.offsetWidth || 1;
              const offset = info.offset.x;
              const velocity = info.velocity.x;

              let newIndex = index;

              // If fast swipe, use velocity
              if (Math.abs(velocity) > 500) {
                newIndex = velocity > 0 ? index - 1 : index + 1;
              }
              // Otherwise use offset threshold (30% of container width)
              else if (Math.abs(offset) > containerWidth * 0.3) {
                newIndex = offset > 0 ? index - 1 : index + 1;
              }

              // Clamp index
              newIndex = Math.max(0, Math.min(items.length - 1, newIndex));
              setIndex(newIndex);
            }}
            style={{ x }}
          >
            {items.map((item) => (
              <div key={item.id} className='shrink-0 w-full h-[400px]'>
                <img
                  src={item.url}
                  alt={item.title}
                  className='w-full h-full object-cover rounded-lg select-none pointer-events-none'
                  draggable={false}
                />
              </div>
            ))}
          </motion.div>

          {/* Navigation Buttons */}
          <motion.button
            disabled={index === 0}
            onClick={() => setIndex((i) => Math.max(0, i - 1))}
            className={`absolute left-4 top-1/2 -translate-y-1/2 w-10 h-10 rounded-full flex items-center justify-center shadow-lg transition-transform z-10
              ${
                index === 0
                  ? 'opacity-40 cursor-not-allowed'
                  : 'bg-white hover:scale-110 hover:opacity-100 opacity-70'
              }`}
          >
            <svg className='w-6 h-6' fill='none' stroke='currentColor' viewBox='0 0 24 24'>
              <path
                strokeLinecap='round'
                strokeLinejoin='round'
                strokeWidth={2}
                d='M15 19l-7-7 7-7'
              />
            </svg>
          </motion.button>

          {/* Next Button */}
          <motion.button
            disabled={index === items.length - 1}
            onClick={() => setIndex((i) => Math.min(items.length - 1, i + 1))}
            className={`absolute right-4 top-1/2 -translate-y-1/2 w-10 h-10 rounded-full flex items-center justify-center shadow-lg transition-transform z-10
              ${
                index === items.length - 1
                  ? 'opacity-40 cursor-not-allowed'
                  : 'bg-white hover:scale-110 hover:opacity-100 opacity-70'
              }`}
          >
            <svg className='w-6 h-6' fill='none' stroke='currentColor' viewBox='0 0 24 24'>
              <path strokeLinecap='round' strokeLinejoin='round' strokeWidth={2} d='M9 5l7 7-7 7' />
            </svg>
          </motion.button>
        </div>

        <Thumbnails index={index} setIndex={setIndex} />
      </div>
    </div>
  );
}

function Thumbnails({ index, setIndex }: { index: number; setIndex: any }) {
  const x = index * 100 * (COLLAPSED_ASPECT_RATIO / FULL_ASPECT_RATIO) + MARGIN + index * GAP;
  const xSpring = useSpring(x, { bounce: 0 });
  const xPercentage = useMotionTemplate`-${xSpring}%`;

  useEffect(() => {
    xSpring.set(x);
  }, [x, xSpring]);

  return (
    <div className='flex h-16 justify-center overflow-hidden'>
      <motion.div
        style={{
          aspectRatio: FULL_ASPECT_RATIO,
          gap: `${GAP}%`,
          x: xPercentage,
        }}
        className='flex min-w-0'
      >
        {items.map((item, i) => (
          <motion.button
            key={item.id}
            onClick={() => setIndex(i)}
            initial={false}
            animate={i === index ? 'active' : 'inactive'}
            variants={{
              active: {
                aspectRatio: FULL_ASPECT_RATIO,
                marginLeft: `${MARGIN}%`,
                marginRight: `${MARGIN}%`,
              },
              inactive: {
                aspectRatio: COLLAPSED_ASPECT_RATIO,
                marginLeft: 0,
                marginRight: 0,
              },
            }}
            className='h-full shrink-0'
          >
            <img
              src={item.url}
              alt={item.title}
              className='h-full w-full object-cover pointer-events-none select-none'
            />
          </motion.button>
        ))}
      </motion.div>
    </div>
  );
}

export default FramerMoveableThumbnails;

components/ui/carousel.tsx
'use client';

import { cn } from '@/lib/utils';
import type { EmblaCarouselType, EmblaEventType, EmblaOptionsType } from 'embla-carousel';
import useEmblaCarousel from 'embla-carousel-react';
import { AnimatePresence, motion } from 'motion/react';
import type React from 'react';
import {
  createContext,
  forwardRef,
  useCallback,
  useContext,
  useEffect,
  useId,
  useRef,
  useState,
} from 'react';

// ============= TYPES =============
interface CarouselProps extends React.HTMLAttributes<HTMLDivElement> {
  options?: EmblaOptionsType;
  plugins?: Parameters<typeof useEmblaCarousel>[1];
  isScale?: boolean;
}

interface CarouselContextType {
  emblaApi: EmblaCarouselType | undefined;
  emblaThumbsApi: EmblaCarouselType | undefined;
  emblaRef: ReturnType<typeof useEmblaCarousel>[0];
  emblaThumbsRef: ReturnType<typeof useEmblaCarousel>[0];
  prevBtnDisabled: boolean;
  nextBtnDisabled: boolean;
  onPrevButtonClick: () => void;
  onNextButtonClick: () => void;
  selectedIndex: number;
  scrollSnaps: number[];
  onDotButtonClick: (index: number) => void;
  scrollProgress: number;
  selectedSnap: number;
  snapCount: number;
  isScale: boolean;
  slidesArr: string[];
  setSlidesArr: React.Dispatch<React.SetStateAction<string[]>>;
  onThumbClick: (index: number) => void;
  carouselId: string;
  orientation: 'vertical' | 'horizontal';
  direction: 'ltr' | 'rtl' | undefined;
  handleKeyDown: (event: React.KeyboardEvent<HTMLDivElement>) => void;
}

// ============= CONTEXT =============
const CarouselContext = createContext<CarouselContextType | undefined>(undefined);

export const useCarousel = () => {
  const context = useContext(CarouselContext);
  if (!context) {
    throw new Error('useCarousel must be used within a Carousel component');
  }
  return context;
};

// ============= UTILITIES =============
const TWEEN_FACTOR_BASE = 0.52;
const numberWithinRange = (number: number, min: number, max: number): number =>
  Math.min(Math.max(number, min), max);

// ============= MAIN CAROUSEL COMPONENT =============
export const Carousel = forwardRef<HTMLDivElement, CarouselProps>(
  ({ children, options = {}, plugins = [], className, isScale = false, dir, ...props }, ref) => {
    const carouselId = useId();
    const [slidesArr, setSlidesArr] = useState<string[]>([]);

    const orientation = options.axis === 'y' ? 'vertical' : 'horizontal';
    const direction = options.direction ?? (dir as 'ltr' | 'rtl' | undefined);

    // Main carousel
    const [emblaRef, emblaApi] = useEmblaCarousel(
      {
        ...options,
        axis: orientation === 'vertical' ? 'y' : 'x',
        direction,
      },
      plugins
    );

    // Thumbnails carousel
    const [emblaThumbsRef, emblaThumbsApi] = useEmblaCarousel({
      containScroll: 'keepSnaps',
      dragFree: true,
      axis: orientation === 'vertical' ? 'y' : 'x',
      direction,
    });

    // State
    const [prevBtnDisabled, setPrevBtnDisabled] = useState(true);
    const [nextBtnDisabled, setNextBtnDisabled] = useState(true);
    const [selectedIndex, setSelectedIndex] = useState(0);
    const [scrollSnaps, setScrollSnaps] = useState<number[]>([]);
    const [scrollProgress, setScrollProgress] = useState(0);
    const [snapCount, setSnapCount] = useState(0);

    // Navigation callbacks
    const onPrevButtonClick = useCallback(() => {
      emblaApi?.scrollPrev();
    }, [emblaApi]);

    const onNextButtonClick = useCallback(() => {
      emblaApi?.scrollNext();
    }, [emblaApi]);

    const onDotButtonClick = useCallback(
      (index: number) => {
        emblaApi?.scrollTo(index);
      },
      [emblaApi]
    );

    const onThumbClick = useCallback(
      (index: number) => {
        if (!emblaApi || !emblaThumbsApi) return;
        emblaApi.scrollTo(index);
      },
      [emblaApi, emblaThumbsApi]
    );

    // Keyboard navigation
    const handleKeyDown = useCallback(
      (event: React.KeyboardEvent<HTMLDivElement>) => {
        if (!emblaApi) return;
        switch (event.key) {
          case 'ArrowLeft':
            event.preventDefault();
            if (orientation === 'horizontal') {
              direction === 'rtl' ? onNextButtonClick() : onPrevButtonClick();
            }
            break;
          case 'ArrowRight':
            event.preventDefault();
            if (orientation === 'horizontal') {
              direction === 'rtl' ? onPrevButtonClick() : onNextButtonClick();
            }
            break;
          case 'ArrowUp':
            event.preventDefault();
            if (orientation === 'vertical') onPrevButtonClick();
            break;
          case 'ArrowDown':
            event.preventDefault();
            if (orientation === 'vertical') onNextButtonClick();
            break;
        }
      },
      [emblaApi, orientation, direction, onPrevButtonClick, onNextButtonClick]
    );

    // Selection handler
    const onSelect = useCallback(() => {
      if (!emblaApi) return;
      setSelectedIndex(emblaApi.selectedScrollSnap());
      setPrevBtnDisabled(!emblaApi.canScrollPrev());
      setNextBtnDisabled(!emblaApi.canScrollNext());
      emblaThumbsApi?.scrollTo(emblaApi.selectedScrollSnap());
    }, [emblaApi, emblaThumbsApi]);

    // Scroll progress handler
    const onScroll = useCallback((emblaApi: EmblaCarouselType) => {
      const progress = Math.max(0, Math.min(1, emblaApi.scrollProgress()));
      setScrollProgress(progress * 100);
    }, []);

    // Scale animation for isScale mode
    const tweenFactor = useRef(0);
    const tweenNodes = useRef<HTMLElement[]>([]);

    const setTweenNodes = useCallback(
      (emblaApi: EmblaCarouselType): void => {
        if (!isScale) return;
        tweenNodes.current = emblaApi
          .slideNodes()
          .map((slideNode) => slideNode.querySelector('.slider_content')) as HTMLElement[];
      },
      [isScale]
    );

    const setTweenFactor = useCallback(
      (emblaApi: EmblaCarouselType) => {
        if (!isScale) return;
        tweenFactor.current = TWEEN_FACTOR_BASE * emblaApi.scrollSnapList().length;
      },
      [isScale]
    );

    const tweenScale = useCallback(
      (emblaApi: EmblaCarouselType, eventName?: EmblaEventType) => {
        if (!isScale) return;
        const engine = emblaApi.internalEngine();
        const scrollProgress = emblaApi.scrollProgress();
        const slidesInView = emblaApi.slidesInView();
        const isScrollEvent = eventName === 'scroll';

        emblaApi.scrollSnapList().forEach((scrollSnap, snapIndex) => {
          let diffToTarget = scrollSnap - scrollProgress;
          const slidesInSnap = engine.slideRegistry[snapIndex];

          slidesInSnap.forEach((slideIndex) => {
            if (isScrollEvent && !slidesInView.includes(slideIndex)) return;

            if (engine.options.loop) {
              engine.slideLooper.loopPoints.forEach((loopItem) => {
                const target = loopItem.target();
                if (slideIndex === loopItem.index && target !== 0) {
                  const sign = Math.sign(target);
                  if (sign === -1) {
                    diffToTarget = scrollSnap - (1 + scrollProgress);
                  }
                  if (sign === 1) {
                    diffToTarget = scrollSnap + (1 - scrollProgress);
                  }
                }
              });
            }

            const tweenValue = 1 - Math.abs(diffToTarget * tweenFactor.current);
            const scale = numberWithinRange(tweenValue, 0, 1).toString();
            const tweenNode = tweenNodes.current[slideIndex];
            if (tweenNode) {
              tweenNode.style.transform = `scale(${scale})`;
            }
          });
        });
      },
      [isScale]
    );

    // Effects
    useEffect(() => {
      if (!emblaApi) return;
      setScrollSnaps(emblaApi.scrollSnapList());
      setSnapCount(emblaApi.scrollSnapList().length);
      onSelect();
      onScroll(emblaApi);

      emblaApi
        .on('reInit', onSelect)
        .on('select', onSelect)
        .on('reInit', onScroll)
        .on('scroll', onScroll);

      if (isScale) {
        setTweenNodes(emblaApi);
        setTweenFactor(emblaApi);
        tweenScale(emblaApi);
        emblaApi
          .on('reInit', setTweenNodes)
          .on('reInit', setTweenFactor)
          .on('reInit', tweenScale)
          .on('scroll', tweenScale);
      }
    }, [emblaApi, onSelect, onScroll, isScale, setTweenNodes, setTweenFactor, tweenScale]);

    return (
      <CarouselContext.Provider
        value={{
          emblaApi,
          emblaThumbsApi,
          emblaRef,
          emblaThumbsRef,
          prevBtnDisabled,
          nextBtnDisabled,
          onPrevButtonClick,
          onNextButtonClick,
          selectedIndex,
          scrollSnaps,
          onDotButtonClick,
          scrollProgress,
          selectedSnap: selectedIndex,
          snapCount,
          isScale,
          slidesArr,
          setSlidesArr,
          onThumbClick,
          carouselId,
          orientation,
          direction,
          handleKeyDown,
        }}
      >
        <div
          ref={ref}
          tabIndex={0}
          onKeyDownCapture={handleKeyDown}
          className={cn('relative w-full focus:outline-hidden', className)}
          dir={direction}
          {...props}
        >
          {children}
        </div>
      </CarouselContext.Provider>
    );
  }
);

Carousel.displayName = 'Carousel';

// ============= SLIDER CONTAINER =============
export const SliderContainer = forwardRef<HTMLDivElement, React.HTMLAttributes<HTMLDivElement>>(
  ({ className, children, ...props }, ref) => {
    const { emblaRef, orientation } = useCarousel();

    return (
      <div ref={emblaRef} className='overflow-hidden' {...props}>
        <div
          ref={ref}
          className={cn('flex', orientation === 'vertical' ? 'flex-col' : 'flex-row', className)}
          style={{ touchAction: 'pan-y pinch-zoom' }}
        >
          {children}
        </div>
      </div>
    );
  }
);

SliderContainer.displayName = 'SliderContainer';

// ============= SLIDER ITEM =============
interface SliderProps extends React.HTMLAttributes<HTMLDivElement> {
  thumbnailSrc?: string;
}

export const Slider = forwardRef<HTMLDivElement, SliderProps>(
  ({ children, className, thumbnailSrc, ...props }, ref) => {
    const { isScale, setSlidesArr, orientation } = useCarousel();

    useEffect(() => {
      if (thumbnailSrc) {
        setSlidesArr((prev) => {
          if (!prev.includes(thumbnailSrc)) {
            return [...prev, thumbnailSrc];
          }
          return prev;
        });
      }
    }, [thumbnailSrc, setSlidesArr]);

    return (
      <div
        ref={ref}
        className={cn(
          'min-w-0 shrink-0 grow-0',
          // orientation === 'vertical' ? 'pb-1' : 'pr-1',
          className
        )}
        {...props}
      >
        {isScale ? <div className='slider_content'>{children}</div> : children}
      </div>
    );
  }
);

Slider.displayName = 'Slider';

// ============= NAVIGATION BUTTONS =============
export const SliderPrevButton = forwardRef<
  HTMLButtonElement,
  React.ButtonHTMLAttributes<HTMLButtonElement>
>(({ children, className, ...props }, ref) => {
  const { onPrevButtonClick, prevBtnDisabled } = useCarousel();

  return (
    <button
      ref={ref}
      type='button'
      onClick={onPrevButtonClick}
      disabled={prevBtnDisabled}
      className={cn('', className)}
      {...props}
    >
      {children}
    </button>
  );
});

SliderPrevButton.displayName = 'SliderPrevButton';

export const SliderNextButton = forwardRef<
  HTMLButtonElement,
  React.ButtonHTMLAttributes<HTMLButtonElement>
>(({ children, className, ...props }, ref) => {
  const { onNextButtonClick, nextBtnDisabled } = useCarousel();

  return (
    <button
      ref={ref}
      type='button'
      onClick={onNextButtonClick}
      disabled={nextBtnDisabled}
      className={cn('', className)}
      {...props}
    >
      {children}
    </button>
  );
});

SliderNextButton.displayName = 'SliderNextButton';

// ============= PROGRESS BAR =============
export const SliderProgress = forwardRef<HTMLDivElement, React.HTMLAttributes<HTMLDivElement>>(
  ({ className, ...props }, ref) => {
    const { scrollProgress } = useCarousel();

    return (
      <div
        ref={ref}
        className={cn(
          'bg-neutral-500 relative rounded-md h-2 w-96 max-w-full overflow-hidden',
          className
        )}
        {...props}
      >
        <div
          className='dark:bg-white bg-black absolute w-full top-0 -left-full bottom-0 transition-transform'
          style={{ transform: `translate3d(${scrollProgress}%,0px,0px)` }}
        />
      </div>
    );
  }
);

SliderProgress.displayName = 'SliderProgress';

// ============= SNAP DISPLAY =============
export const SliderSnapDisplay = forwardRef<HTMLDivElement, React.HTMLAttributes<HTMLDivElement>>(
  ({ className, ...props }, ref) => {
    const { selectedSnap, snapCount } = useCarousel();
    const prevSnapRef = useRef(selectedSnap);
    const direction = selectedSnap > prevSnapRef.current ? 1 : -1;

    useEffect(() => {
      prevSnapRef.current = selectedSnap;
    }, [selectedSnap]);

    return (
      <div
        ref={ref}
        className={cn('mix-blend-difference overflow-hidden flex gap-1 items-center', className)}
        {...props}
      >
        <AnimatePresence mode='wait'>
          <motion.div
            key={selectedSnap}
            custom={direction}
            // @ts-expect-error
            initial={(d: number) => ({ y: d * 20, opacity: 0 })}
            animate={{ y: 0, opacity: 1 }}
            // @ts-expect-error
            exit={(d: number) => ({ y: d * -20, opacity: 0 })}
          >
            {selectedSnap + 1}
          </motion.div>
        </AnimatePresence>
        <span>/ {snapCount}</span>
      </div>
    );
  }
);

SliderSnapDisplay.displayName = 'SliderSnapDisplay';

// ============= DOT BUTTONS =============
interface SliderDotButtonProps extends React.HTMLAttributes<HTMLDivElement> {
  activeClass?: string;
}

export const SliderDotButton = forwardRef<HTMLDivElement, SliderDotButtonProps>(
  ({ className, activeClass, ...props }, ref) => {
    const { selectedIndex, scrollSnaps, orientation, onDotButtonClick, carouselId } = useCarousel();

    return (
      <div ref={ref} className={cn('flex gap-2', className)} {...props}>
        {scrollSnaps.map((_, index) => (
          <button
            key={`${carouselId}-dot-${_}`}
            type='button'
            onClick={() => onDotButtonClick(index)}
            className={cn(
              'relative inline-flex p-0 m-0',
              orientation === 'vertical' ? 'h-6 w-1' : 'w-6 h-1'
            )}
          >
            <div
              className={cn(
                'bg-neutral-500/40 rounded-full ',
                orientation === 'vertical' ? 'h-6 w-1' : 'w-6 h-1'
              )}
            />
            {index === selectedIndex && (
              <AnimatePresence mode='wait'>
                <motion.div
                  transition={{
                    layout: {
                      duration: 0.4,
                      ease: 'easeInOut',
                      delay: 0.04,
                    },
                  }}
                  layoutId={`hover-${carouselId}`}
                  className={cn(
                    'absolute z-3 w-full h-full left-0 top-0 dark:bg-white bg-black rounded-full',
                    orientation === 'vertical' ? 'h-6 w-1' : 'w-6 h-1',
                    activeClass
                  )}
                />
              </AnimatePresence>
            )}
          </button>
        ))}
      </div>
    );
  }
);

SliderDotButton.displayName = 'SliderDotButton';

// ============= CAROUSEL INDICATORS =============
interface CarouselIndicatorProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  index: number;
}

export const CarouselIndicator = forwardRef<HTMLButtonElement, CarouselIndicatorProps>(
  ({ className, index, ...props }, ref) => {
    const { selectedIndex, onDotButtonClick } = useCarousel();
    const isActive = selectedIndex === index;

    return (
      <button
        ref={ref}
        type='button'
        onClick={() => onDotButtonClick(index)}
        className={cn(
          'h-1.5 w-6 rounded-full transition-colors',
          isActive ? 'bg-primary' : 'bg-primary/50',
          className
        )}
        aria-label={`Go to slide ${index + 1}`}
        {...props}
      >
        <span className='sr-only'>Slide {index + 1}</span>
      </button>
    );
  }
);

CarouselIndicator.displayName = 'CarouselIndicator';

// Auto-generate thumbnails from slides
export const ThumbsSlider = forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement> & {
    thumbsClassName?: string;
    thumbsSliderClassName?: string;
  }
>(({ className, thumbsClassName, thumbsSliderClassName, ...props }, ref) => {
  const { slidesArr, selectedIndex, onThumbClick, orientation, emblaThumbsRef } = useCarousel();

  if (slidesArr.length === 0) return null;

  return (
    <div ref={emblaThumbsRef} className={cn('overflow-hidden', className)} {...props}>
      <div
        ref={ref}
        className={cn(
          'flex gap-2 h-[300px]',
          orientation === 'vertical' ? 'flex-col' : 'flex-row',
          thumbsClassName
        )}
      >
        {slidesArr.map((src, index) => (
          <div
            key={src}
            onClick={() => onThumbClick(index)}
            className={cn(
              'shrink-0 cursor-pointer transition-opacity',
              'border-2 rounded-md',
              orientation === 'vertical' ? 'basis-[15%] h-20' : 'basis-[15%] h-24',
              selectedIndex === index
                ? 'opacity-100 border-primary'
                : 'opacity-30 border-transparent',
              thumbsSliderClassName
            )}
          >
            <img
              src={src}
              alt={`Thumbnail ${index + 1}`}
              className='w-full h-full object-cover rounded-md'
            />
          </div>
        ))}
      </div>
    </div>
  );
});

ThumbsSlider.displayName = 'ThumbsSlider';

// Alias for backward compatibility

demo.tsx
import FramerMoveableThumbnails from '@/components/ui/framer-moveable-thumbnails';

export default function Default() {
  return (
    <div className='w-full max-w-3xl mx-auto'>
      <FramerMoveableThumbnails />
    </div>
  );
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
