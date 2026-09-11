<!-- Information Card · @KarrixLee · https://21st.dev/@KarrixLee/components/info-card
     license: MIT · category: announcement
     Information cards can serve as callout cards, banners, toast notifications, or announcement boxes to deliver news, updates, and alerts to your users. -->

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
components/ui/info-card.tsx
"use client";

import {
  useState,
  useRef,
  useEffect,
  createContext,
  useContext,
  useMemo,
  useCallback,
} from "react";
import { motion, AnimatePresence } from "motion/react";
import { cn } from "@/lib/utils";
import React from "react";

interface InfoCardTitleProps extends React.HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode;
}

interface InfoCardDescriptionProps
  extends React.HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode;
}

const InfoCardTitle = React.memo(
  ({ children, className, ...props }: InfoCardTitleProps) => {
    return (
      <div className={cn("font-medium mb-1", className)} {...props}>
        {children}
      </div>
    );
  }
);
InfoCardTitle.displayName = "InfoCardTitle";

const InfoCardDescription = React.memo(
  ({ children, className, ...props }: InfoCardDescriptionProps) => {
    return (
      <div
        className={cn("text-muted-foreground leading-4", className)}
        {...props}
      >
        {children}
      </div>
    );
  }
);
InfoCardDescription.displayName = "InfoCardDescription";

interface CommonCardProps extends React.HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode;
}

interface InfoCardProps extends React.HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode;
  storageKey?: string;
  dismissType?: "once" | "forever";
}

type InfoCardContentProps = CommonCardProps;
type InfoCardFooterProps = CommonCardProps;
type InfoCardDismissProps = React.HTMLAttributes<HTMLDivElement> & {
  children: React.ReactNode;
  onDismiss?: () => void;
};
type InfoCardActionProps = CommonCardProps;

const InfoCardContent = React.memo(
  ({ children, className, ...props }: InfoCardContentProps) => {
    return (
      <div className={cn("flex flex-col gap-1 text-xs", className)} {...props}>
        {children}
      </div>
    );
  }
);
InfoCardContent.displayName = "InfoCardContent";

interface MediaItem {
  type?: "image" | "video";
  src: string;
  alt?: string;
  className?: string;
  [key: string]: any;
}

interface InfoCardMediaProps extends React.HTMLAttributes<HTMLDivElement> {
  media: MediaItem[];
  loading?: "eager" | "lazy";
  shrinkHeight?: number;
  expandHeight?: number;
}

const InfoCardImageContext = createContext<{
  handleMediaLoad: (mediaSrc: string) => void;
  setAllImagesLoaded: (loaded: boolean) => void;
}>({
  handleMediaLoad: () => {},
  setAllImagesLoaded: () => {},
});

const InfoCardContext = createContext<{
  isHovered: boolean;
  onDismiss: () => void;
}>({
  isHovered: false,
  onDismiss: () => {},
});

function InfoCard({
  children,
  className,
  storageKey,
  dismissType = "once",
}: InfoCardProps) {
  if (dismissType === "forever" && !storageKey) {
    throw new Error(
      'A storageKey must be provided when using dismissType="forever"'
    );
  }

  const [isHovered, setIsHovered] = useState(false);
  const [allImagesLoaded, setAllImagesLoaded] = useState(true);
  const [isDismissed, setIsDismissed] = useState(() => {
    if (typeof window === "undefined" || dismissType === "once") return false;
    return dismissType === "forever"
      ? localStorage.getItem(storageKey!) === "dismissed"
      : false;
  });

  const handleDismiss = useCallback(() => {
    setIsDismissed(true);
    if (dismissType === "forever") {
      localStorage.setItem(storageKey!, "dismissed");
    }
  }, [storageKey, dismissType]);

  const imageContextValue = useMemo(
    () => ({
      handleMediaLoad: () => {},
      setAllImagesLoaded,
    }),
    [setAllImagesLoaded]
  );

  const cardContextValue = useMemo(
    () => ({
      isHovered,
      onDismiss: handleDismiss,
    }),
    [isHovered, handleDismiss]
  );

  return (
    <InfoCardContext.Provider value={cardContextValue}>
      <InfoCardImageContext.Provider value={imageContextValue}>
        <AnimatePresence>
          {!isDismissed && (
            <motion.div
              initial={{ opacity: 0, y: 10 }}
              animate={{
                opacity: allImagesLoaded ? 1 : 0,
                y: allImagesLoaded ? 0 : 10,
              }}
              exit={{
                opacity: 0,
                y: 10,
                transition: { duration: 0.2 },
              }}
              transition={{ duration: 0.3, delay: 0 }}
              className={cn(
                "group rounded-lg border p-3",
                "bg-white",
                "dark:bg-gradient-to-br dark:from-zinc-800 dark:to-zinc-900",
                "dark:border-zinc-700",
                className
              )}
              onMouseEnter={() => setIsHovered(true)}
              onMouseLeave={() => setIsHovered(false)}
            >
              {children}
            </motion.div>
          )}
        </AnimatePresence>
      </InfoCardImageContext.Provider>
    </InfoCardContext.Provider>
  );
}

