<!-- Story · @youcefbnm · https://21st.dev/@youcefbnm/components/story
     license: unspecified · category: carousel
     - story similar to IG and facebook, you can also use it as a autoplay carousel -->

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
import { cn } from '@/lib/utils';
import * as React from 'react';
import { Button, ButtonProps } from '@/components/systaliko-ui/shadcn/button';
import { PauseIcon, PlayIcon, ReplyIcon } from 'lucide-react';

interface StoryProps extends React.HTMLAttributes<HTMLDivElement> {
  mediaLength: number;
  duration?: number;
}
interface StoryContextValue {
  mediaLength: number;
  currentIndex: number;
  progress: number;
  isPaused: boolean;
  isEnded: boolean;
  handleControl: () => void;
  setCurrentIndex: (index: number) => void;
  setIsPaused: (paused: boolean) => void;
  setIsEnded: (ended: boolean) => void;
}
const StoryContext = React.createContext<StoryContextValue | undefined>(
  undefined,
);
function useStoryContext() {
  const context = React.useContext(StoryContext);
  if (context === undefined) {
    throw new Error('useStoryContext must be used within a StoryProvider');
  }
  return context;
}
export const Story = React.forwardRef<HTMLDivElement, StoryProps>(
  ({ mediaLength, duration = 2000, className, children, ...props }, ref) => {
    const [currentIndex, setCurrentIndex] = React.useState(0);
    const [progress, setProgress] = React.useState(0);
    const [isPaused, setIsPaused] = React.useState(false);
    const [isEnded, setIsEnded] = React.useState(false);
    const progressRef = React.useRef<number>(0);
    const intervalRef = React.useRef<ReturnType<typeof setInterval> | null>(
      null,
    );

    React.useEffect(() => {
      progressRef.current = 0;
      setProgress(0);
    }, [currentIndex, duration, mediaLength]);
    React.useEffect(() => {
      if (mediaLength === 0 || isPaused) return;

      if (intervalRef.current) {
        clearInterval(intervalRef.current);
        intervalRef.current = null;
      }

      const tick = 50;
      const totalTicks = duration / tick;

      intervalRef.current = setInterval(() => {
        progressRef.current += 1;
        const newProgress = (progressRef.current / totalTicks) * 100;
        setProgress(newProgress);

        if (progressRef.current >= totalTicks) {
          clearInterval(intervalRef.current!);
          intervalRef.current = null;

          if (currentIndex < mediaLength - 1) {
            setCurrentIndex((idx) => idx + 1);
          } else {
            setIsPaused(true);
            setIsEnded(true);
          }
        }
      }, tick);

      return () => {
        if (intervalRef.current) {
          clearInterval(intervalRef.current);
          intervalRef.current = null;
        }
      };
    }, [isPaused, currentIndex, duration, mediaLength]);

    if (mediaLength === 0) {
      return (
        <div className="text-center text-secondary">No stories to display</div>
      );
    }

    const handleControl = () => {
      if (isEnded) {
        setCurrentIndex(0);
        setIsEnded(false);
        setIsPaused(false);
      } else {
        setIsPaused((prev) => !prev);
      }
    };

    return (
      <StoryContext.Provider
        value={{
          mediaLength,
          currentIndex,
          progress,
          isPaused,
          isEnded,
          handleControl,
          setCurrentIndex,
          setIsPaused,
          setIsEnded,
        }}
      >
        <div className={cn('mx-auto', className)} ref={ref} {...props}>
          {children}
        </div>
      </StoryContext.Provider>
    );
  },
);
Story.displayName = 'Story';

export const StoryProgress = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement> & {
    progressWrapClass?: string;
    progressActiveClass?: string;
  }
