<!-- Image Preview Dropzone · @uilayout.contact · https://21st.dev/@uilayout.contact/components/imgpreview-dropzone
     license: MIT · category: upload-download
     A drag-and-drop file uploader that shows thumbnail previews of selected images with per-file remove controls. -->

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
components/ui/img-preview.tsx
'use client';

import {
  FileInput,
  FileUploader,
  FileUploaderContent,
  FileUploaderItem,
} from '@/components/ui/file-upload';
import Image from 'next/image';
import { useState } from 'react';
import type { DropzoneOptions } from 'react-dropzone';

const FileSvgDraw = () => {
  return (
    <>
      <svg
        className='w-8 h-8 mb-3 text-primary'
        aria-hidden='true'
        xmlns='http://www.w3.org/2000/svg'
        fill='none'
        viewBox='0 0 20 16'
      >
        <path
          stroke='currentColor'
          strokeLinecap='round'
          strokeLinejoin='round'
          strokeWidth='2'
          d='M13 13h3a3 3 0 0 0 0-6h-.025A5.56 5.56 0 0 0 16 6.5 5.5 5.5 0 0 0 5.207 5.021C5.137 5.017 5.071 5 5 5a4 4 0 0 0 0 8h2.167M10 15V6m0 0L8 8m2-2 2 2'
        />
      </svg>
      <p className='mb-1 text-sm  text-primary'>
        <span className='font-semibold'>Click to upload</span>
        &nbsp; or drag and drop
      </p>
      <p className='text-xs  text-primary'>SVG, PNG, JPG or GIF</p>
    </>
  );
};

const FileUploadDropzone = () => {
  const [files, setFiles] = useState<File[] | null>([]);

  const dropzone = {
    accept: {
      'image/*': ['.jpg', '.jpeg', '.png', '.gif'],
    },
    multiple: true,
    maxFiles: 4,
    maxSize: 1 * 1024 * 1024,
  } satisfies DropzoneOptions;

  return (
    <div className='py-16'>
      <FileUploader
        value={files}
        orientation='vertical'
        onValueChange={setFiles}
        dropzoneOptions={dropzone}
        className='relative  rounded-lg p-2 w-96 mx-auto'
      >
        <FileInput className='outline-dashed dark:bg-neutral-800 bg-neutral-50 outline-2 outline-primary/40'>
          <div className='flex items-center justify-center flex-col pt-3 pb-4 w-full '>
            <FileSvgDraw />
          </div>
        </FileInput>
        <FileUploaderContent className='flex items-center flex-row gap-2'>
          {files?.map((file, i) => (
            <FileUploaderItem
              key={`${file.name}-${file.lastModified}-${file.size}`}
              index={i}
              className='size-20 p-0 rounded-md overflow-hidden border'
              aria-roledescription={`file ${i + 1} containing ${file.name}`}
            >
              <Image
                src={URL.createObjectURL(file)}
                alt={file.name}
                height={80}
                width={80}
                className='size-20 rounded-md object-cover bg-primary'
              />
            </FileUploaderItem>
          ))}
        </FileUploaderContent>
      </FileUploader>
    </div>
  );
};

export default FileUploadDropzone;

components/ui/file-upload.tsx
'use client';

import { cn } from '@/lib/utils';
import { Trash2 as RemoveIcon } from 'lucide-react';
import {
  type Dispatch,
  type SetStateAction,
  createContext,
  forwardRef,
  useCallback,
  useContext,
  useEffect,
  useRef,
  useState,
} from 'react';
import {
  type DropzoneOptions,
  type DropzoneState,
  type FileRejection,
  useDropzone,
} from 'react-dropzone';
import { toast } from 'sonner';

type DirectionOptions = 'rtl' | 'ltr' | undefined;

type FileUploaderContextType = {
  dropzoneState: DropzoneState;
  isLOF: boolean;
  isFileTooBig: boolean;
  removeFileFromSet: (index: number) => void;
  activeIndex: number;
  setActiveIndex: Dispatch<SetStateAction<number>>;
  orientation: 'horizontal' | 'vertical';
  direction: DirectionOptions;
};

const FileUploaderContext = createContext<FileUploaderContextType | null>(null);

export const useFileUpload = () => {
  const context = useContext(FileUploaderContext);
  if (!context) {
    throw new Error('useFileUpload must be used within a FileUploaderProvider');
  }
  return context;
};

type FileUploaderProps = {
  value: File[] | null;
  reSelect?: boolean;
  onValueChange: (value: File[] | null) => void;
  dropzoneOptions: DropzoneOptions;
  orientation?: 'horizontal' | 'vertical';
};

