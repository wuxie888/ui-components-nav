<!-- File Dropzone · @joyco · https://21st.dev/@joyco/components/file-dropzone
     license: no-license · category: upload-download
     A drag-and-drop file upload area with image preview, file-type and size validation, single or multiple file support, and error handling. -->

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
components/file-dropzone.tsx
'use client'

import {
  AlertCircleIcon,
  FileIcon,
  ImageIcon,
  UploadIcon,
  XIcon,
} from 'lucide-react'

import { Button } from '@/components/ui/button'
import { useFileUpload } from '@/registry/hooks/use-file-upload'

export interface FileDropzoneProps {
  accept?: string
  maxSizeMB?: number
  maxFiles?: number
  multiple?: boolean
  onUpload?: (file: File) => Promise<unknown> | void
}

export const FileDropzone = ({
  accept,
  maxSizeMB = 2,
  maxFiles,
  multiple = false,
  onUpload,
}: FileDropzoneProps) => {
  const maxSize = maxSizeMB * 1024 * 1024
  const finalMaxFiles = maxFiles ?? (multiple ? 5 : 1)

  const {
    files,
    isDragging,
    errors,
    handleDragEnter,
    handleDragLeave,
    handleDragOver,
    handleDrop,
    openFileDialog,
    removeFile,
    getInputProps,
  } = useFileUpload({
    accept,
    maxSize,
    maxFiles: finalMaxFiles,
    onUpload,
  })

  const previewUrl = files[0]?.preview || null
  const uploadedFile = files[0]?.file || null
  const hasFiles = files.length > 0

  return (
    <div className="flex flex-col gap-4">
      <div className="relative">
        {/* Drop area */}
        <div
          className="border-input has-[input:focus]:border-ring has-[input:focus]:ring-ring/50 data-[dragging=true]:bg-accent/50 relative flex min-h-52 flex-col items-center justify-center overflow-hidden rounded-xl border border-dashed p-4 transition-colors has-[input:focus]:ring-[3px]"
          data-dragging={isDragging || undefined}
          onDragEnter={handleDragEnter}
          onDragLeave={handleDragLeave}
          onDragOver={handleDragOver}
          onDrop={handleDrop}
        >
          <input
            {...getInputProps()}
            aria-label="Upload file"
            className="sr-only"
          />
          {/* Single file mode: show file inside drop area */}
          {!multiple && uploadedFile ? (
            <div className="absolute inset-0 flex items-center justify-center p-4">
              {previewUrl ? (
                <img
                  alt={uploadedFile.name || 'Uploaded file'}
                  className="mx-auto max-h-full rounded object-contain"
                  src={previewUrl}
                />
              ) : (
                <div className="flex flex-col items-center justify-center gap-2 text-center">
                  <div
                    aria-hidden="true"
                    className="bg-background flex size-16 shrink-0 items-center justify-center rounded-full border"
                  >
                    <FileIcon className="size-8 opacity-60" />
                  </div>
                  <p className="text-sm font-medium">{uploadedFile.name}</p>
                  <p className="text-muted-foreground text-xs">
                    {uploadedFile.size < 1024 * 1024
                      ? `${(uploadedFile.size / 1024).toFixed(0)} KB`
                      : `${(uploadedFile.size / 1024 / 1024).toFixed(2)} MB`}
                  </p>
                </div>
              )}
            </div>
          ) : (
            <div className="flex flex-col items-center justify-center px-4 py-3 text-center">
              <div
                aria-hidden="true"
                className="bg-background mb-2 flex size-11 shrink-0 items-center justify-center rounded-full border"
              >
                <ImageIcon className="size-4 opacity-60" />
              </div>
              <p className="mb-1.5 text-sm font-medium">
                Drop your {multiple ? 'files' : 'file'} here
              </p>
              <p className="text-muted-foreground text-xs">
                {accept ? `${accept.split(',').join(', ')} ` : 'Any file type '}
                (max. {maxSizeMB}MB)
                {multiple &&
                  finalMaxFiles > 1 &&
                  ` · Up to ${finalMaxFiles} files`}
              </p>
              <Button
                className="mt-4"
                onClick={openFileDialog}
                variant="outline"
              >
                <UploadIcon
                  aria-hidden="true"
                  className="-ms-1 size-4 opacity-60"
                />
                Select {multiple ? 'files' : 'file'}
              </Button>
            </div>
          )}
        </div>

        {/* Remove button for single file mode */}
        {!multiple && uploadedFile && (
          <div className="absolute top-4 right-4">
            <button
              aria-label="Remove file"
              className="focus-visible:border-ring focus-visible:ring-ring/50 z-50 flex size-8 cursor-pointer items-center justify-center rounded-full bg-black/60 text-white transition-[color,box-shadow] outline-none hover:bg-black/80 focus-visible:ring-[3px]"
              onClick={() => removeFile(files[0]?.id)}
              type="button"
            >
              <XIcon aria-hidden="true" className="size-4" />
            </button>
          </div>
        )}
      </div>

      {/* Error messages */}
      {errors.length > 0 && (
        <div
          className="text-destructive flex items-center gap-1 text-xs"
          role="alert"
        >
          <AlertCircleIcon className="size-3 shrink-0" />
          <span>{errors[0]}</span>
        </div>
      )}

      {/* Uploaded files list for multiple mode */}
      {multiple && hasFiles && (
        <div className="space-y-2">
          {files.map((fileWithPreview) => {
            const { id, file, preview } = fileWithPreview
            return (
              <div
                key={id}
                className="bg-muted/50 border-border relative flex items-center gap-3 rounded-lg border p-3"
              >
                {preview ? (
                  <img
                    src={preview}
                    alt={file.name}
                    className="size-10 shrink-0 rounded object-cover"
                  />
                ) : (
                  <div className="bg-background flex size-10 shrink-0 items-center justify-center rounded border">
                    <FileIcon className="size-4 opacity-60" />
                  </div>
                )}
                <div className="min-w-0 flex-1">
                  <p className="truncate text-sm font-medium">{file.name}</p>
                  <p className="text-muted-foreground text-xs">
                    {file.size < 1024 * 1024
                      ? `${(file.size / 1024).toFixed(0)} KB`
                      : `${(file.size / 1024 / 1024).toFixed(2)} MB`}
                  </p>
                </div>
                <button
                  onClick={() => removeFile(id)}
                  aria-label={`Remove ${file.name}`}
                  className="text-muted-foreground hover:text-destructive shrink-0 transition-colors"
                  type="button"
                >
                  <XIcon className="size-4" />
                </button>
              </div>
            )
          })}
        </div>
      )}
    </div>
  )
}