>(({ className, progressWrapClass, progressActiveClass, ...props }, ref) => {
  const {
    mediaLength,
    currentIndex,
    progress,
    setCurrentIndex,
    setIsEnded,
    setIsPaused,
  } = useStoryContext();

  const handleProgressClick = (index: number) => {
    setCurrentIndex(index);
    setIsPaused(false);
    setIsEnded(false);
  };

  return (
    <div className={cn('space-x-1 flex', className)} ref={ref} {...props}>
      {Array.from({ length: mediaLength }).map((_, index) => {
        const isActive = index === currentIndex;
        const isCompleted = index < currentIndex;

        return (
          <div
            key={index}
            className={cn(
              'h-1 flex-1 rounded bg-secondary cursor-pointer transition-colors',
              'hover:bg-secondary/80',
              progressWrapClass,
            )}
            onClick={() => handleProgressClick(index)}
            role="button"
            tabIndex={0}
            onKeyDown={(e) => {
              if (e.key === 'Enter' || e.key === ' ') {
                handleProgressClick(index);
              }
            }}
          >
            <div
              className={cn(
                'h-full rounded-[inherit] transition-all duration-200',
                isActive
                  ? 'bg-primary'
                  : isCompleted
                    ? 'bg-primary'
                    : 'bg-transparent',
                progressActiveClass,
              )}
              style={{
                width: isActive ? `${progress}%` : isCompleted ? '100%' : '0%',
              }}
            />
          </div>
        );
      })}
    </div>
  );
});
StoryProgress.displayName = 'StoryProgress';

export const StorySlide = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement> & { index: number }
>(({ index, className, ...props }, ref) => {
  const { currentIndex } = useStoryContext();
  if (index !== currentIndex) return null;
  return (
    <div className={cn('animate-in fade-in', className)} ref={ref} {...props} />
  );
});
StorySlide.displayName = 'StorySlide';

export const StoryControls: React.FC<ButtonProps> = ({
  className,
  ...props
}) => {
  const { isPaused, isEnded, handleControl } = useStoryContext();
  return (
    <Button
      onClick={handleControl}
      size="icon"
      {...props}
      className={className}
    >
      {isPaused ? isEnded ? <ReplyIcon /> : <PlayIcon /> : <PauseIcon />}
    </Button>
  );
};
StoryControls.displayName = 'StoryControls';

export const StoryOverlay: React.FC = () => (
  <div className=" absolute inset-0 ">
    <div className="absolute inset-x-0 top-0 h-24 bg-gradient-to-b from-black to-transparent" />
    <div className="absolute inset-x-0 bottom-0 h-40 bg-gradient-to-t from-black to-transparent" />
  </div>
);

demo.tsx
import { Avatar, AvatarFallback, AvatarImage } from '@/components/ui/avatar';

import { Story,
  StoryProgress,
  StoryControls,
  StorySlide,
  StoryOverlay, } from "@/components/ui/story";
