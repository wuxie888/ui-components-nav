<!-- Condition Grid · @uilayout.contact · https://21st.dev/@uilayout.contact/components/conditiongrid
     license: MIT · category: grid
     A responsive projects showcase grid with alternating column spans, animated cards, and overlaid title and link badges. -->

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
components/ui/condition-grid.tsx
'use client';
import { MoveUpRight } from 'lucide-react';
import { motion } from 'motion/react';
import Image from 'next/image';
import React from 'react';

interface ProjectsTypes {
  id: string;
  img: string;
  title: string;
  des: string;
}
const projects: ProjectsTypes[] = [
  {
    id: '01',
    img: 'https://images.unsplash.com/photo-1543508282-6319a3e2621f?q=80&w=1200&auto=format&fit=crop',
    title: 'Distrokings',
    des: 'We make your logo communicate with your customers more than words ever could',
  },
  {
    id: '02',
    img: 'https://images.unsplash.com/photo-1704677982215-a2248af6009b?q=80&w=1200&auto=format&fit=crop',
    title: 'Profitables',
    des: 'We are dedicated to unlocking your business potential through precision development',
  },
  {
    id: '03',
    img: 'https://images.unsplash.com/photo-1520256862855-398228c41684?q=80&w=800&auto=format&fit=crop',
    title: 'Topserve-copiers',
    des: 'We are dedicated to unlocking your business potential through precision development',
  },
  {
    id: '04',
    img: 'https://images.unsplash.com/photo-1605733160314-4fc7dac4bb16?q=80&w=800&auto=format&fit=crop',
    title: 'Labramart.',
    des: 'We specialize in crafting marketing solutions that propel your brand to new heights',
  },
];

export default function ConditionGrid() {
  return (
    <>
      <div className='grid grid-cols-12  gap-4 overflow-hidden px-5 lg:pb-5 pb-2'>
        {projects.map((project, index) => {
          let colSpanClass = 'sm:col-span-6 col-span-12 ';
          if (index === 0) {
            colSpanClass = ' sm:col-span-5 col-span-12 ';
          } else if (index === 1) {
            colSpanClass = 'sm:col-span-7 col-span-12 ';
          } else if (index === projects.length - 2) {
            colSpanClass = 'sm:col-span-7 col-span-12 ';
          } else if (index === projects.length - 1) {
            colSpanClass = 'sm:col-span-5 col-span-12 ';
          }
          return (
            <>
              <motion.article
                key={project.id ?? project.title}
                initial={{ y: 50, opacity: 0 }}
                whileInView={{ y: 0, opacity: 1 }}
                transition={{ ease: 'easeOut' }}
                viewport={{ once: false }}
                className={` relative  ${colSpanClass} `}
              >
                <div className='w-auto h-full'>
                  <Image
                    src={project?.img}
                    alt={'image'}
                    height={600}
                    width={1200}
                    className='h-full w-full object-cover rounded-xl'
                  />
                </div>
                <div className='absolute lg:bottom-2 bottom-0 text-black w-full p-4 flex justify-between items-center'>
                  <h3 className='lg:text-xl text-sm bg-black text-white rounded-xl p-2 px-4'>
                    {project.title}
                  </h3>
                  <div className='lg:w-12 w-10 lg:h-12 h-10 text-white grid place-content-center rounded-full bg-black'>
                    <MoveUpRight />
                  </div>
                </div>
              </motion.article>
            </>
          );
        })}
      </div>
    </>
  );
}

demo.tsx
import ConditionGrid from "@/components/ui/conditiongrid";

export default function Default() {
  return (
    <div className="w-full">
      <ConditionGrid />
    </div>
  );
}
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
