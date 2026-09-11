<!-- Status Badge · @arihantcodes_1f7b8c4d · https://21st.dev/@arihantcodes_1f7b8c4d/components/status-badge
     license: unspecified · category: badge
     Here is Status Badge component -->

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
components/ui/statusdemo.tsx
import {
  CircleCheck,
  CircleDashed,
  CircleX,
  Clock5,
  ScanSearch,
  Send,
  TriangleAlert,
} from "lucide-react";
import React from "react";

const statuses = [
  {
    label: "Pending",
    icon: TriangleAlert,
    className:
      "bg-amber-50 text-amber-700 ring-amber-600/20 dark:bg-amber-400/10 dark:text-amber-300 dark:ring-amber-300/25",
  },
  {
    label: "Failed",
    icon: CircleX,
    className:
      "bg-rose-50 text-rose-700 ring-rose-600/20 dark:bg-rose-400/10 dark:text-rose-300 dark:ring-rose-300/25",
  },
  {
    label: "Success",
    icon: CircleCheck,
    className:
      "bg-emerald-50 text-emerald-700 ring-emerald-600/20 dark:bg-emerald-400/10 dark:text-emerald-300 dark:ring-emerald-300/25",
  },
  {
    label: "In progress",
    icon: CircleDashed,
    className:
      "bg-sky-50 text-sky-700 ring-sky-600/20 dark:bg-sky-400/10 dark:text-sky-300 dark:ring-sky-300/25",
    spin: true,
  },
  {
    label: "In review",
    icon: ScanSearch,
    className:
      "bg-violet-50 text-violet-700 ring-violet-600/20 dark:bg-violet-400/10 dark:text-violet-300 dark:ring-violet-300/25",
  },
  {
    label: "Submitted",
    icon: Send,
    className:
      "bg-indigo-50 text-indigo-700 ring-indigo-600/20 dark:bg-indigo-400/10 dark:text-indigo-300 dark:ring-indigo-300/25",
  },
  {
    label: "Expired",
    icon: Clock5,
    className:
      "bg-neutral-100 text-neutral-600 ring-neutral-500/20 dark:bg-neutral-400/10 dark:text-neutral-300 dark:ring-neutral-300/20",
  },
];

const StatusDemo = () => {
  return (
    <div className="flex max-w-xl flex-wrap items-center justify-center gap-3">
      {statuses.map(({ label, icon: Icon, className, spin }) => (
        <span
          key={label}
          className={`inline-flex select-none items-center gap-1.5 rounded-full px-3 py-1.5 text-[13px] font-medium leading-none ring-1 ring-inset ${className}`}
        >
          <Icon
            className={`size-3.5 shrink-0 ${spin ? "animate-spin [animation-duration:3s] motion-reduce:animate-none" : ""}`}
            strokeWidth={2.25}
            aria-hidden="true"
          />
          {label}
        </span>
      ))}
    </div>
  );
};

export default StatusDemo;

demo.tsx
import {
  CircleCheck,
  CircleDashed,
  CircleX,
  Clock5,
  ScanSearch,
  TriangleAlert,
} from "lucide-react";
import React from "react";

const StatusDemo = () => {
  return (
    <div className="flex flex-col items-center justify-center gap-6">
      <div className="grid grid-cols-1 md:grid-cols-2 gap-4 ">
        <div className="w-40 h-[35px] flex items-center justify-center bg-orange-50 rounded-xl ">
          <h1 className="flex items-center  text-[#EAA65D] font-semibold">
            <TriangleAlert className="w-4 h-4 mr-2" strokeWidth={3} />
            Pending
          </h1>
        </div>
        <div className="w-40 h-[35px] flex items-center justify-center bg-rose-50 rounded-xl ">
          <h1 className="flex items-center  text-[#D57463] font-semibold">
            <CircleX className="w-4 h-4 mr-2" strokeWidth={3} />
            Failed
          </h1>
        </div>
      </div>
      <div className="grid grid-cols-1 md:grid-cols-3 gap-4 ">
        <div className="w-40 h-[35px] flex items-center justify-center bg-emerald-50 rounded-xl ">
          <h1 className="flex items-center  text-[#57BC6C] font-semibold">
            <CircleCheck className="w-4 h-4 mr-2" strokeWidth={3} />
            Success
          </h1>
        </div>
        <div className="w-40 h-[35px] flex items-center justify-center bg-sky-100 rounded-xl ">
          <h1 className="flex items-center  text-[#008AF5] font-semibold">
            <CircleDashed className="w-4 h-4 mr-2" strokeWidth={3} />
            In progress
          </h1>
        </div>{" "}
        <div className="w-40 h-[35px] flex items-center justify-center bg-yellow-50 rounded-xl ">
          <h1 className="flex items-center  text-[#F0B13D] font-semibold">
            <ScanSearch className="w-4 h-4 mr-2" strokeWidth={3} />
            In review
          </h1>
        </div>{" "}
      </div>
      <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
        <div className="w-40 h-[35px] flex items-center justify-center bg-zinc-100 rounded-xl ">
          <h1 className="flex items-center  text-[#777777] font-semibold">
            <Clock5 className="w-4 h-4 mr-2" strokeWidth={3} />
            Expired
          </h1>
        </div>
        <div className="w-40 h-[35px] flex items-center justify-center bg-violet-50 rounded-xl ">
          <h1 className="flex items-center  text-[#6C3CF0] font-semibold">
            <Clock5 className="w-4 h-4 mr-2" strokeWidth={3} />
            Submited
          </h1>
        </div>
      </div>
    </div>
  );
};

export default StatusDemo;
```

Install NPM dependencies:
```bash
npm install lucide-react
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