const InfoCardFooter = ({ children, className }: InfoCardFooterProps) => {
  const { isHovered } = useContext(InfoCardContext);

  return (
    <motion.div
      className={cn(
        "flex justify-between text-xs text-muted-foreground",
        className
      )}
      initial={{ opacity: 0, height: "0px" }}
      animate={{
        opacity: isHovered ? 1 : 0,
        height: isHovered ? "auto" : "0px",
      }}
      transition={{
        type: "spring",
        stiffness: 300,
        damping: 30,
        duration: 0.3,
      }}
    >
      {children}
    </motion.div>
  );
};

const InfoCardDismiss = React.memo(
  ({ children, className, onDismiss, ...props }: InfoCardDismissProps) => {
    const { onDismiss: contextDismiss } = useContext(InfoCardContext);

    const handleClick = (e: React.MouseEvent) => {
      e.preventDefault();
      onDismiss?.();
      contextDismiss();
    };

    return (
      <div
        className={cn("cursor-pointer", className)}
        onClick={handleClick}
        {...props}
      >
        {children}
      </div>
    );
  }
);
InfoCardDismiss.displayName = "InfoCardDismiss";

const InfoCardAction = React.memo(
  ({ children, className, ...props }: InfoCardActionProps) => {
    return (
      <div className={cn("", className)} {...props}>
        {children}
      </div>
    );
  }
);
InfoCardAction.displayName = "InfoCardAction";

const InfoCardMedia = ({
  media = [],
  className,
  loading = undefined,
  shrinkHeight = 75,
  expandHeight = 150,
}: InfoCardMediaProps) => {
  const { isHovered } = useContext(InfoCardContext);
  const { setAllImagesLoaded } = useContext(InfoCardImageContext);
  const [isOverflowVisible, setIsOverflowVisible] = useState(false);
  const loadedMedia = useRef(new Set());

  const handleMediaLoad = (mediaSrc: string) => {
    loadedMedia.current.add(mediaSrc);
    if (loadedMedia.current.size === Math.min(3, media.slice(0, 3).length)) {
      setAllImagesLoaded(true);
    }
  };

  const processedMedia = useMemo(
    () =>
      media.map((item) => ({
        ...item,
        type: item.type || "image",
      })),
    [media]
  );

  const displayMedia = useMemo(
    () => processedMedia.slice(0, 3),
    [processedMedia]
  );

  useEffect(() => {
    if (media.length > 0) {
      setAllImagesLoaded(false);
      loadedMedia.current.clear();
    } else {
      setAllImagesLoaded(true); // No media to load
    }
  }, [media.length]);

  useEffect(() => {
    let timeoutId: NodeJS.Timeout;
    if (isHovered) {
      timeoutId = setTimeout(() => {
        setIsOverflowVisible(true);
      }, 100);
    } else {
      setIsOverflowVisible(false);
    }
    return () => clearTimeout(timeoutId);
  }, [isHovered]);

  const mediaCount = displayMedia.length;

  const getRotation = (index: number) => {
    if (!isHovered || mediaCount === 1) return 0;
    return (index - (mediaCount === 2 ? 0.5 : 1)) * 5;
  };

  const getTranslateX = (index: number) => {
    if (!isHovered || mediaCount === 1) return 0;
    return (index - (mediaCount === 2 ? 0.5 : 1)) * 20;
  };

  const getTranslateY = (index: number) => {
    if (!isHovered) return 0;
    if (mediaCount === 1) return -5;
    return index === 0 ? -10 : index === 1 ? -5 : 0;
  };

  const getScale = (index: number) => {
    if (!isHovered) return 1;
    return mediaCount === 1 ? 1 : 0.95 + index * 0.02;
  };

  return (
    <InfoCardImageContext.Provider
      value={{
        handleMediaLoad,
        setAllImagesLoaded,
      }}
    >
      <motion.div
        className={cn("relative mt-2 rounded-md", className)}
        animate={{
          height:
            media.length > 0
              ? isHovered
                ? expandHeight
                : shrinkHeight
              : "auto",
        }}
        style={{
          overflow: isOverflowVisible ? "visible" : "hidden",
        }}
        transition={{
          type: "spring",
          stiffness: 300,
          damping: 30,
          duration: 0.3,
        }}
      >
        <div
          className={cn(
            "relative",
            media.length > 0 ? { height: shrinkHeight } : "h-auto"
          )}
        >
          {displayMedia.map((item, index) => {
            const {
              type,
              src,
              alt,
              className: itemClassName,
              ...mediaProps
            } = item;

            return (
              <motion.div
                key={src}
                className="absolute w-full"
                animate={{
                  rotateZ: getRotation(index),
                  x: getTranslateX(index),
                  y: getTranslateY(index),
                  scale: getScale(index),
                }}
                transition={{
                  type: "spring",
                  stiffness: 300,
                  damping: 30,
                }}
              >
                {type === "video" ? (
                  <video
                    src={src}
                    className={cn(
                      "w-full rounded-md border border-gray-200 dark:border-zinc-700/10 object-cover shadow-lg",
                      itemClassName
                    )}
                    onLoadedData={() => handleMediaLoad(src)}
                    preload="metadata"
                    muted
                    playsInline
                    {...mediaProps}
                  />
                ) : (
                  <img
                    src={src}
                    alt={alt}
                    className={cn(
                      "w-full rounded-md border border-gray-200 dark:border-zinc-700/10 object-cover shadow-lg",
                      itemClassName
                    )}
                    onLoad={() => handleMediaLoad(src)}
                    loading={loading}
                    {...mediaProps}
                  />
                )}
              </motion.div>
            );
          })}
        </div>

        <motion.div
          className="absolute right-0 bottom-0 left-0 h-10 bg-gradient-to-b from-transparent to-white dark:to-zinc-900"
          animate={{ opacity: isHovered ? 0 : 1 }}
          transition={{
            type: "spring",
            stiffness: 300,
            damping: 30,
            duration: 0.3,
          }}
        />
      </motion.div>
    </InfoCardImageContext.Provider>
  );
};

