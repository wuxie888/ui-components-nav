<!-- Vertical Thumbnail Autostart Slider · @uilayout.contact · https://21st.dev/@uilayout.contact/components/vertical-thumbnail-autostart-slider
     license: no-license · category: gallery
     An auto-playing vertical image carousel with a synced thumbnail navigation strip on the side. -->

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
components/ui/verticalthumbs-autostart-slider.tsx
'use client';
import {
  Carousel,
  Slider,
  SliderContainer,
  SliderDotButton,
  ThumbsSlider,
} from '@/components/ui/carousel';
import type { EmblaOptionsType } from 'embla-carousel';
import Autoplay from 'embla-carousel-autoplay';
import React, { ReactNode } from 'react';

function VerticalthumbsAutostartSlider() {
  const OPTIONS: EmblaOptionsType = {
    loop: false,
    axis: 'y',
  };
  return (
    <>
      <div className='py-10'>
        <Carousel
          options={OPTIONS}
          plugins={[
            Autoplay({
              playOnInit: true,
              delay: 2000,
              stopOnMouseEnter: false,
              stopOnInteraction: false,
            }),
          ]}
          dir='ltr'
          className='relative h-full'
        >
          <SliderContainer className='h-[400px]'>
            <Slider
              className='w-full h-full'
              thumbnailSrc='https://images.unsplash.com/photo-1759395073808-17782f3d8d66?q=80&w=1471&auto=format&fit=crop'
            >
              <img
                src='https://images.unsplash.com/photo-1759395073808-17782f3d8d66?q=80&w=1471&auto=format&fit=crop'
                alt='Slide 1'
                className='h-full w-full object-cover rounded-lg'
              />
            </Slider>

            <Slider
              className='w-full h-full'
              thumbnailSrc='https://images.unsplash.com/photo-1759434192768-fe3facebd5f6?q=80&w=1471&auto=format&fit=crop'
            >
              <img
                src='https://images.unsplash.com/photo-1759434192768-fe3facebd5f6?q=80&w=1471&auto=format&fit=crop'
                alt='Slide 2'
                className='h-full w-full object-cover rounded-lg'
              />
            </Slider>

            <Slider
              className='w-full h-full'
              thumbnailSrc='https://images.unsplash.com/photo-1758641008040-28cdd59ca8fb?q=80&w=687&auto=format&fit=crop'
            >
              <img
                src='https://images.unsplash.com/photo-1758641008040-28cdd59ca8fb?q=80&w=687&auto=format&fit=crop'
                alt='Slide 3'
                className='h-full w-full object-cover rounded-lg'
              />
            </Slider>

            <Slider
              className='w-full h-full'
              thumbnailSrc='https://images.unsplash.com/photo-1618220649687-ba860f3176e7?q=80&w=1474&auto=format&fit=crop'
            >
              <img
                src='https://images.unsplash.com/photo-1618220649687-ba860f3176e7?q=80&w=1474&auto=format&fit=crop'
                alt='Slide 4'
                className='h-full w-full object-cover rounded-lg'
              />
            </Slider>

            <Slider
              className='w-full h-full'
              thumbnailSrc='https://images.unsplash.com/photo-1525310072745-f49212b5ac6d?q=80&w=765&auto=format&fit=crop'
            >
              <img
                src='https://images.unsplash.com/photo-1525310072745-f49212b5ac6d?q=80&w=765&auto=format&fit=crop'
                alt='Slide 5'
                className='h-full w-full object-cover rounded-lg'
              />
            </Slider>
          </SliderContainer>

          <ThumbsSlider
            className='absolute right-4 top-1/2 -translate-y-1/2 w-20'
            thumbsSliderClassName='border-black'
          />
        </Carousel>
      </div>
    </>
  );
}

export default VerticalthumbsAutostartSlider;

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
import VerticalthumbsAutostartSlider from '@/components/ui/vertical-thumbnail-autostart-slider';

export default function Default() {
  return (
    <div className='mx-auto max-w-md'>
      <VerticalthumbsAutostartSlider />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install embla-carousel embla-carousel-autoplay embla-carousel-class-names embla-carousel-react lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add carousel
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