hooks/use-file-upload.tsx
import { useCallback, useId, useRef, useState } from 'react'

export interface FileWithPreview {
  id: string
  file: File
  preview?: string
}

export interface UseFileUploadOptions {
  accept?: string
  maxSize?: number
  maxFiles?: number
  onUpload?: (file: File) => Promise<unknown> | void
}

export interface UseFileUploadReturn {
  files: FileWithPreview[]
  errors: string[]
  isDragging: boolean
  removeFile: (id: string) => void
  openFileDialog: () => void
  getInputProps: () => {
    ref: React.RefObject<HTMLInputElement | null>
    type: 'file'
    id: string
    accept?: string
    multiple: boolean
    onChange: (event: React.ChangeEvent<HTMLInputElement>) => Promise<void>
  }
  handleDragEnter: (e: React.DragEvent) => void
  handleDragLeave: (e: React.DragEvent) => void
  handleDragOver: (e: React.DragEvent) => void
  handleDrop: (e: React.DragEvent) => void
}

export function useFileUpload(
  options: UseFileUploadOptions = {}
): UseFileUploadReturn {
  const { accept, maxSize, maxFiles = 1, onUpload } = options

  const id = useId()
  const inputRef = useRef<HTMLInputElement>(null)
  const dragCounterRef = useRef(0)

  const [files, setFiles] = useState<FileWithPreview[]>([])
  const [errors, setErrors] = useState<string[]>([])
  const [isDragging, setIsDragging] = useState(false)

  const validateFile = useCallback(
    (file: File): string | null => {
      if (accept) {
        const acceptedTypes = accept.split(',').map((t) => t.trim())
        const fileType = file.type
        const isAccepted = acceptedTypes.some((type) => {
          if (type.endsWith('/*')) {
            return fileType.startsWith(type.replace('/*', ''))
          }
          return fileType === type
        })

        if (!isAccepted) {
          return `File type not accepted. Allowed: ${accept}`
        }
      }

      if (maxSize && file.size > maxSize) {
        const maxSizeMB = (maxSize / (1024 * 1024)).toFixed(1)
        return `File too large. Max size: ${maxSizeMB}MB`
      }

      return null
    },
    [accept, maxSize]
  )

  const createPreview = useCallback(
    (file: File): Promise<string | undefined> => {
      return new Promise((resolve) => {
        if (file.type.startsWith('image/')) {
          const reader = new FileReader()
          reader.onloadend = () => resolve(reader.result as string)
          reader.onerror = () => resolve(undefined)
          reader.readAsDataURL(file)
        } else {
          resolve(undefined)
        }
      })
    },
    []
  )

  const processFiles = useCallback(
    async (fileList: FileList | File[]) => {
      const filesArray = Array.from(fileList)
      const newErrors: string[] = []
      const validFiles: FileWithPreview[] = []

      for (const file of filesArray) {
        const error = validateFile(file)
        if (error) {
          newErrors.push(error)
          continue
        }

        const preview = await createPreview(file)

        validFiles.push({
          id: `${Date.now()}-${Math.random()}`,
          file,
          preview,
        })

        if (onUpload) {
          await onUpload(file)
        }
      }

      setErrors(newErrors)
      setFiles((prev) => {
        const combined = [...prev, ...validFiles]
        return combined.slice(0, maxFiles)
      })
    },
    [validateFile, createPreview, onUpload, maxFiles]
  )

  const removeFile = useCallback((fileId: string) => {
    setFiles((prev) => prev.filter((f) => f.id !== fileId))
  }, [])

  const openFileDialog = useCallback(() => {
    inputRef.current?.click()
  }, [])

  const handleFileChange = useCallback(
    async (event: React.ChangeEvent<HTMLInputElement>) => {
      const fileList = event.target.files
      if (fileList && fileList.length > 0) {
        await processFiles(fileList)
      }
      event.target.value = ''
    },
    [processFiles]
  )

  const getInputProps = useCallback(() => {
    return {
      ref: inputRef,
      type: 'file' as const,
      id: `${id}-file-input`,
      accept,
      multiple: maxFiles > 1,
      onChange: handleFileChange,
    }
  }, [id, accept, maxFiles, handleFileChange])

  const handleDragEnter = useCallback((e: React.DragEvent) => {
    e.preventDefault()
    e.stopPropagation()
    dragCounterRef.current++
    if (e.dataTransfer.items && e.dataTransfer.items.length > 0) {
      setIsDragging(true)
    }
  }, [])

  const handleDragLeave = useCallback((e: React.DragEvent) => {
    e.preventDefault()
    e.stopPropagation()
    dragCounterRef.current--
    if (dragCounterRef.current === 0) {
      setIsDragging(false)
    }
  }, [])

  const handleDragOver = useCallback((e: React.DragEvent) => {
    e.preventDefault()
    e.stopPropagation()
  }, [])

  const handleDrop = useCallback(
    async (e: React.DragEvent) => {
      e.preventDefault()
      e.stopPropagation()
      setIsDragging(false)
      dragCounterRef.current = 0

      const fileList = e.dataTransfer.files
      if (fileList && fileList.length > 0) {
        await processFiles(fileList)
      }
    },
    [processFiles]
  )

  return {
    files,
    errors,
    isDragging,
    removeFile,
    openFileDialog,
    getInputProps,
    handleDragEnter,
    handleDragLeave,
    handleDragOver,
    handleDrop,
  }
}

demo.tsx
'use client'

import { FileDropzone } from '@/components/ui/file-dropzone'

function FileForm() {
  const handleUpload = (file: File) => {
    console.log('File uploaded:', file.name, file.type)
  }

  return (
    <div className="flex flex-col gap-4 p-10">
      <FileDropzone
        accept="application/pdf,image/*"
        maxSizeMB={10}
        maxFiles={20}
        multiple={true}
        onUpload={handleUpload}
      />
    </div>
  )
}

export default FileForm
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
