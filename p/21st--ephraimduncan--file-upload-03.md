<!-- File Upload Multi-File Dropzone · @ephraimduncan · https://21st.dev/@ephraimduncan/components/file-upload-03
     license: no-license · category: upload-download
     A multi-file upload card with a drag-and-drop dropzone, bucket name and visibility fields, and a removable list of selected files. -->

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
components/ui/file-upload-03.tsx
'use client';

import { File, Trash } from 'lucide-react';
import React from 'react';
import { useDropzone } from 'react-dropzone';

import { Button } from '@/components/ui/button';
import {
  Card,
  CardContent,
  CardDescription,
  CardHeader,
  CardTitle,
} from '@/components/ui/card';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from '@/components/ui/select';
import { Separator } from '@/components/ui/separator';
import { cn } from '@/lib/utils';

export default function FileUpload03() {
  const [files, setFiles] = React.useState<File[]>([]);
  const { getRootProps, getInputProps, isDragActive } = useDropzone({
    onDrop: (acceptedFiles) => setFiles(acceptedFiles),
  });

  const filesList = files.map((file) => (
    <li className="relative" key={file.name}>
      <Card className="relative p-4 shadow-none">
        <div className="-translate-y-1/2 absolute top-1/2 right-4">
          <Button
            aria-label="Remove file"
            onClick={() =>
              setFiles((prevFiles) =>
                prevFiles.filter((prevFile) => prevFile.name !== file.name)
              )
            }
            size="icon"
            type="button"
            variant="ghost"
          >
            <Trash aria-hidden={true} className="h-5 w-5" />
          </Button>
        </div>
        <CardContent className="flex items-center space-x-3 p-0">
          <span className="flex h-10 w-10 shrink-0 items-center justify-center rounded-md bg-muted">
            <File aria-hidden={true} className="h-5 w-5 text-foreground" />
          </span>
          <div>
            <p className="text-pretty font-medium text-foreground">
              {file.name}
            </p>
            <p className="mt-0.5 text-pretty text-muted-foreground text-sm">
              {file.size} bytes
            </p>
          </div>
        </CardContent>
      </Card>
    </li>
  ));

  return (
    <div className="flex items-center justify-center p-10">
      <Card className="shadow-none sm:mx-auto sm:max-w-xl">
        <CardHeader>
          <CardTitle>Set up your first cloud storage</CardTitle>
          <CardDescription>
            Lorem ipsum dolor sit amet, consetetur sadipscing elitr.
          </CardDescription>
        </CardHeader>
        <CardContent>
          <form action="#" method="post">
            <div className="grid grid-cols-1 gap-4 sm:grid-cols-6">
              <div className="col-span-full sm:col-span-3">
                <Label className="font-medium" htmlFor="bucket-name">
                  Bucket name
                </Label>
                <Input
                  className="mt-2"
                  id="bucket-name"
                  name="bucket-name"
                  placeholder="Bucket name"
                  type="text"
                />
              </div>
              <div className="col-span-full sm:col-span-3">
                <Label className="font-medium" htmlFor="visibility">
                  Visibility
                </Label>
                <Select
                  defaultValue="private"
                  disabled
                  items={{ private: 'Private', public: 'Public' }}
                >
                  <SelectTrigger
                    className="mt-2 w-full"
                    id="visibility"
                    name="visibility"
                  >
                    <SelectValue placeholder="Select visibility" />
                  </SelectTrigger>
                  <SelectContent>
                    <SelectItem value="private">Private</SelectItem>
                    <SelectItem value="public">Public</SelectItem>
                  </SelectContent>
                </Select>
                <p className="mt-2 text-pretty text-muted-foreground text-sm">
                  Only admins can change visibility.
                </p>
              </div>
              <div className="col-span-full">
                <Label className="font-medium" htmlFor="file-upload-2">
                  File(s) upload
                </Label>
                <div
                  {...getRootProps()}
                  className={cn(
                    isDragActive
                      ? 'border-primary bg-primary/10 ring-2 ring-primary/20'
                      : 'border-border',
                    'mt-2 flex justify-center rounded-md border border-dashed px-6 py-20 transition-colors duration-200'
                  )}
                >
                  <div>
                    <File
                      aria-hidden={true}
                      className="mx-auto h-12 w-12 text-muted-foreground/80"
                    />
                    <div className="mt-4 flex text-muted-foreground">
                      <p>Drag and drop or</p>
                      <label
                        className="relative cursor-pointer rounded-sm pl-1 font-medium text-primary hover:text-primary/80 hover:underline hover:underline-offset-4"
                        htmlFor="file"
                      >
                        <span>choose file(s)</span>
                        <input
                          {...getInputProps()}
                          className="sr-only"
                          id="file-upload-2"
                          name="file-upload-2"
                          type="file"
                        />
                      </label>
                      <p className="text-pretty pl-1">to upload</p>
                    </div>
                  </div>
                </div>
                <p className="mt-2 text-pretty text-muted-foreground text-sm leading-5 sm:flex sm:items-center sm:justify-between">
                  <span>All file types are allowed to upload.</span>
                  <span className="pl-1 sm:pl-0">Max. size per file: 50MB</span>
                </p>
                {filesList.length > 0 && (
                  <>
                    <h4 className="mt-6 text-balance font-medium text-foreground">
                      File(s) to upload
                    </h4>
                    <ul className="mt-4 space-y-4" role="list">
                      {filesList}
                    </ul>
                  </>
                )}
              </div>
            </div>
            <Separator className="my-6" />
            <div className="flex items-center justify-end space-x-3">
              <Button type="button" variant="outline">
                Cancel
              </Button>
              <Button type="submit">Upload</Button>
            </div>
          </form>
        </CardContent>
      </Card>
    </div>
  );
}

demo.tsx
import FileUpload03 from "@/components/ui/file-upload-03";

export default function Default() {
  return (
    <div className="w-full bg-background text-foreground">
      <FileUpload03 />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react react-dropzone
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button card input label select separator
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