/**
 * File upload Docs: {@link: https://localhost:3000/docs/file-upload}
 */

export const FileUploader = forwardRef<
  HTMLDivElement,
  FileUploaderProps & React.HTMLAttributes<HTMLDivElement>
>(
  (
    {
      className,
      dropzoneOptions,
      value,
      onValueChange,
      reSelect,
      orientation = 'vertical',
      children,
      dir,
      ...props
    },
    ref
  ) => {
    const [isFileTooBig, setIsFileTooBig] = useState(false);
    const [isLOF, setIsLOF] = useState(false);
    const [activeIndex, setActiveIndex] = useState(-1);
    const {
      accept = {
        'image/*': ['.jpg', '.jpeg', '.png', '.gif'],
        'video/*': ['.mp4', '.MOV', '.AVI'],
      },
      maxFiles = 1,
      maxSize = 4 * 1024 * 1024,
      multiple = true,
    } = dropzoneOptions;

    const reSelectAll = maxFiles === 1 ? true : reSelect;
    const direction: DirectionOptions = dir === 'rtl' ? 'rtl' : 'ltr';

    const removeFileFromSet = useCallback(
      (i: number) => {
        if (!value) return;
        const newFiles = value.filter((_, index) => index !== i);
        onValueChange(newFiles);
      },
      [value, onValueChange]
    );

    const handleKeyDown = useCallback(
      (e: React.KeyboardEvent<HTMLDivElement>) => {
        e.preventDefault();
        e.stopPropagation();

        if (!value) return;

        const moveNext = () => {
          const nextIndex = activeIndex + 1;
          setActiveIndex(nextIndex > value.length - 1 ? 0 : nextIndex);
        };

        const movePrev = () => {
          const nextIndex = activeIndex - 1;
          setActiveIndex(nextIndex < 0 ? value.length - 1 : nextIndex);
        };

        const prevKey =
          orientation === 'horizontal'
            ? direction === 'ltr'
              ? 'ArrowLeft'
              : 'ArrowRight'
            : 'ArrowUp';

        const nextKey =
          orientation === 'horizontal'
            ? direction === 'ltr'
              ? 'ArrowRight'
              : 'ArrowLeft'
            : 'ArrowDown';

        if (e.key === nextKey) {
          moveNext();
        } else if (e.key === prevKey) {
          movePrev();
        } else if (e.key === 'Enter' || e.key === 'Space') {
          if (activeIndex === -1) {
            dropzoneState.inputRef.current?.click();
          }
        } else if (e.key === 'Delete' || e.key === 'Backspace') {
          if (activeIndex !== -1) {
            removeFileFromSet(activeIndex);
            if (value.length - 1 === 0) {
              setActiveIndex(-1);
              return;
            }
            movePrev();
          }
        } else if (e.key === 'Escape') {
          setActiveIndex(-1);
        }
      },
      // eslint-disable-next-line react-hooks/exhaustive-deps
      [value, activeIndex, removeFileFromSet]
    );

    const onDrop = useCallback(
      (acceptedFiles: File[], rejectedFiles: FileRejection[]) => {
        const files = acceptedFiles;

        if (!files) {
          toast.error('file error , probably too big');
          return;
        }

        const newValues: File[] = value ? [...value] : [];

        if (reSelectAll) {
          newValues.splice(0, newValues.length);
        }

        files.forEach((file) => {
          if (newValues.length < maxFiles) {
            newValues.push(file);
          }
        });

        onValueChange(newValues);

        if (rejectedFiles.length > 0) {
          for (let i = 0; i < rejectedFiles.length; i++) {
            if (rejectedFiles[i].errors[0]?.code === 'file-too-large') {
              toast.error(`File is too large. Max size is ${maxSize / 1024 / 1024}MB`);
              break;
            }
            if (rejectedFiles[i].errors[0]?.message) {
              toast.error(rejectedFiles[i].errors[0].message);
              break;
            }
          }
        }
      },
      // eslint-disable-next-line react-hooks/exhaustive-deps
      [reSelectAll, value]
    );

    useEffect(() => {
      if (!value) return;
      if (value.length === maxFiles) {
        setIsLOF(true);
        return;
      }
      setIsLOF(false);
    }, [value, maxFiles]);

    const opts = dropzoneOptions ? dropzoneOptions : { accept, maxFiles, maxSize, multiple };

    const dropzoneState = useDropzone({
      ...opts,
      onDrop,
      onDropRejected: () => setIsFileTooBig(true),
      onDropAccepted: () => setIsFileTooBig(false),
    });

    return (
      <FileUploaderContext.Provider
        value={{
          dropzoneState,
          isLOF,
          isFileTooBig,
          removeFileFromSet,
          activeIndex,
          setActiveIndex,
          orientation,
          direction,
        }}
      >
        <div
          ref={ref}
          tabIndex={0}
          onKeyDownCapture={handleKeyDown}
          className={cn('grid w-full focus:outline-hidden overflow-hidden ', className, {
            'gap-2': value && value.length > 0,
          })}
          dir={dir}
          {...props}
        >
          {children}
        </div>
      </FileUploaderContext.Provider>
    );
  }
);

