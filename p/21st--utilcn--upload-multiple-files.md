<!-- Upload Multiple Files · @utilcn · https://21st.dev/@utilcn/components/upload-multiple-files
     license: no-license · category: upload-download
     A drag-and-drop dropzone to upload multiple files to a storage provider, with per-file previews and upload progress via a presigned upload URL. -->

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
components/ui/upload-multiple-files.tsx
import {
  AlertCircleIcon,
  FileArchiveIcon,
  FileIcon,
  FileSpreadsheetIcon,
  FileTextIcon,
  HeadphonesIcon,
  ImageIcon,
  Trash2Icon,
  UploadIcon,
  VideoIcon,
  XIcon,
} from 'lucide-react';
import { useState } from 'react';
import { Button } from '@/components/ui/button';
import {
  type FileWithPreview,
  formatBytes,
  useFileUpload,
} from '@/hooks/use-file-upload';
import { useUploadFile } from '@/registry/default/storage/use-upload-file';

const getFileIcon = (file: { file: File | { type: string; name: string } }) => {
  const fileType = file.file instanceof File ? file.file.type : file.file.type;
  const fileName = file.file instanceof File ? file.file.name : file.file.name;

  const iconMap = {
    pdf: {
      icon: FileTextIcon,
      conditions: (type: string, name: string) =>
        type.includes('pdf') ||
        name.endsWith('.pdf') ||
        type.includes('word') ||
        name.endsWith('.doc') ||
        name.endsWith('.docx'),
    },
    archive: {
      icon: FileArchiveIcon,
      conditions: (type: string, name: string) =>
        type.includes('zip') ||
        type.includes('archive') ||
        name.endsWith('.zip') ||
        name.endsWith('.rar'),
    },
    excel: {
      icon: FileSpreadsheetIcon,
      conditions: (type: string, name: string) =>
        type.includes('excel') ||
        name.endsWith('.xls') ||
        name.endsWith('.xlsx'),
    },
    video: {
      icon: VideoIcon,
      conditions: (type: string) => type.includes('video/'),
    },
    audio: {
      icon: HeadphonesIcon,
      conditions: (type: string) => type.includes('audio/'),
    },
    image: {
      icon: ImageIcon,
      conditions: (type: string) => type.startsWith('image/'),
    },
  };

  for (const { icon: Icon, conditions } of Object.values(iconMap)) {
    if (conditions(fileType, fileName)) {
      return <Icon className="size-5 opacity-60" />;
    }
  }

  return <FileIcon className="size-5 opacity-60" />;
};

const getFilePreview = (file: {
  file: File | { type: string; name: string; url?: string };
}) => {
  const fileType = file.file instanceof File ? file.file.type : file.file.type;
  const fileName = file.file instanceof File ? file.file.name : file.file.name;

  const renderImage = (src: string) => (
    <img
      alt={fileName}
      className="size-full rounded-t-[inherit] object-cover"
      src={src}
    />
  );

  return (
    <div className="flex aspect-square items-center justify-center overflow-hidden rounded-t-[inherit] bg-accent">
      {fileType.startsWith('image/') ? (
        file.file instanceof File ? (
          (() => {
            const previewUrl = URL.createObjectURL(file.file);
            return renderImage(previewUrl);
          })()
        ) : file.file.url ? (
          renderImage(file.file.url)
        ) : (
          <ImageIcon className="size-5 opacity-60" />
        )
      ) : (
        getFileIcon(file)
      )}
    </div>
  );
};

type UploadProgress = {
  fileId: string;
  progress: number;
  completed: boolean;
  error?: string;
  fileUrl?: string;
};

const BYTES_PER_KB = 1024;
const BYTES_PER_MB = BYTES_PER_KB * BYTES_PER_KB;
const MAX_SIZE_MB = 5;
const MAX_FILES = 6;

type FileItemProps = {
  file: FileWithPreview;
  uploadProgress?: UploadProgress;
  onRemove: (fileId: string) => void;
};

