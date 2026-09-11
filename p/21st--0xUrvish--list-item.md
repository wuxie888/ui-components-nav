<!-- List Item · @0xUrvish · https://21st.dev/@0xUrvish/components/list-item
     license: MIT · category: list
     An animated list item with smooth entrance and exit effects. -->

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
components/ui/list-item.tsx
"use client";
import { motion, MotionConfig } from "motion/react";
import { Dispatch, SetStateAction, useState } from "react";
import clsx from "clsx";

import {
  Appointment01Icon,
  BalloonsIcon,
  GoogleMapsIcon,
  ZoomIcon,
  ReminderIcon,
  TaskDaily01Icon,
  Tick02Icon,
  FilterHorizontalIcon,
} from "@hugeicons/core-free-icons";
import { HugeiconsFreeIcons } from "@hugeicons/core-free-icons";
import { HugeiconsIcon } from "@hugeicons/react";

export type FilterKey = (typeof filterKeys)[number];

// Change Here
export const filterKeys = [
  {
    name: "tasks",
    Icon: ({ size }: { size: number }) => (
      <HugeiconsIcon icon={TaskDaily01Icon} size={size} />
    ),
  },
  {
    name: "events",
    Icon: ({ size }: { size: number }) => (
      <HugeiconsIcon icon={GoogleMapsIcon} size={size} />
    ),
  },
  {
    name: "reminders",
    Icon: ({ size }: { size: number }) => (
      <HugeiconsIcon icon={ReminderIcon} size={size} />
    ),
  },
  {
    name: "appointments",
    Icon: ({ size }: { size: number }) => (
      <HugeiconsIcon icon={Appointment01Icon} size={size} />
    ),
  },
  {
    name: "meetings",
    Icon: ({ size }: { size: number }) => (
      <HugeiconsIcon icon={ZoomIcon} size={size} />
    ),
  },
  {
    name: "celebrations",
    Icon: ({ size }: { size: number }) => (
      <HugeiconsIcon icon={BalloonsIcon} size={size} />
    ),
  },
];

function ListItem(props: {
  index: number;
  filterKey: FilterKey;
  selectedFilterKey: FilterKey;
  setSelectedFilterKey: Dispatch<SetStateAction<FilterKey>>;
  setIsOpened: Dispatch<SetStateAction<boolean>>;
}) {
  const {
    index,
    filterKey,
    selectedFilterKey,
    setSelectedFilterKey,
    setIsOpened,
  } = props;
  const delay = (index + 8) * 0.025;

  return (
    <motion.div
      initial={{ opacity: 0, y: 40 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{
        type: "spring",
        bounce: 0.1,
        duration: 0.25,
        delay,
        ease: [0.215, 0.61, 0.355, 1],
      }}
      onClick={() => {
        setSelectedFilterKey(filterKey);

        setTimeout(() => {
          setIsOpened(false);
        }, 150);
      }}
      className="px-3 py-2 rounded-2xl flex justify-between items-center cursor-default hover:bg-accent  text-foreground"
    >
      <div className="flex items-center gap-x-3">
        <span className="text-muted-foreground">
          <filterKey.Icon size={24} />
        </span>
        <span className="capitalize">{filterKey.name}</span>
      </div>
      <div
        className={clsx(
          "relative border-border w-6 h-6 overflow-hidden rounded-full",
          selectedFilterKey.name == filterKey.name
            ? "border-none"
            : "border-[2px]"
        )}
      >
        {selectedFilterKey.name == filterKey.name && (
          <div className="absolute inset-0 bg-primary flex justify-center items-center text-primary-foreground">
            <HugeiconsIcon icon={Tick02Icon} size={16} />
          </div>
        )}
      </div>
    </motion.div>
  );
}

const FilterInteraction = () => {
  const [selectedFilterKey, setSelectedFilterKey] = useState(filterKeys[0]);
  const [isOpened, setIsOpened] = useState(false);

  return (
    <section className="flex justify-center items-center fill-muted-foreground/70">
      <MotionConfig
        transition={{ type: "spring", duration: 0.85, bounce: 0.35 }}
      >
        <div
          onClick={() => setIsOpened(true)}
          className="relative left-2.5 w-20 h-20 flex justify-center items-center"
        >
          <HugeiconsIcon
            icon={FilterHorizontalIcon}
            className="text-foreground relative z-10 fill-none"
            size={36}
          />
          <motion.div
            layoutId="wrapper"
            className="absolute inset-0 z-[2] bg-background border-border"
            style={{ borderRadius: 40, borderWidth: 1 }}
          />
        </div>
        <motion.div
          initial={{ x: 0 }}
          animate={{
            x: isOpened ? -20 : 0,
            // transition: { delay: isOpened ? 0 : 0.2 },
          }}
          transition={{ type: "spring", bounce: 0.3, duration: 1.5 }}
          className="relative right-2.5 w-20 h-20 border border-border rounded-full flex justify-center items-center bg-background"
        >
          <span className="text-muted-foreground">
            <selectedFilterKey.Icon size={36} />
          </span>
        </motion.div>

        {isOpened && (
          <motion.section
            layoutId="wrapper"
            className="absolute z-20 w-72 px-1 py-1 bg-card border border-border text-xl overflow-hidden "
            style={{ borderRadius: 20, borderWidth: 1 }}
          >
            <div className="flex flex-col gap-1">
              {filterKeys.map((item, index) => (
                <ListItem
                  key={item.name}
                  index={index}
                  filterKey={item}
                  selectedFilterKey={selectedFilterKey}
                  setSelectedFilterKey={setSelectedFilterKey}
                  setIsOpened={setIsOpened}
                />
              ))}
            </div>
          </motion.section>
        )}
      </MotionConfig>
    </section>
  );
};

export default FilterInteraction;

demo.tsx
"use client";
import ListItem from "@/components/ui/list-item";

export default function Demo() {
  return (
    <div className="flex items-center justify-center w-full min-h-screen bg-background p-8">
      <ListItem />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @hugeicons/core-free-icons @hugeicons/react clsx motion tailwind-merge
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
