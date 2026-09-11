<!-- Progressive Carousel · @uilayout.contact · https://21st.dev/@uilayout.contact/components/progressive-carousel
     license: unspecified · category: gallery
      -->

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
components/ui/progressive-carousel.tsx
import { cn } from '@/lib/utils';
import { AnimatePresence, motion } from 'motion/react';
import React, {
  createContext,
  type FC,
  type ReactNode,
  useContext,
  useEffect,
  useRef,
  useState,
} from 'react';

// Define the type for the context value
interface ProgressSliderContextType {
  active: string;
  progress: number;
  handleButtonClick: (value: string) => void;
  vertical: boolean;
}

// Define the type for the component props
interface ProgressSliderProps {
  children: ReactNode;
  duration?: number;
  fastDuration?: number;
  vertical?: boolean;
  activeSlider: string;
  className?: string;
}

interface SliderContentProps {
  children: ReactNode;
  className?: string;
}

interface SliderWrapperProps {
  children: ReactNode;
  value: string;
  className?: string;
}

interface ProgressBarProps {
  children: ReactNode;
  className?: string;
}

interface SliderBtnProps {
  children: ReactNode;
  value: string;
  className?: string;
  progressBarClass?: string;
}

// Create the context with an undefined initial value
const ProgressSliderContext = createContext<ProgressSliderContextType | undefined>(undefined);

export const useProgressSliderContext = (): ProgressSliderContextType => {
  const context = useContext(ProgressSliderContext);
  if (!context) {
    throw new Error('useProgressSliderContext must be used within a ProgressSlider');
  }
  return context;
};

export const ProgressSlider: FC<ProgressSliderProps> = ({
  children,
  duration = 5000,
  fastDuration = 400,
  vertical = false,
  activeSlider,
  className,
}) => {
  const [active, setActive] = useState<string>(activeSlider);
  const [progress, setProgress] = useState<number>(0);
  const [isFastForward, setIsFastForward] = useState<boolean>(false);
  const frame = useRef<number>(0);
  const firstFrameTime = useRef<number>(performance.now());
  const targetValue = useRef<string | null>(null);
  const [sliderValues, setSliderValues] = useState<string[]>([]);

  useEffect(() => {
    const getChildren = React.Children.toArray(children).find(
      (child) => (child as React.ReactElement<any>).type === SliderContent
    ) as React.ReactElement<any> | undefined;

    if (getChildren) {
      const values = React.Children.toArray(getChildren.props.children).map(
        (child) => (child as React.ReactElement<any>).props.value as string
      );
      setSliderValues(values);
    }
  }, [children]);

  useEffect(() => {
    if (sliderValues.length > 0) {
      firstFrameTime.current = performance.now();
      frame.current = requestAnimationFrame(animate);
    }
    return () => {
      cancelAnimationFrame(frame.current);
    };
  }, [sliderValues, active, isFastForward]);

  const animate = (now: number) => {
    const currentDuration = isFastForward ? fastDuration : duration;
    const elapsedTime = now - firstFrameTime.current;
    const timeFraction = elapsedTime / currentDuration;

    if (timeFraction <= 1) {
      setProgress(isFastForward ? progress + (100 - progress) * timeFraction : timeFraction * 100);
      frame.current = requestAnimationFrame(animate);
    } else {
      if (isFastForward) {
        setIsFastForward(false);
        if (targetValue.current !== null) {
          setActive(targetValue.current);
          targetValue.current = null;
        }
      } else {
        // Move to the next slide
        const currentIndex = sliderValues.indexOf(active);
        const nextIndex = (currentIndex + 1) % sliderValues.length;
        setActive(sliderValues[nextIndex]);
      }
      setProgress(0);
      firstFrameTime.current = performance.now();
    }
  };

  const handleButtonClick = (value: string) => {
    if (value !== active) {
      const elapsedTime = performance.now() - firstFrameTime.current;
      const currentProgress = (elapsedTime / duration) * 100;
      setProgress(currentProgress);
      targetValue.current = value;
      setIsFastForward(true);
      firstFrameTime.current = performance.now();
    }
  };

  return (
    <ProgressSliderContext.Provider value={{ active, progress, handleButtonClick, vertical }}>
      <div className={cn('relative', className)}>{children}</div>
    </ProgressSliderContext.Provider>
  );
};

export const SliderContent: FC<SliderContentProps> = ({ children, className }) => {
  return <div className={cn('', className)}>{children}</div>;
};