const FileItem = ({ file, uploadProgress, onRemove }: FileItemProps) => {
  const isUploading = uploadProgress && !uploadProgress.completed;

  return (
    <div
      className="flex flex-col gap-1 rounded-lg border bg-background p-2 pe-3 transition-opacity duration-300"
      data-uploading={isUploading || undefined}
    >
      <div className="flex items-center justify-between gap-2">
        <div className="flex items-center gap-3 overflow-hidden in-data-[uploading=true]:opacity-50">
          <div className="flex aspect-square size-10 shrink-0 items-center justify-center overflow-hidden rounded border">
            {(file.file instanceof File
              ? file.file.type
              : file.file.type
            ).startsWith('image/')
              ? getFilePreview(file)
              : getFileIcon(file)}
          </div>
          <div className="flex min-w-0 flex-col gap-0.5">
            <p className="truncate font-medium text-[13px]">
              {file.file instanceof File ? file.file.name : file.file.name}
            </p>
            <p className="text-muted-foreground text-xs">
              {formatBytes(
                file.file instanceof File ? file.file.size : file.file.size,
              )}
            </p>
          </div>
        </div>
        <Button
          aria-label="Remove file"
          className="-me-2 size-8 text-muted-foreground/80 hover:bg-transparent hover:text-foreground"
          onClick={() => onRemove(file.id)}
          size="icon"
          variant="ghost"
        >
          <XIcon aria-hidden="true" className="size-4" />
        </Button>
      </div>

      {uploadProgress &&
        (() => {
          const progress = uploadProgress.progress || 0;
          const completed = uploadProgress.completed;
          const hasError = uploadProgress.error;

          if (completed && !hasError) {
            return null;
          }

          return (
            <div className="mt-1 flex items-center gap-2">
              <div className="h-1.5 w-full overflow-hidden rounded-full bg-gray-100">
                <div
                  className={`h-full transition-all duration-300 ease-out ${
                    hasError ? 'bg-destructive' : 'bg-primary'
                  }`}
                  style={{ width: `${progress}%` }}
                />
              </div>
              <span className="w-10 text-muted-foreground text-xs tabular-nums">
                {hasError ? 'Error' : `${progress}%`}
              </span>
            </div>
          );
        })()}
    </div>
  );
};

export default function UploadMultipleFiles() {
  const maxSize = MAX_SIZE_MB * BYTES_PER_MB;

  const [uploadProgress, setUploadProgress] = useState<UploadProgress[]>([]);
  const { uploadFile } = useUploadFile();

  const handleFilesAdded = (addedFiles: FileWithPreview[]) => {
    const newProgressItems = addedFiles.map((file) => ({
      fileId: file.id,
      progress: 0,
      completed: false,
    }));

    setUploadProgress((prev) => [...prev, ...newProgressItems]);

    for (const file of addedFiles) {
      if (file.file instanceof File) {
        uploadFile({
          file: file.file,
          onProgress: (progress) => {
            setUploadProgress((prev) =>
              prev.map((item) =>
                item.fileId === file.id ? { ...item, progress } : item,
              ),
            );
          },
          onSuccess: (fileUrl) => {
            setUploadProgress((prev) =>
              prev.map((item) =>
                item.fileId === file.id
                  ? { ...item, completed: true, fileUrl }
                  : item,
              ),
            );
          },
          onError: (error) => {
            setUploadProgress((prev) =>
              prev.map((item) =>
                item.fileId === file.id
                  ? { ...item, error: error.message, completed: true }
                  : item,
              ),
            );
          },
        });
      }
    }
  };

  const handleFileRemoved = (fileId: string) => {
    setUploadProgress((prev) => prev.filter((item) => item.fileId !== fileId));
  };

  const [
    { files, isDragging, errors },
    {
      handleDragEnter,
      handleDragLeave,
      handleDragOver,
      handleDrop,
      openFileDialog,
      removeFile,
      clearFiles,
      getInputProps,
    },
  ] = useFileUpload({
    multiple: true,
    maxFiles: MAX_FILES,
    maxSize,
    onFilesAdded: handleFilesAdded,
  });

  return (
    <div className="flex flex-col gap-2">
      {/* Drop area */}
      <div
        className="relative flex min-h-52 flex-col items-center not-data-[files]:justify-center overflow-hidden rounded-xl border border-input border-dashed p-4 transition-colors has-[input:focus]:border-ring has-[input:focus]:ring-[3px] has-[input:focus]:ring-ring/50 data-[dragging=true]:bg-accent/50"
        data-dragging={isDragging || undefined}
        data-files={files.length > 0 || undefined}
        onDragEnter={handleDragEnter}
        onDragLeave={handleDragLeave}
        onDragOver={handleDragOver}
        onDrop={handleDrop}
        onKeyDown={(e) => {
          if (e.key === 'Enter' || e.key === ' ') {
            e.preventDefault();
            openFileDialog();
          }
        }}
      >
        <input
          {...getInputProps()}
          aria-label="Upload image file"
          className="sr-only"
        />
        {files.length > 0 ? (
          <div className="flex w-full flex-col gap-3">
            <div className="flex items-center justify-between gap-2">
              <h3 className="truncate font-medium text-sm">
                Files ({files.length})
              </h3>
              <div className="flex gap-2">
                <Button onClick={openFileDialog} size="sm" variant="outline">
                  <UploadIcon
                    aria-hidden="true"
                    className="-ms-0.5 size-3.5 opacity-60"
                  />
                  Add files
                </Button>
                <Button
                  onClick={() => {
                    setUploadProgress([]);
                    clearFiles();
                  }}
                  size="sm"
                  variant="outline"
                >
                  <Trash2Icon
                    aria-hidden="true"
                    className="-ms-0.5 size-3.5 opacity-60"
                  />
                  Remove all
                </Button>
              </div>
            </div>

            <div className="w-full space-y-2">
              {files.map((file) => (
                <FileItem
                  file={file}
                  key={file.id}
                  onRemove={(fileId) => {
                    handleFileRemoved(fileId);
                    removeFile(fileId);
                  }}
                  uploadProgress={uploadProgress.find(
                    (p) => p.fileId === file.id,
                  )}
                />
              ))}
            </div>
          </div>
        ) : (
          <div className="flex flex-col items-center justify-center px-4 py-3 text-center">
            <div
              aria-hidden="true"
              className="mb-2 flex size-11 shrink-0 items-center justify-center rounded-full border bg-background"
            >
              <ImageIcon className="size-4 opacity-60" />
            </div>
            <p className="mb-1.5 font-medium text-sm">Drop your files here</p>
            <p className="text-muted-foreground text-xs">
              Max {MAX_FILES} files ∙ Up to {MAX_SIZE_MB}MB
            </p>
            <Button className="mt-4" onClick={openFileDialog} variant="outline">
              <UploadIcon aria-hidden="true" className="-ms-1 opacity-60" />
              Select files
            </Button>
          </div>
        )}
      </div>

      {errors.length > 0 && (
        <div
          className="flex items-center gap-1 text-destructive text-xs"
          role="alert"
        >
          <AlertCircleIcon className="size-3 shrink-0" />
          <span>{errors[0]}</span>
        </div>
      )}
    </div>
  );
}

