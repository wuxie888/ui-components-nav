<!-- Multi Accordion · @uilayout.contact · https://21st.dev/@uilayout.contact/components/multi-accordion
     license: unspecified · category: faq
     A flexible accordion component that allows multiple sections to stay open at the same time. Unlike a traditional accordion, where opening one section automatically closes the others, this component gives you the option to expand and view multiple sections simultaneously. -->

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
components/ui/accordion.tsx
'use client';

import { cn } from '@/lib/utils';
import { ChevronDown } from 'lucide-react';
import { AnimatePresence, motion } from 'motion/react';
import React, { type ReactNode, useCallback } from 'react';

/**
 * Interface for AccordionContext values
 */
interface AccordionContextType {
  /**
   * Whether the accordion item is active
   */
  isActive?: boolean;
  /**
   * The value of the accordion item
   */
  value?: string;
  /**
   * Function to change the active index
   */
  onChangeIndex?: (value: string) => void;
}

/**
 * Context for accordion components
 */
const AccordionContext = React.createContext<AccordionContextType>({});
/**
 * Hook to use the accordion context
 */
const useAccordion = () => React.useContext(AccordionContext);

/**
 * Container component for accordion items
 */
export function AccordionContainer({
  children,
  className,
}: {
  children: ReactNode;
  className?: string;
}) {
  return <div className={cn('grid grid-cols-2 gap-1', className)}>{children}</div>;
}

/**
 * Wrapper component for accordion items
 */
export function AccordionWrapper({ children }: { children: ReactNode }) {
  return <div>{children}</div>;
}

/**
 * Interface for Accordion props
 */
interface AccordionProps {
  /**
   * Children components
   */
  children: ReactNode;
  /**
   * Whether multiple items can be active at the same time
   */
  multiple?: boolean;
  /**
   * Default active index
   */
  defaultValue?: string | string[];
}

/**
 * Accordion component
 */
export function Accordion({ children, multiple, defaultValue }: AccordionProps) {
  /**
   * State for active index
   */
  const [activeIndex, setActiveIndex] = React.useState<string | string[] | null>(
    multiple ? (Array.isArray(defaultValue) ? defaultValue : []) : defaultValue || null
  );

  /**
   * Function to change the active index
   */
  const onChangeIndex = useCallback(
    (value: string) => {
      setActiveIndex((currentActiveIndex) => {
        if (!multiple) {
          return value === currentActiveIndex ? null : value;
        }

        if (Array.isArray(currentActiveIndex)) {
          if (currentActiveIndex.includes(value)) {
            return currentActiveIndex.filter((i) => i !== value);
          }
          return [...currentActiveIndex, value];
        }

        return [value];
      });
    },
    [multiple]
  );

  return React.Children.map(children, (child) => {
    if (!React.isValidElement(child)) return null;

    const childProps = child.props as { value: string };
    const value = childProps.value;
    const isActive = multiple
      ? Array.isArray(activeIndex) && activeIndex.includes(value)
      : activeIndex === value;

    return (
      <AccordionContext.Provider value={{ isActive, value, onChangeIndex }}>
        {child}
      </AccordionContext.Provider>
    );
  });
}

/**
 * Interface for AccordionItem props
 */
interface AccordionItemProps {
  /**
   * Children components
   */
  children: ReactNode;
  /**
   * Value of the accordion item
   */
  value: string;
  className?: string;
}

/**
 * Accordion item component
 */
export function AccordionItem({ children, value, className }: AccordionItemProps) {
  const { isActive } = useAccordion();

  return (
    <div
      data-active={isActive || undefined}
      className={cn(
        'rounded-lg overflow-hidden mb-2 group border border-neutral-200 dark:border-neutral-800',
        className
      )}
    >
      {children}
    </div>
  );
}

/**
 * Interface for AccordionHeader props
 */
interface AccordionHeaderProps {
  /**
   * Children components
   */
  children: ReactNode;
  /**
   * Icon component
   */
  customIcon?: boolean;
  className?: string;
}

/**
 * Accordion header component
 */
