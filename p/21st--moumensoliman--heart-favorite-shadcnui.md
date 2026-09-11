<!-- Heart Favorite · @moumensoliman · https://21st.dev/@moumensoliman/components/heart-favorite-shadcnui
     license: MIT · category: icon
     Animated heart icon for like/favorite actions -->

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
components/ui/heart-favorite.tsx
"use client";

import { motion } from "framer-motion";
import { Heart } from "lucide-react";
import { useState } from "react";

export function HeartFavorite() {
  const [isLiked, setIsLiked] = useState(false);

  return (
    <div className="flex items-center justify-center p-12">
      <motion.button
        onClick={() => setIsLiked(!isLiked)}
        whileTap={{ scale: 0.9 }}
        className="rounded-full p-4 transition-colors hover:bg-gray-100 dark:hover:bg-gray-800"
      >
        <motion.div
          animate={{
            scale: isLiked ? [1, 1.3, 1] : 1,
          }}
          transition={{
            duration: 0.3,
            ease: "easeInOut",
          }}
        >
          <Heart
            className={`h-8 w-8 transition-colors ${
              isLiked ? "fill-red-500 text-red-500" : "text-gray-400"
            }`}
          />
        </motion.div>
      </motion.button>
    </div>
  );
}

demo.tsx
import { HeartFavorite } from "@/components/ui/heart-favorite-shadcnui"

export default function Demo() {
  return (
    <div className="flex min-h-screen items-center justify-center bg-background p-8">
      <HeartFavorite />
    </div>
  )
}
```

Install NPM dependencies:
```bash
npm install framer-motion lucide-react
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
