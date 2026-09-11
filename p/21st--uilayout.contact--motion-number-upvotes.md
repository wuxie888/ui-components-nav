<!-- Upvote Downvote Counter · @uilayout.contact · https://21st.dev/@uilayout.contact/components/motion-number-upvotes
     license: MIT · category: card
     An animated upvote and downvote counter with smoothly transitioning numbers and color feedback on the active vote. -->

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
components/ui/motion-number-upvotes.tsx
'use client';
import NumberFlow from '@number-flow/react';
import { ArrowDown, ArrowUp } from 'lucide-react';
import { motion } from 'motion/react';
import type React from 'react';
import { useState } from 'react';

const UpvoteDownvote: React.FC = () => {
  const [votes, setVotes] = useState(14); // Initial votes
  const [activeVote, setActiveVote] = useState<'up' | 'down' | null>(null); // Track active arrow

  const handleVote = (state: 'up' | 'down') => {
    if (state === 'up') {
      setVotes(votes + 1);
      setActiveVote(state);
    } else {
      setVotes(votes - 1);
      setActiveVote(state);
    }
  };

  return (
    <div
      className={`flex flex-col items-center gap-2 p-4  border rounded-xl shadow-lg w-fit mx-auto ${
        activeVote === 'up'
          ? 'dark:bg-green-950 bg-green-300 border-green-600 text-white'
          : activeVote === 'down'
            ? 'dark:bg-red-950 bg-red-300 border-red-600 text-white'
            : 'dark:bg-neutral-950 bg-neutral-50 text-primary'
      }`}
    >
      <div className='  text-lg font-medium'>
        <NumberFlow value={votes} format={{ notation: 'compact' }} /> Upvotes
      </div>

      <div className='flex items-center gap-4'>
        <motion.button
          whileTap={{ scale: 0.9 }}
          onClick={() => handleVote('up')}
          className={`p-2 rounded-full text-white ${
            activeVote === 'up' ? 'bg-green-500 ' : ' bg-black '
          } transition-colors`}
        >
          <ArrowUp size={24} />
        </motion.button>

        <motion.button
          whileTap={{ scale: 0.9 }}
          onClick={() => handleVote('down')}
          className={`p-2 rounded-full text-white ${
            activeVote === 'down' ? 'bg-red-500 ' : ' bg-black '
          } transition-colors`}
        >
          <ArrowDown size={24} />
        </motion.button>
      </div>
    </div>
  );
};

export default UpvoteDownvote;

demo.tsx
import UpvoteDownvote from "@/components/ui/motion-number-upvotes";

export default function Demo() {
  return (
    <div className="flex min-h-[300px] w-full items-center justify-center bg-background p-8">
      <UpvoteDownvote />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @number-flow/react lucide-react motion
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