export {
  InfoCard,
  InfoCardTitle,
  InfoCardDescription,
  InfoCardContent,
  InfoCardMedia,
  InfoCardFooter,
  InfoCardDismiss,
  InfoCardAction,
};

demo.tsx
import {
  InfoCard,
  InfoCardContent,
  InfoCardTitle,
  InfoCardDescription,
  InfoCardMedia,
  InfoCardFooter,
  InfoCardDismiss,
  InfoCardAction,
} from "@/components/ui/info-card";
import {
  Sidebar,
  SidebarProvider,
  SidebarContent,
  SidebarGroup,
  SidebarGroupLabel,
  SidebarGroupContent,
  SidebarMenu,
  SidebarMenuItem,
  SidebarMenuButton,
  SidebarFooter,
  SidebarTrigger,
} from "@/components/blocks/sidebar";
import {
  ExternalLink,
  User,
  ChevronsUpDown,
  Calendar,
  Home,
  Inbox,
  Search,
  Settings,
} from "lucide-react";
import Link from "next/link";
// Menu items.
const items = [
  {
    title: "Home",
    url: "#",
    icon: Home,
  },
  {
    title: "Inbox",
    url: "#",
    icon: Inbox,
  },
  {
    title: "Calendar",
    url: "#",
    icon: Calendar,
  },
  {
    title: "Search",
    url: "#",
    icon: Search,
  },
  {
    title: "Settings",
    url: "#",
    icon: Settings,
  },
];

export function InfoCardDemo() {
  return (
    <SidebarProvider>
      <Sidebar>
        <SidebarContent>
          <SidebarGroup>
            <SidebarGroupLabel>Application</SidebarGroupLabel>
            <SidebarGroupContent>
              <SidebarMenu>
                {items.map((item) => (
                  <SidebarMenuItem key={item.title}>
                    <SidebarMenuButton asChild>
                      <a href={item.url}>
                        <item.icon />
                        <span>{item.title}</span>
                      </a>
                    </SidebarMenuButton>
                  </SidebarMenuItem>
                ))}
              </SidebarMenu>
            </SidebarGroupContent>
          </SidebarGroup>
        </SidebarContent>
        <SidebarFooter>
          <InfoCard>
            <InfoCardContent>
              <InfoCardTitle>Introducing New Dashboard</InfoCardTitle>
              <InfoCardDescription>
                New Feature. New Platform. Same Feel.
              </InfoCardDescription>
              <InfoCardMedia
                media={[
                  {
                    src: "https://cdn.21st.dev/assets/mirror/a1/a12526ad6ce4aa3cac9d4f005480e4c2aa5a4d3cccf7b860ca68ebea82c4c157.webp",
                  },
                  {
                    src: "https://cdn.21st.dev/assets/mirror/90/908d2bed189c6957365043783152c6347defce5b15eccacf195507176306b08f.webp",
                  },
                  {
                    src: "https://cdn.21st.dev/assets/mirror/a2/a23cdd989b1a11901eb1275f59149b466783492dbde051a57e41382b5bd94896.webp",
                  },
                ]}
              />
              <InfoCardFooter>
                <InfoCardDismiss>Dismiss</InfoCardDismiss>
                <InfoCardAction>
                  <Link
                    href="#"
                    className="flex flex-row items-center gap-1 underline"
                  >
                    Try it out <ExternalLink size={12} />
                  </Link>
                </InfoCardAction>
              </InfoCardFooter>
            </InfoCardContent>
          </InfoCard>
          <SidebarGroup>
            <SidebarMenuButton className="w-full justify-between gap-3 h-12">
              <div className="flex items-center gap-2">
                <User className="h-5 w-5 rounded-md" />
                <div className="flex flex-col items-start">
                  <span className="text-sm font-medium">KL</span>
                  <span className="text-xs text-muted-foreground">
                    kl@example.com
                  </span>
                </div>
              </div>
              <ChevronsUpDown className="h-5 w-5 rounded-md" />
            </SidebarMenuButton>
          </SidebarGroup>
        </SidebarFooter>
      </Sidebar>
      <div className="px-4 py-2">
        <SidebarTrigger className="size-3 pt-2" />
      </div>
    </SidebarProvider>
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