FileUploader.displayName = 'FileUploader';

export const FileUploaderContent = forwardRef<HTMLDivElement, React.HTMLAttributes<HTMLDivElement>>(
  ({ children, className, ...props }, ref) => {
    const { orientation } = useFileUpload();
    const containerRef = useRef<HTMLDivElement>(null);

    return (
      <div className={cn('w-full px-1')} ref={containerRef} aria-description='content file holder'>
        <div
          {...props}
          ref={ref}
          className={cn(
            ' rounded-xl gap-1',
            orientation === 'horizontal' ? 'grid grid-cols-2' : 'flex flex-col',
            className
          )}
        >
          {children}
        </div>
      </div>
    );
  }
);

FileUploaderContent.displayName = 'FileUploaderContent';

export const FileUploaderItem = forwardRef<
  HTMLDivElement,
  { index: number } & React.HTMLAttributes<HTMLDivElement>
>(({ className, index, children, ...props }, ref) => {
  const { removeFileFromSet, activeIndex, direction } = useFileUpload();
  const isSelected = index === activeIndex;
  return (
    <div
      ref={ref}
      className={cn(
        'h-7 p-1 border rounded-md justify-between overflow-hidden  w-full cursor-pointer relative hover:bg-primary-foreground',
        className,
        isSelected ? 'bg-muted' : ''
      )}
      {...props}
    >
      <div className='font-medium   leading-none tracking-tight flex items-center gap-1.5 h-full w-full'>
        {children}
      </div>
      <button
        type='button'
        className={cn(
          'absolute bg-primary rounded-sm text-background p-1',
          direction === 'rtl' ? 'top-1 left-1' : 'top-[0.145em] right-1'
        )}
        onClick={() => removeFileFromSet(index)}
      >
        <span className='sr-only'>remove item {index}</span>
        <RemoveIcon className='w-3 h-3 hover:stroke-destructive  duration-200 ease-in-out' />
      </button>
    </div>
  );
});

FileUploaderItem.displayName = 'FileUploaderItem';

interface FileInputProps extends React.HTMLAttributes<HTMLDivElement> {
  parentclass?: string;
  dropmsg?: string;
}
export const FileInput = forwardRef<HTMLDivElement, FileInputProps>(
  ({ className, parentclass, dropmsg, children, ...props }, ref) => {
    const { dropzoneState, isFileTooBig, isLOF } = useFileUpload();
    const rootProps = isLOF ? {} : dropzoneState.getRootProps();

    return (
      <div
        ref={ref}
        {...props}
        className={cn(
          'relative w-full',
          parentclass,
          isLOF ? 'opacity-50 cursor-not-allowed' : 'cursor-pointer'
        )}
      >
        <div
          className={cn(
            'w-full rounded-lg transition-colors duration-300 ease-in-out',
            dropzoneState.isDragAccept && 'border-green-500 bg-green-50',
            dropzoneState.isDragReject && 'border-red-500 bg-red-50',
            isFileTooBig && 'border-red-500 bg-red-200',
            !dropzoneState.isDragActive && 'border-neutral-300 hover:border-neutral-400',
            className
          )}
          {...rootProps}
        >
          {children}
          {dropzoneState.isDragActive && (
            <div className='absolute inset-0 flex items-center justify-center bg-primary-foreground/60 backdrop-blur-xs rounded-lg'>
              <p className='text-primary font-medium'>Drop an image here.</p>
            </div>
          )}
        </div>
        <input
          ref={dropzoneState.inputRef}
          disabled={isLOF}
          {...dropzoneState.getInputProps()}
          className={cn(isLOF && 'cursor-not-allowed')}
        />
      </div>
    );
  }
);

demo.tsx
"use client";

import FileUploadDropzone from "@/components/ui/imgpreview-dropzone";
import { Toaster } from "sonner";

export default function Default() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center bg-background text-foreground">
      <FileUploadDropzone />
      <Toaster />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react react-dropzone
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
