<!-- Image Crop · @haydenbleasel · https://21st.dev/@haydenbleasel/components/image-crop
     license: MIT · category: image
     An interactive image cropper with customizable aspect ratios, circular crop mode, and apply/reset controls that returns the cropped result as a PNG data URL. -->

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
"use client";

import { Button } from "@repo/shadcn-ui/components/ui/button";
import { CropIcon, RotateCcwIcon } from "lucide-react";
import { Slot } from "radix-ui";
import {
  type ComponentProps,
  type CSSProperties,
  createContext,
  type MouseEvent,
  type ReactNode,
  type RefObject,
  type SyntheticEvent,
  useCallback,
  useContext,
  useEffect,
  useRef,
  useState,
} from "react";
import ReactCrop, {
  centerCrop,
  makeAspectCrop,
  type PercentCrop,
  type PixelCrop,
  type ReactCropProps,
} from "react-image-crop";
import { cn } from "@/lib/utils";

import "react-image-crop/dist/ReactCrop.css";

const centerAspectCrop = (
  mediaWidth: number,
  mediaHeight: number,
  aspect: number | undefined
): PercentCrop =>
  centerCrop(
    aspect
      ? makeAspectCrop(
          {
            unit: "%",
            width: 90,
          },
          aspect,
          mediaWidth,
          mediaHeight
        )
      : { x: 0, y: 0, width: 90, height: 90, unit: "%" },
    mediaWidth,
    mediaHeight
  );

const getCroppedPngImage = async (
  imageSrc: HTMLImageElement,
  scaleFactor: number,
  pixelCrop: PixelCrop,
  maxImageSize: number
): Promise<string> => {
  const canvas = document.createElement("canvas");
  const ctx = canvas.getContext("2d");

  if (!ctx) {
    throw new Error("Context is null, this should never happen.");
  }

  const scaleX = imageSrc.naturalWidth / imageSrc.width;
  const scaleY = imageSrc.naturalHeight / imageSrc.height;

  ctx.imageSmoothingEnabled = false;
  canvas.width = pixelCrop.width;
  canvas.height = pixelCrop.height;

  ctx.drawImage(
    imageSrc,
    pixelCrop.x * scaleX,
    pixelCrop.y * scaleY,
    pixelCrop.width * scaleX,
    pixelCrop.height * scaleY,
    0,
    0,
    canvas.width,
    canvas.height
  );

  const croppedImageUrl = canvas.toDataURL("image/png");
  const response = await fetch(croppedImageUrl);
  const blob = await response.blob();

  if (blob.size > maxImageSize) {
    return await getCroppedPngImage(
      imageSrc,
      scaleFactor * 0.9,
      pixelCrop,
      maxImageSize
    );
  }

  return croppedImageUrl;
};

type ImageCropContextType = {
  file: File;
  maxImageSize: number;
  imgSrc: string;
  crop: PercentCrop | undefined;
  completedCrop: PixelCrop | null;
  imgRef: RefObject<HTMLImageElement | null>;
  onCrop?: (croppedImage: string) => void;
  reactCropProps: Omit<ReactCropProps, "onChange" | "onComplete" | "children">;
  handleChange: (pixelCrop: PixelCrop, percentCrop: PercentCrop) => void;
  handleComplete: (
    pixelCrop: PixelCrop,
    percentCrop: PercentCrop
  ) => Promise<void>;
  onImageLoad: (e: SyntheticEvent<HTMLImageElement>) => void;
  applyCrop: () => Promise<void>;
  resetCrop: () => void;
};

const ImageCropContext = createContext<ImageCropContextType | null>(null);

const useImageCrop = () => {
  const context = useContext(ImageCropContext);
  if (!context) {
    throw new Error("ImageCrop components must be used within ImageCrop");
  }
  return context;
};

export type ImageCropProps = {
  file: File;
  maxImageSize?: number;
  onCrop?: (croppedImage: string) => void;
  children: ReactNode;
  onChange?: ReactCropProps["onChange"];
  onComplete?: ReactCropProps["onComplete"];
} & Omit<ReactCropProps, "onChange" | "onComplete" | "children">;

