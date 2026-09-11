<!-- Image Player · @youcefbnm · https://21st.dev/@youcefbnm/components/image-player
     license: unspecified · category: gallery
     - Image player component, flexible to use takes array of images urls and interval, also you can use any html image tag '<img />' (by default) or '<picture>' or use next 'Image' this on is very recommended (check the demo) as next optimized images makes load fast.
👉 Docs: https://systaliko-ui.vercel.app/docs/blocks/image-player -->

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
/* eslint-disable @next/next/no-img-element */
'use client';
import * as React from 'react';

interface ImagePlayerProps
  extends Omit<React.ImgHTMLAttributes<HTMLImageElement>, 'src'> {
  images: string[];
  interval?: number;
  loop?: boolean;
  onComplete?: () => void;
  renderImage?: (src: string, index: number) => React.ReactNode;

  renderLoading?: () => React.ReactNode;
}

export const ImagePlayer: React.FC<ImagePlayerProps> = ({
  images,
  interval = 500,
  loop = true,
  onComplete,
  renderImage,
  renderLoading,
  ...props
}) => {
  const [currentIndex, setCurrentIndex] = React.useState<number>(0);
  const [imagesLoaded, setImagesLoaded] = React.useState(false);
  const intervalRef = React.useRef<NodeJS.Timeout | null>(null);

  const currentImage = React.useMemo(
    () => images[currentIndex],
    [images, currentIndex],
  );

  React.useEffect(() => {
    const loadImage = (src: string): Promise<void> => {
      return new Promise((resolve) => {
        const img = document.createElement('img');
        img.src = src;
        img.onload = () => resolve();
      });
    };

    Promise.all(images.map(loadImage))
      .then(() => {
        setImagesLoaded(true);
      })
      .catch(() => {
        setImagesLoaded(true);
      });
  }, [images]);

  React.useEffect(() => {
    if (images.length <= 1 || !imagesLoaded) return;

    intervalRef.current = setInterval(() => {
      setCurrentIndex((prevIndex) => {
        const nextIndex = prevIndex + 1;

        if (nextIndex >= images.length) {
          if (loop) {
            return 0;
          } else {
            onComplete?.();
            return prevIndex;
          }
        }

        return nextIndex;
      });
    }, interval);

    return () => {
      if (intervalRef.current) {
        clearInterval(intervalRef.current);
      }
    };
  }, [images.length, interval, loop, onComplete, imagesLoaded]);

  React.useEffect(() => {
    setCurrentIndex(0);
  }, [images]);

  if (!images || images.length === 0) {
    return <div className="text-destructive">No images !!</div>;
  }

  if (!imagesLoaded) {
    return renderLoading ? (
      renderLoading()
    ) : (
      <div className="w-full h-full rounded-xl bg-gradient-to-r from-gray-200 to-gray-300 animate-pulse" />
    );
  }

  return (
    <>
      {renderImage ? (
        renderImage(currentImage, currentIndex)
      ) : (
        <img
          src={currentImage}
          alt={props.alt || 'Slideshow image'}
          {...props}
        />
      )}
    </>
  );
};

demo.tsx
'use client'
import { ImagePlayer } from "@/components/ui/image-player";
import Image from 'next/image';

const IMAGES = [
  'https://cdn.21st.dev/assets/mirror/e7/e7b015e46ffb9c1eedc33dc96aed89153f0b51dab58d99f4496fb14fc8ebe61c.jpg',
  'https://cdn.21st.dev/assets/mirror/4a/4a205b8a966ed612c8ab5a035843f7ecdf45f7becfe678d2ebd480a06e7a1fde.jpg',
  'https://cdn.21st.dev/assets/mirror/6d/6d5d6222fd6988a0360ddc2294c46a7fe9248eada9299bb035667c963f9a08b9.jpg',
  'https://cdn.21st.dev/assets/mirror/fc/fcb00c084011c54dca7a9b686e78f595060aa33f46f0bd3aa259b51fbd365026.jpg',
];

export default function DemoOne() {
  return (<div className="h-screen p-12 flex items-center justify-center">
      <ImagePlayer
        images={IMAGES}
        interval={200}
        renderImage={(src) => (
          <Image
            src={src}
            width={400}
            height={300}
            className="size-full h-auto max-h-full max-w-xl object-cover inline-block align-middle"
            alt="showcalse"
          />
        )}
      />
    </div>)
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
