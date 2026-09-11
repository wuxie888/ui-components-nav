<!-- File Upload Field · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-field-21
     license: MIT · category: upload-download
     A form field for uploading a file such as a resume or document, with an upload label, helper description, and validation error message. -->

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
components/ui/v-field-21.tsx
import { UploadIcon } from "lucide-react";
import {
  Field,
  FieldDescription,
  FieldError,
  FieldLabel,
} from "@/registry/default/ui/field";
import { Input } from "@/registry/default/ui/input";

export default function Particle() {
  return (
    <Field>
      <FieldLabel>
        <span className="flex items-center gap-1.5">
          <UploadIcon aria-hidden="true" className="size-4 opacity-60" />
          Upload resume
        </span>
      </FieldLabel>
      <Input accept=".pdf,.doc,.docx" type="file" />
      <FieldDescription>PDF or Word document, max 5 MB.</FieldDescription>
      <FieldError>Please upload a valid file.</FieldError>
    </Field>
  );
}

demo.tsx
import Particle from "@/components/ui/v-field-21";

export default function Default() {
  return (
    <div className="flex min-h-64 w-full items-center justify-center p-6">
      <div className="w-full max-w-sm">
        <Particle />
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
npx shadcn@latest add field input
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