export const ImageCrop = ({
  file,
  maxImageSize = 1024 * 1024 * 5,
  onCrop,
  children,
  onChange,
  onComplete,
  ...reactCropProps
}: ImageCropProps) => {
  const imgRef = useRef<HTMLImageElement | null>(null);
  const [imgSrc, setImgSrc] = useState<string>("");
  const [crop, setCrop] = useState<PercentCrop>();
  const [completedCrop, setCompletedCrop] = useState<PixelCrop | null>(null);
  const [initialCrop, setInitialCrop] = useState<PercentCrop>();

  useEffect(() => {
    const reader = new FileReader();
    reader.addEventListener("load", () =>
      setImgSrc(reader.result?.toString() || "")
    );
    reader.readAsDataURL(file);
  }, [file]);

  const onImageLoad = useCallback(
    (e: SyntheticEvent<HTMLImageElement>) => {
      const { width, height } = e.currentTarget;
      const newCrop = centerAspectCrop(width, height, reactCropProps.aspect);
      setCrop(newCrop);
      setInitialCrop(newCrop);
    },
    [reactCropProps.aspect]
  );

  const handleChange = (pixelCrop: PixelCrop, percentCrop: PercentCrop) => {
    setCrop(percentCrop);
    onChange?.(pixelCrop, percentCrop);
  };

  // biome-ignore lint/suspicious/useAwait: "onComplete is async"
  const handleComplete = async (
    pixelCrop: PixelCrop,
    percentCrop: PercentCrop
  ) => {
    setCompletedCrop(pixelCrop);
    onComplete?.(pixelCrop, percentCrop);
  };

  const applyCrop = async () => {
    if (!(imgRef.current && completedCrop)) {
      return;
    }

    const croppedImage = await getCroppedPngImage(
      imgRef.current,
      1,
      completedCrop,
      maxImageSize
    );

    onCrop?.(croppedImage);
  };

  const resetCrop = () => {
    if (initialCrop) {
      setCrop(initialCrop);
      setCompletedCrop(null);
    }
  };

  const contextValue: ImageCropContextType = {
    file,
    maxImageSize,
    imgSrc,
    crop,
    completedCrop,
    imgRef,
    onCrop,
    reactCropProps,
    handleChange,
    handleComplete,
    onImageLoad,
    applyCrop,
    resetCrop,
  };

  return (
    <ImageCropContext.Provider value={contextValue}>
      {children}
    </ImageCropContext.Provider>
  );
};

export type ImageCropContentProps = {
  style?: CSSProperties;
  className?: string;
};

export const ImageCropContent = ({
  style,
  className,
}: ImageCropContentProps) => {
  const {
    imgSrc,
    crop,
    handleChange,
    handleComplete,
    onImageLoad,
    imgRef,
    reactCropProps,
  } = useImageCrop();

  const shadcnStyle = {
    "--rc-border-color": "var(--color-border)",
    "--rc-focus-color": "var(--color-primary)",
  } as CSSProperties;

  return (
    <ReactCrop
      className={cn("max-h-[277px] max-w-full", className)}
      crop={crop}
      onChange={handleChange}
      onComplete={handleComplete}
      style={{ ...shadcnStyle, ...style }}
      {...reactCropProps}
    >
      {imgSrc && (
        <img
          alt="crop"
          className="size-full"
          onLoad={onImageLoad}
          ref={imgRef}
          src={imgSrc}
        />
      )}
    </ReactCrop>
  );
};

export type ImageCropApplyProps = ComponentProps<"button"> & {
  asChild?: boolean;
};

export const ImageCropApply = ({
  asChild = false,
  children,
  onClick,
  ...props
}: ImageCropApplyProps) => {
  const { applyCrop } = useImageCrop();

  const handleClick = async (e: MouseEvent<HTMLButtonElement>) => {
    await applyCrop();
    onClick?.(e);
  };

  if (asChild) {
    return (
      <Slot.Root onClick={handleClick} {...props}>
        {children}
      </Slot.Root>
    );
  }

  return (
    <Button onClick={handleClick} size="icon" variant="ghost" {...props}>
      {children ?? <CropIcon className="size-4" />}
    </Button>
  );
};

export type ImageCropResetProps = ComponentProps<"button"> & {
  asChild?: boolean;
};

export const ImageCropReset = ({
  asChild = false,
  children,
  onClick,
  ...props
}: ImageCropResetProps) => {
  const { resetCrop } = useImageCrop();

  const handleClick = (e: MouseEvent<HTMLButtonElement>) => {
    resetCrop();
    onClick?.(e);
  };

  if (asChild) {
    return (
      <Slot.Root onClick={handleClick} {...props}>
        {children}
      </Slot.Root>
    );
  }

  return (
    <Button onClick={handleClick} size="icon" variant="ghost" {...props}>
      {children ?? <RotateCcwIcon className="size-4" />}
    </Button>
  );
};