export function AccordionHeader({ children, customIcon, className }: AccordionHeaderProps) {
  const { isActive, value, onChangeIndex } = useAccordion();

  const handleClick = useCallback(() => {
    if (value && onChangeIndex) {
      onChangeIndex(value);
    }
  }, [onChangeIndex, value]);

  return (
    <motion.button
      type='button'
      data-active={isActive || undefined}
      aria-expanded={isActive}
      className={cn(
        'p-4 cursor-pointer w-full transition-all font-semibold text-neutral-500 dark:data-active:text-neutral-200 data-active:text-neutral-800 dark:data-active:bg-neutral-800 data-active:bg-neutral-200 hover:bg-neutral-100 hover:text-black flex justify-between gap-2 items-center text-left',
        className
      )}
      onClick={handleClick}
    >
      {children}
      {!customIcon && (
        <ChevronDown
          className={cn(
            'transition-transform shrink-0 text-neutral-500 dark:text-neutral-400',
            isActive ? 'rotate-180' : 'rotate-0'
          )}
          aria-hidden='true'
        />
      )}
    </motion.button>
  );
}
/**
 * Interface for AccordionPanel props
 */
interface AccordionPanelProps {
  /**
   * Children components
   */
  children: ReactNode;
  /**
   * className
   */
  className?: string;
  /**
   * article className
   */
  articleClassName?: string;
}

/**
 * Accordion panel component
 */
export function AccordionPanel({ children, className, articleClassName }: AccordionPanelProps) {
  const { isActive, value } = useAccordion();

  return (
    <AnimatePresence initial={true}>
      {isActive && (
        <motion.div
          data-active={isActive || undefined}
          role='region'
          id={`accordion-panel-${value}`}
          aria-labelledby={`accordion-header-${value}`}
          initial={{ height: 0, overflow: 'hidden' }}
          animate={{ height: 'auto', overflow: 'hidden' }}
          exit={{ height: 0 }}
          transition={{ type: 'spring', duration: 0.3, bounce: 0 }}
          className={cn(
            'bg-neutral-100 dark:bg-neutral-900 px-2 data-active:bg-neutral-200 dark:data-active:bg-neutral-800 text-black dark:text-white',
            className
          )}
        >
          <motion.div
            initial={{ clipPath: 'polygon(0 0, 100% 0, 100% 0, 0 0)' }}
            animate={{ clipPath: 'polygon(0 0, 100% 0, 100% 100%, 0% 100%)' }}
            exit={{
              clipPath: 'polygon(0 0, 100% 0, 100% 0, 0 0)',
            }}
            transition={{
              type: 'spring',
              duration: 0.4,
              bounce: 0,
            }}
            className={cn('px-3 bg-transparent pb-4 space-y-2', articleClassName)}
          >
            {children}
          </motion.div>
        </motion.div>
      )}
    </AnimatePresence>
  );
}

demo.tsx
import React from 'react';
import {
  Accordion,
  AccordionHeader,
  AccordionItem,
  AccordionPanel,
  AccordionWrapper,
} from '@/components/ui/multi-accordion';

function AccordionDemo() {
  return (
    <div className="px-10 sm:w-[34rem] w-full mx-auto">
      <Accordion defaultValue={'item-2'} multiple>
        <AccordionItem value='item-1'>
          <AccordionHeader>What is a UI component?</AccordionHeader>
          <AccordionPanel>
            A UI (User Interface) component is a modular, reusable element that
            serves a specific function within a graphical user interface.
            Examples include buttons, input fields, dropdown menus, sliders, and
            checkboxes.
          </AccordionPanel>
        </AccordionItem>
        <AccordionItem value='item-2'>
          <AccordionHeader>Why are UI components important?</AccordionHeader>
          <AccordionPanel>
            UI components promote consistency, efficiency, and scalability in
            software development. They allow developers to reuse code, maintain
            a consistent look and feel across an application, and easily make
            updates or modifications without affecting the entire system.
          </AccordionPanel>
        </AccordionItem>
        <AccordionItem value='item-3'>
          <AccordionHeader>
            Key characteristics of UI components?
          </AccordionHeader>
          <AccordionPanel>
            Well-designed UI components should be modular, customizable, and
            accessible. They should have clear and intuitive functionality, be
            easily styled to match the overall design language of the
            application.
          </AccordionPanel>
        </AccordionItem>
      </Accordion>
    </div>
  );
}

export default AccordionDemo;
```

Install NPM dependencies:
```bash
npm install lucide-react motion
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
