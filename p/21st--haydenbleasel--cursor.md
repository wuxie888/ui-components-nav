<!-- Cursor · @haydenbleasel · https://21st.dev/@haydenbleasel/components/cursor
     license: MIT · category: cursor
      -->

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
import { Children, type HTMLAttributes, type SVGProps } from "react";
import { cn } from "@/lib/utils";

export type CursorProps = HTMLAttributes<HTMLSpanElement>;

export const Cursor = ({ className, children, ...props }: CursorProps) => (
  <span
    className={cn("pointer-events-none relative select-none", className)}
    {...props}
  >
    {children}
  </span>
);

export type CursorPointerProps = SVGProps<SVGSVGElement>;

export const CursorPointer = ({ className, ...props }: CursorPointerProps) => (
  <svg
    aria-hidden="true"
    className={cn("size-3.5", className)}
    fill="none"
    focusable="false"
    height="20"
    viewBox="0 0 20 20"
    width="20"
    xmlns="http://www.w3.org/2000/svg"
    {...props}
  >
    <path
      d="M19.438 6.716 1.115.05A.832.832 0 0 0 .05 1.116L6.712 19.45a.834.834 0 0 0 1.557.025l3.198-8 7.995-3.2a.833.833 0 0 0 0-1.559h-.024Z"
      fill="currentColor"
    />
  </svg>
);

export type CursorBodyProps = HTMLAttributes<HTMLSpanElement>;

export const CursorBody = ({
  children,
  className,
  ...props
}: CursorBodyProps) => (
  <span
    className={cn(
      "relative ml-3.5 flex flex-col whitespace-nowrap rounded-xl py-1 pr-3 pl-2.5 text-xs",
      Children.count(children) > 1 && "rounded-tl [&>:first-child]:opacity-70",
      "bg-secondary text-foreground",
      className
    )}
    {...props}
  >
    {children}
  </span>
);

export type CursorNameProps = HTMLAttributes<HTMLSpanElement>;

export const CursorName = (props: CursorNameProps) => <span {...props} />;

export type CursorMessageProps = HTMLAttributes<HTMLSpanElement>;

export const CursorMessage = (props: CursorMessageProps) => <span {...props} />;

demo.tsx
import {
  Cursor,
  CursorName,
  CursorMessage,
  CursorPointer,
  CursorBody,
} from '@/components/ui/cursor';

const Demo = () => (
  <>
    <Cursor className="absolute top-24 left-24">
      <CursorPointer className="text-emerald-500" />
      <CursorBody className="bg-emerald-100 text-emerald-700">
        <CursorName>@haydenbleasel</CursorName>
        <CursorMessage>Can we adjust the color?</CursorMessage>
      </CursorBody>
    </Cursor>
    <Cursor className="absolute top-48 right-32">
      <CursorPointer className="text-rose-500" />
      <CursorBody className="bg-rose-100 text-rose-700">
        <CursorName>@leerob</CursorName>
        <CursorMessage>One more thing...</CursorMessage>
      </CursorBody>
    </Cursor>
    <Cursor className="absolute bottom-24 left-48">
      <CursorPointer className="text-sky-500" />
      <CursorBody className="bg-sky-100 text-sky-700">
        <CursorName>@shadcn</CursorName>
        <CursorMessage>Another new component?!!</CursorMessage>
      </CursorBody>
    </Cursor>
  </>
);
  
export default { Demo }
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