components/ui/use-upload-file.ts
import { useCallback } from 'react';

type UploadArgs = {
  file: File;
  onProgress?: (percent: number) => void;
  onSuccess?: (fileUrl: string) => void;
  onError?: (error: Error) => void;
};

export function useUploadFile() {
  const uploadFile = useCallback(
    async ({ file, onProgress, onSuccess, onError }: UploadArgs) => {
      try {
        const presignRes = await fetch('http://localhost:8080/uploadFile', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            fileName: file.name,
            contentLength: file.size,
          }),
        });

        if (!presignRes.ok) {
          throw new Error('Failed to get presigned URL');
        }
        const presign = await presignRes.json();

        await new Promise<void>((resolve, reject) => {
          const xhr = new XMLHttpRequest();
          xhr.open('PUT', presign.uploadUrl);

          xhr.setRequestHeader('Content-Type', file.type);

          xhr.upload.onprogress = (evt) => {
            if (evt.lengthComputable && onProgress) {
              const PERCENTAGE_MULTIPLIER = 100;
              const percent = Math.round(
                (evt.loaded * PERCENTAGE_MULTIPLIER) / evt.total,
              );
              onProgress(percent);
            }
          };

          xhr.onload = () => {
            if (xhr.status >= 200 && xhr.status < 300) {
              resolve();
            } else {
              reject(new Error('Upload failed'));
            }
          };

          xhr.onerror = () => reject(new Error('Upload failed'));
          xhr.send(file);
        });

        const fileUrl = presign.fileUrl as string;
        onSuccess?.(fileUrl);
        return fileUrl;
      } catch (error) {
        const uploadError =
          error instanceof Error ? error : new Error('Upload failed');
        onError?.(uploadError);
        throw uploadError;
      }
    },
    [],
  );

  return { uploadFile };
}

demo.tsx
import UploadMultipleFiles from "@/components/ui/upload-multiple-files";

const initialFiles = [
  {
    id: "intro.zip-1744638436563-8u5xuls",
    name: "intro.zip",
    size: 252873,
    type: "application/zip",
    url: "https://example.com/intro.zip",
  },
  {
    id: "image-01-123456789",
    name: "image-01.jpg",
    size: 1528737,
    type: "image/jpeg",
    url: "https://cdn.21st.dev/assets/mirror/8c/8cd15bf4cc542bae86dba8441c408e8cc77f82f75df28851fd76265da85cca43.jpg",
  },
  {
    id: "audio-123456789",
    name: "audio.mp3",
    size: 1528737,
    type: "audio/mpeg",
    url: "https://example.com/audio.mp3",
  },
];

export default function UploadMultipleFilesDemo() {
  return (
    <div className="flex min-h-screen w-full items-center justify-center bg-background p-6">
      <div className="w-full max-w-md">
        <UploadMultipleFiles initialFiles={initialFiles} />
      </div>
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button comp-553.json
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
