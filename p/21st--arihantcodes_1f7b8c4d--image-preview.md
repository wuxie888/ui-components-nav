<!-- Image Preview · @arihantcodes_1f7b8c4d · https://21st.dev/@arihantcodes_1f7b8c4d/components/image-preview
     license: MIT · category: gallery
     A clickable image thumbnail that opens a full-size lightbox preview in a modal overlay with a close button. -->

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
components/ui/useone.tsx
import React from "react";
import ImagePreview from "@/app/registry/imagepreview/image-preview-dependecies";

const Demoimages = () => {
  return (
    <>
      <ImagePreview
        src="https://images.pexels.com/photos/1242764/pexels-photo-1242764.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1"
        width={400}
        height={400}
      />
    </>
  );
};

export default Demoimages;

demo.tsx
import React from "react";
import ImagePreview from "@/components/ui/image-preview";

export default function Default() {
  return (
    <div className="flex min-h-[400px] w-full items-center justify-center bg-background p-8">
      <ImagePreview
        src="https://cdn.21st.dev/assets/mirror/42/42258075158c2affc9fc985497dadef4d41d5d04d89199f0f62f5eec571c1667.jpg"
        alt="Mountain landscape"
        width={400}
        height={400}
      />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-dialog lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add image-preview-dependencies
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