export const SliderWrapper: FC<SliderWrapperProps> = ({ children, value, className }) => {
  const { active } = useProgressSliderContext();

  return (
    <AnimatePresence mode='popLayout'>
      {active === value && (
        <motion.div
          key={value}
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          exit={{ opacity: 0 }}
          className={cn('', className)}
        >
          {children}
        </motion.div>
      )}
    </AnimatePresence>
  );
};

export const SliderBtnGroup: FC<ProgressBarProps> = ({ children, className }) => {
  return <div className={cn('', className)}>{children}</div>;
};

export const SliderBtn: FC<SliderBtnProps> = ({ children, value, className, progressBarClass }) => {
  const { active, progress, handleButtonClick, vertical } = useProgressSliderContext();

  return (
    <button
      className={cn(`relative ${active === value ? 'opacity-100' : 'opacity-50'}`, className)}
      onClick={() => handleButtonClick(value)}
    >
      {children}
      <div
        className='absolute inset-0 overflow-hidden -z-10 max-h-full max-w-full '
        role='progressbar'
        aria-valuenow={active === value ? progress : 0}
      >
        <span
          className={cn('absolute left-0 ', progressBarClass)}
          style={{
            [vertical ? 'height' : 'width']: active === value ? `${progress}%` : '0%',
          }}
        />
      </div>
    </button>
  );
};

demo.tsx
'use client';
import Image from 'next/image';
import {
  SliderBtnGroup,
  ProgressSlider,
  SliderBtn,
  SliderContent,
  SliderWrapper,
} from '@/components/ui/progressive-carousel';

const items = [
  {
    img: "https://cdn.21st.dev/assets/mirror/81/819d6181fac78c13d6c5c47b6b886a02ef2d5d1113485d71378b397d0641bb5f.jpg",
    title: 'Bridge',
    desc: 'A breathtaking view of a city illuminated by countless lights, showcasing the vibrant and bustling nightlife.',
    sliderName: 'bridge',
  },
  {
    img: "https://cdn.21st.dev/assets/mirror/c6/c6a93202432615b6731735b389598cce9379eae6ba7fb89bb1628c546425f078.jpg",
    title: 'Mountains View',
    desc: 'A serene lake reflecting the surrounding mountains and trees, creating a mirror-like surface.',
    sliderName: 'mountains',
  },
  {
    img: "https://cdn.21st.dev/assets/mirror/dc/dc44f9b938664426d83cdbc2588cc1703894bdfb8104ae1568410ff443a470d2.jpg",
    title: 'Autumn',
    desc: 'A picturesque path winding through a dense forest adorned with vibrant autumn foliage.',
    sliderName: 'autumn',
  },
  {
    img: "https://cdn.21st.dev/assets/mirror/06/06aec18cd03d0dce8a99e2df34cf15cc64c4686dfb87cda45ffc3eaccc8d91c1.jpg",
    title: 'Foggy',
    sliderName: 'foggy',
    desc: 'A stunning foggy view over the foresh, with the sun casting a golden glow across the forest. ',
  },
];
export default function Demos() {
  return (
    <main className="max-w-4xl px-10 mx-auto">
      <ProgressSlider vertical={false} activeSlider='bridge'>
        <SliderContent>
          {items.map((item, index) => (
            <SliderWrapper key={index} value={item?.sliderName}>
              <Image
                className='rounded-xl 2xl:h-[500px] h-[450px] object-cover'
                src={item.img}
                width={1900}
                height={1080}
                alt={item.desc}
              />
            </SliderWrapper>
          ))}
        </SliderContent>

        <SliderBtnGroup className='absolute bottom-0 h-fit dark:text-white text-black dark:bg-black/40 bg-white/40  backdrop-blur-md overflow-hidden grid grid-cols-2 md:grid-cols-4  rounded-md'>
          {items.map((item, index) => (
            <SliderBtn
              key={index}
              value={item?.sliderName}
              className='text-left cursor-pointer p-3 border-r'
              progressBarClass='dark:bg-black bg-white h-full'
            >
              <h2 className='relative px-4 rounded-full w-fit dark:bg-white dark:text-black text-white bg-gray-900 mb-2'>
                {item.title}
              </h2>
              <p className='text-sm font-medium  line-clamp-2'>{item.desc}</p>
            </SliderBtn>
          ))}
        </SliderBtnGroup>
      </ProgressSlider>
    </main>
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
