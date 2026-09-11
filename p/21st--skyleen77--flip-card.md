<!-- Flip Card · @skyleen77 · https://21st.dev/@skyleen77/components/flip-card
     license: MIT · category: profile
     A 3D profile card that flips on hover or tap to reveal a bio, stats, and social links. -->

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
components/community/flip-card/index.tsx
/* eslint-disable @next/next/no-img-element */
'use client';

import { Button } from '@/components/ui/button';
import { easeOut, motion } from 'motion/react';
import * as React from 'react';
import { Github, Linkedin, Twitter } from 'lucide-react';

export interface FlipCardData {
  name: string;
  username: string;
  image: string;
  bio: string;
  stats: {
    following: number;
    followers: number;
    posts?: number;
  };
  socialLinks?: {
    linkedin?: string;
    github?: string;
    twitter?: string;
  };
}

interface FlipCardProps {
  data: FlipCardData;
}

export function FlipCard({ data }: FlipCardProps) {
  const [isFlipped, setIsFlipped] = React.useState(false);

  const isTouchDevice =
    typeof window !== 'undefined' && 'ontouchstart' in window;

  const handleClick = () => {
    if (isTouchDevice) setIsFlipped(!isFlipped);
  };

  const handleMouseEnter = () => {
    if (!isTouchDevice) setIsFlipped(true);
  };

  const handleMouseLeave = () => {
    if (!isTouchDevice) setIsFlipped(false);
  };

  const cardVariants = {
    front: { rotateY: 0, transition: { duration: 0.5, ease: easeOut } },
    back: { rotateY: 180, transition: { duration: 0.5, ease: easeOut } },
  };

  return (
    <div
      className="mt-2 relative w-40 h-60 md:w-60 md:h-80 perspective-1000 cursor-pointer mx-auto"
      onClick={handleClick}
      onMouseEnter={handleMouseEnter}
      onMouseLeave={handleMouseLeave}
    >
      {/* FRONT: Profile */}
      <motion.div
        className="absolute inset-0 backface-hidden rounded-md border-2 border-foreground/20 px-4 py-6 flex flex-col items-center justify-center bg-gradient-to-br from-muted via-background to-muted text-center"
        animate={isFlipped ? 'back' : 'front'}
        variants={cardVariants}
        style={{ transformStyle: 'preserve-3d' }}
      >
        <img
          src={data.image}
          alt={data.name}
          className="size-20 md:size-24 rounded-full object-cover mb-4 border-2"
        />
        <h2 className="text-lg font-bold text-foreground">{data.name}</h2>
        <p className="text-sm text-muted-foreground">@{data.username}</p>
      </motion.div>

      {/* BACK: Bio + Stats + Socials */}
      <motion.div
        className="absolute inset-0 backface-hidden rounded-md border-2 border-foreground/20 px-4 py-6 flex flex-col justify-between items-center gap-y-4 bg-gradient-to-tr from-muted via-background to-muted "
        initial={{ rotateY: 180 }}
        animate={isFlipped ? 'front' : 'back'}
        variants={cardVariants}
        style={{ transformStyle: 'preserve-3d', rotateY: 180 }}
      >
        <p className="text-xs md:text-sm text-muted-foreground text-center">
          {data.bio}
        </p>

        <div className="px-6 flex items-center justify-between w-full">
          <div>
            <p className="text-base font-bold">{data.stats.following}</p>
            <p className="text-xs text-muted-foreground">Following</p>
          </div>
          <div>
            <p className="text-base font-bold">{data.stats.followers}</p>
            <p className="text-xs text-muted-foreground">Followers</p>
          </div>
          {data.stats.posts && (
            <div>
              <p className="text-base font-bold">{data.stats.posts}</p>
              <p className="text-xs text-muted-foreground">Posts</p>
            </div>
          )}
        </div>

        {/* Social Media Icons */}
        <div className="flex items-center justify-center gap-4">
          {data.socialLinks?.linkedin && (
            <a
              href={data.socialLinks.linkedin}
              target="_blank"
              rel="noopener noreferrer"
              className="hover:scale-105 transition-transform"
            >
              <Linkedin size={20} />
            </a>
          )}
          {data.socialLinks?.github && (
            <a
              href={data.socialLinks.github}
              target="_blank"
              rel="noopener noreferrer"
              className="hover:scale-105 transition-transform"
            >
              <Github size={20} />
            </a>
          )}
          {data.socialLinks?.twitter && (
            <a
              href={data.socialLinks.twitter}
              target="_blank"
              rel="noopener noreferrer"
              className="hover:scale-105 transition-transform"
            >
              <Twitter size={20} />
            </a>
          )}
        </div>

        <Button>Follow</Button>
      </motion.div>
    </div>
  );
}

demo.tsx
"use client";

import { FlipCard } from "@/components/ui/flip-card";

const data = {
  name: "Animate UI",
  username: "animate_ui",
  image:
    "https://cdn.21st.dev/assets/localized/2714e0a93ebb6f7921d0ba9edbcb5c9a2db2b3bac7af1cfa94ff24664b49eede.jpg",
  bio: "A fully animated, open-source component distribution built with React, TypeScript, Tailwind CSS, and Motion.",
  stats: { following: 200, followers: 2900, posts: 120 },
  socialLinks: {
    linkedin: "https://linkedin.com",
    github: "https://github.com",
    twitter: "https://twitter.com",
  },
};

export default function FlipCardDemo() {
  return <FlipCard data={data} />;
}
```

Install NPM dependencies:
```bash
npm install class-variance-authority lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button
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