import {
  Dialog,
  DialogContent,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from '@/components/ui/dialog';
const FABRIZIO_STORIES = [
  {
    title: 'Champions league will begin soon',
    caption: 'whos you are running for ?',
    storyImage:
      'https://cdn.21st.dev/assets/mirror/a4/a43a1cdf92a7ded73a0071554501e3ff6e2c751e0437b9e42e623fe854f8d876.jpg',
  },
  {
    title: "who's your favourite player ?",
    caption: 'who you think will win the champions league ?',
    storyImage:
      'https://cdn.21st.dev/assets/mirror/e4/e4d02ca4e9b399dab06f39a4486f99c3ce544d443c43d0de7a8d5f0f1c4f21bf.jpg',
  },
];

const SHADCN_STORIES = [
  {
    title: 'Easy vibes',
    caption: 'In the System Prompts.',
    storyImage:
      'https://cdn.21st.dev/assets/mirror/09/09b10c0826185ea7133592eb84b99a13d4f421dca9089fd4c44492adfbe33174.jpg',
  },
  {
    title: 'The new calendar.tsx is here',
    caption: `
    → Latest react-daypicker
    → Tailwind v3 and v4
    → Date, range & time pickers
    → Persian, Hijri & timezone support
    → 30+ examples to copy, paste, and build.
    `,
    storyImage:
      'https://cdn.21st.dev/assets/mirror/20/2039c036ef48902a03be8610c023a135e9c9ce1442866d4eb14c4b5368626851.jpg',
  },
  {
    title: '🤣🤣🤣🤣🤣',
    caption: 'Me walking away after adding min-w-0 and it works.',
    storyImage:
      'https://cdn.21st.dev/assets/mirror/2b/2ba83238a941377289d8540dd3ecb92b72adf1eb0fddcc1aec21302207cb653b.jpg',
  },
];

const NBA_STORIES = [
  {
    title: 'Shai follows 38 in Game 1 with 34 tonight 🔥🔥🔥',
    caption:
      'MOST POINTS EVER by a player in his first 2 career Finals games 🚨🚨',
    storyImage:
      'https://cdn.21st.dev/assets/mirror/f1/f15899f6f96a11f576f4384b8ea8d5f07237bffcc507e8eb9508aa723b56ae88.jpg',
  },
];

const StoryDemo = () => {
  return  <section className="min-h-dvh p-12 w-full place-content-center">
      <div className="flex gap-4 justify-center">
        <Dialog>
          <DialogTrigger>
            <Avatar className="size-12">
              <AvatarImage
                src="https://scontent.forn3-5.fna.fbcdn.net/v/t39.30808-1/347110386_993663875383747_583934797072922306_n.jpg?stp=c0.124.1179.1179a_cp0_dst-jpg_s80x80_tt6&_nc_cat=1&ccb=1-7&_nc_sid=2d3e12&_nc_ohc=sznTMSftQGgQ7kNvwFnYrhK&_nc_oc=Adl88GWERQJFnS-FhRo3kmyRvwQeqel4uE97CRcHAX2hgEouXRhN98vLowFYZewYbKE&_nc_zt=24&_nc_ht=scontent.forn3-5.fna&_nc_gid=zgCgewXONoFNXl_Ycl7B9Q&oh=00_AfP29XsY8aMHX1lZasw43qaYzda8eY9esKHCjO-ZARUk5A&oe=684D1280"
                alt="@fabrizioRomano"
              />
              <AvatarFallback>FR</AvatarFallback>
            </Avatar>
          </DialogTrigger>
          <DialogContent className="aspect-[12/16] w-auto h-[90vh] overflow-hidden p-0">
            <DialogTitle className="sr-only">Story</DialogTitle>

            <Story
              className="relative size-full "
              duration={5000}
              mediaLength={FABRIZIO_STORIES.length}
            >
              <DialogHeader className="px-4 py-6">
                <div className="relative z-10 flex items-center gap-2">
                  <Avatar className="size-10">
                    <AvatarImage
                      src="https://scontent.forn3-5.fna.fbcdn.net/v/t39.30808-1/347110386_993663875383747_583934797072922306_n.jpg?stp=c0.124.1179.1179a_cp0_dst-jpg_s80x80_tt6&_nc_cat=1&ccb=1-7&_nc_sid=2d3e12&_nc_ohc=sznTMSftQGgQ7kNvwFnYrhK&_nc_oc=Adl88GWERQJFnS-FhRo3kmyRvwQeqel4uE97CRcHAX2hgEouXRhN98vLowFYZewYbKE&_nc_zt=24&_nc_ht=scontent.forn3-5.fna&_nc_gid=zgCgewXONoFNXl_Ycl7B9Q&oh=00_AfP29XsY8aMHX1lZasw43qaYzda8eY9esKHCjO-ZARUk5A&oe=684D1280"
                      alt="@fabrizioRomano"
                    />
                    <AvatarFallback>FR</AvatarFallback>
                  </Avatar>

                  <StoryProgress
                    className="flex-1"
                    progressWrapClass="h-1.5"
                    progressActiveClass="bg-blue-500"
                  />
                  <StoryControls
                    variant="ghost"
                    className="text-white rounded-full"
                  />
                </div>
              </DialogHeader>
              {FABRIZIO_STORIES.map((story, idx) => (
                <StorySlide
                  key={idx}
                  index={idx}
                  className="absolute inset-0 size-full"
                >
                  {/* Example with image */}
                  <img
                    src={story.storyImage}
                    className="w-full h-auto max-h-auto"
                    alt={story.title}
                  />

                  <div className="absolute bottom-4 left-4  z-10 space-y-1 text-white p-4">
                    <a
                      className="text-secondary"
                      href="https://x.com/FabrizioRomano"
                    >
                      @FabrizioRomano
                    </a>
                    <h3 className="text-medium tracking-tight text-foreground-muted">
                      {story.title}
                    </h3>
                    <p className="text-sm text-slate-200">{story.caption}</p>
                  </div>
                </StorySlide>
              ))}
              <StoryOverlay />
            </Story>
          </DialogContent>
        </Dialog>

        <Dialog>
          <DialogTrigger>
            <Avatar className="size-12">
              <AvatarImage
                src="https://cdn.21st.dev/assets/mirror/e9/e941f86e0ca2b1c9eefd47618a92040443f85598470bad47399cee7f259c0756.jpg"
                alt="@shadcn"
              />
              <AvatarFallback>SC</AvatarFallback>
            </Avatar>
          </DialogTrigger>
          <DialogContent className="aspect-[12/16] w-auto h-[90vh] overflow-hidden p-0 rounded-md">
            <DialogTitle className="sr-only">Story</DialogTitle>

            <Story
              className="relative size-full "
              duration={5000}
              mediaLength={SHADCN_STORIES.length}
            >
              <DialogHeader className="px-4 py-6">
                <div className="relative z-10 flex items-center gap-2">
                  <Avatar className="size-10">
                    <AvatarImage
                      src="https://cdn.21st.dev/assets/mirror/e9/e941f86e0ca2b1c9eefd47618a92040443f85598470bad47399cee7f259c0756.jpg"
                      alt="@shadcn"
                    />
                    <AvatarFallback>SC</AvatarFallback>
                  </Avatar>

                  <StoryProgress
                    className="flex-1"
                    progressWrapClass="h-1.5"
                    progressActiveClass="bg-pink-500"
                  />
                  <StoryControls
                    variant="ghost"
                    className="text-white rounded-full"
                  />
                </div>
              </DialogHeader>
              {SHADCN_STORIES.map((story, idx) => (
                <StorySlide
                  key={idx}
                  index={idx}
                  className="absolute inset-0 size-full"
                >
                  <img
                    src={story.storyImage}
                    className="w-full h-auto max-h-auto"
                    alt={story.title}
                  />

                  <div className="absolute bottom-4 left-4  z-10 space-y-1 text-white p-4">
                    <a
                      className="text-secondary"
                      href="https://x.com/FabrizioRomano"
                    >
                      @Shadcn
                    </a>
                    <h3 className="text-medium tracking-tight text-foreground-muted">
                      {story.title}
                    </h3>
                    <p className="text-sm text-slate-200">{story.caption}</p>
                  </div>
                </StorySlide>
              ))}
              <StoryOverlay />
            </Story>
          </DialogContent>
        </Dialog>

        <Dialog>
          <DialogTrigger>
            <Avatar className="size-12">
              <AvatarImage
                src="https://cdn.21st.dev/assets/mirror/14/142642fb731b152d2c4903291404c61859cc41ac9595180975ad57b193dce007.jpg"
                alt="@nba"
              />
              <AvatarFallback>SC</AvatarFallback>
            </Avatar>
          </DialogTrigger>
          <DialogContent className="aspect-[12/16] w-auto h-[90vh] overflow-hidden p-0 rounded-md">
            <DialogTitle className="sr-only">Story</DialogTitle>

            <Story
              className="relative size-full "
              duration={8000}
              mediaLength={NBA_STORIES.length}
            >
              <DialogHeader className="px-4 py-6">
                <div className="relative z-10 flex items-center gap-2">
                  <Avatar className="size-10">
                    <AvatarImage
                      src="https://cdn.21st.dev/assets/mirror/14/142642fb731b152d2c4903291404c61859cc41ac9595180975ad57b193dce007.jpg"
                      alt="@nba"
                    />
                    <AvatarFallback>SC</AvatarFallback>
                  </Avatar>

                  <StoryProgress
                    className="flex-1"
                    progressWrapClass="h-1.5"
                    progressActiveClass="bg-red-500"
                  />
                  <StoryControls
                    variant="ghost"
                    className="text-white rounded-full"
                  />
                </div>
              </DialogHeader>
              {NBA_STORIES.map((story, idx) => (
                <StorySlide
                  key={idx}
                  index={idx}
                  className="absolute inset-0 size-full"
                >
                  <img
                    src={story.storyImage}
                    className="w-full h-auto max-h-auto"
                    alt={story.title}
                  />

                  <div className="absolute bottom-4 left-4  z-10 space-y-1 text-white p-4">
                    <a
                      className="text-secondary"
                      href="https://x.com/FabrizioRomano"
                    >
                      @nba
                    </a>
                    <h3 className="text-medium tracking-tight text-foreground-muted">
                      {story.title}
                    </h3>
                    <p className="text-sm text-slate-200">{story.caption}</p>
                  </div>
                </StorySlide>
              ))}
              <StoryOverlay />
            </Story>
          </DialogContent>
        </Dialog>
      </div>
    </section>;
};

export { StoryDemo };
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add avatar button dialog
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