// Keep the original Cropper component for backward compatibility
export type CropperProps = Omit<ReactCropProps, "onChange"> & {
  file: File;
  maxImageSize?: number;
  onCrop?: (croppedImage: string) => void;
  onChange?: ReactCropProps["onChange"];
};

export const Cropper = ({
  onChange,
  onComplete,
  onCrop,
  style,
  className,
  file,
  maxImageSize,
  ...props
}: CropperProps) => (
  <ImageCrop
    file={file}
    maxImageSize={maxImageSize}
    onChange={onChange}
    onComplete={onComplete}
    onCrop={onCrop}
    {...props}
  >
    <ImageCropContent className={className} style={style} />
  </ImageCrop>
);

demo.tsx
"use client";

import {
  ImageCrop,
  ImageCropApply,
  ImageCropContent,
  ImageCropReset,
} from "@/components/ui/image-crop";
import { Button } from "@/components/ui/button";
import { XIcon } from "lucide-react";
import Image from "next/image";
import { useEffect, useState } from "react";

// Build a self-contained sample image (no network) so the cropper is shown live.
const createSampleFile = () =>
  new Promise<File | null>((resolve) => {
    const canvas = document.createElement("canvas");
    canvas.width = 640;
    canvas.height = 480;
    const ctx = canvas.getContext("2d");
    if (!ctx) {
      resolve(null);
      return;
    }

    // Sky gradient
    const sky = ctx.createLinearGradient(0, 0, 0, 480);
    sky.addColorStop(0, "#0ea5e9");
    sky.addColorStop(1, "#bae6fd");
    ctx.fillStyle = sky;
    ctx.fillRect(0, 0, 640, 480);

    // Sun
    ctx.fillStyle = "#fde047";
    ctx.beginPath();
    ctx.arc(470, 130, 62, 0, Math.PI * 2);
    ctx.fill();

    // Far hills
    ctx.fillStyle = "#4ade80";
    ctx.beginPath();
    ctx.moveTo(0, 480);
    ctx.quadraticCurveTo(160, 290, 340, 390);
    ctx.quadraticCurveTo(520, 470, 640, 340);
    ctx.lineTo(640, 480);
    ctx.closePath();
    ctx.fill();

    // Near hills
    ctx.fillStyle = "#16a34a";
    ctx.beginPath();
    ctx.moveTo(0, 480);
    ctx.quadraticCurveTo(220, 380, 430, 440);
    ctx.quadraticCurveTo(560, 470, 640, 445);
    ctx.lineTo(640, 480);
    ctx.closePath();
    ctx.fill();

    canvas.toBlob((blob) => {
      if (!blob) {
        resolve(null);
        return;
      }
      resolve(new File([blob], "sample.png", { type: "image/png" }));
    }, "image/png");
  });

const Example = () => {
  const [selectedFile, setSelectedFile] = useState<File | null>(null);
  const [croppedImage, setCroppedImage] = useState<string | null>(null);

  useEffect(() => {
    let active = true;
    createSampleFile().then((file) => {
      if (active && file) {
        setSelectedFile(file);
      }
    });
    return () => {
      active = false;
    };
  }, []);

  const handleReset = async () => {
    setCroppedImage(null);
    const file = await createSampleFile();
    setSelectedFile(file);
  };

  if (croppedImage) {
    return (
      <div className="space-y-4">
        <Image
          alt="Cropped"
          height={160}
          src={croppedImage}
          unoptimized
          width={160}
        />
        <Button onClick={handleReset} size="icon" type="button" variant="ghost">
          <XIcon className="size-4" />
        </Button>
      </div>
    );
  }

  if (!selectedFile) {
    return null;
  }

  return (
    <div className="space-y-4">
      <ImageCrop
        aspect={1}
        file={selectedFile}
        maxImageSize={1024 * 1024} // 1MB
        onChange={console.log}
        onComplete={console.log}
        onCrop={setCroppedImage}
      >
        <ImageCropContent className="max-w-md" />
        <div className="flex items-center gap-2">
          <ImageCropApply />
          <ImageCropReset />
          <Button
            onClick={handleReset}
            size="icon"
            type="button"
            variant="ghost"
          >
            <XIcon className="size-4" />
          </Button>
        </div>
      </ImageCrop>
    </div>
  );
};

export default Example;
```

Install NPM dependencies:
```bash
npm install lucide-react radix-ui react-image-crop
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button input
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
