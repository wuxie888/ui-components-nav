<!-- Tags Input · @uilayout.contact · https://21st.dev/@uilayout.contact/components/tags-input-2
     license: no-license · category: form
     An input field for adding, editing, and removing multiple tags using Enter or comma keys. -->

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
components/ui/tags-input.tsx
'use client';
import { cn } from '@/lib/utils';
import type React from 'react';
import { useEffect, useRef, useState } from 'react';

interface TagsInputProps {
  tags: string[];
  setTags: React.Dispatch<React.SetStateAction<string[]>>;
  editTag?: boolean;
  className?: string;
}

export const TagsInput: React.FC<TagsInputProps> = ({
  tags,
  setTags,
  editTag = true,
  className,
}) => {
  const [input, setInput] = useState('');
  const [editingIndex, setEditingIndex] = useState<number | null>(null);
  const editInputRef = useRef<HTMLInputElement>(null);

  const handleAddTag = (e: React.KeyboardEvent<HTMLInputElement>) => {
    const trimmedInput = input.trim();

    if ((e.key === 'Enter' || e.key === ',') && trimmedInput) {
      e.preventDefault();
      if (editingIndex !== null) {
        const updatedTags = [...tags];
        updatedTags[editingIndex] = trimmedInput;
        setTags(updatedTags);
        setEditingIndex(null);
      } else if (!tags.includes(trimmedInput)) {
        setTags([...tags, trimmedInput]);
      }
      setInput('');
    }
  };

  const handleRemoveTag = (tag: string) => {
    setTags(tags.filter((t) => t !== tag));
    if (editingIndex !== null) {
      setEditingIndex(null);
    }
  };

  const handleEditTag = (index: number) => {
    if (editTag) {
      setInput(tags[index]);
      setEditingIndex(index);
      setTimeout(() => editInputRef.current?.focus(), 0);
    }
  };

  const handleBlur = () => {
    if (editingIndex !== null) {
      const updatedTags = [...tags];
      const trimmedInput = input.trim();
      if (trimmedInput) {
        updatedTags[editingIndex] = trimmedInput;
      } else {
        updatedTags.splice(editingIndex, 1);
      }
      setTags(updatedTags);
      setEditingIndex(null);
    }
    setInput('');
  };

  useEffect(() => {
    if (editInputRef.current) {
      editInputRef.current.style.width = `${input.length + 1}ch`;
    }
  }, [input]);

  return (
    <div
      className={cn(
        'flex flex-wrap w-full items-center gap-2 p-2 border-2 rounded-md focus-within:border-blue-500 dark:bg-neutral-950 bg-neutral-50',
        className
      )}
    >
      {tags.map((tag, index) => (
        <div key={tag} className='relative'>
          {editTag && editingIndex === index ? (
            <input
              ref={editInputRef}
              type='text'
              value={input}
              onChange={(e) => setInput(e.target.value)}
              onKeyDown={handleAddTag}
              onBlur={handleBlur}
              className='px-2 py-1 text-sm border dark:bg-neutral-800 bg-neutral-200 rounded-sm outline-hidden'
              placeholder='Edit tag...'
              style={{ width: `${input.length + 1 * 1.2}px` }}
            />
          ) : (
            <div
              onClick={() => handleEditTag(index)}
              className='flex items-center gap-2 px-1 pl-2 py-1 text-sm font-medium text-white bg-blue-500 rounded-sm cursor-pointer hover:bg-blue-600'
            >
              {tag}
              <button
                onClick={(e) => {
                  e.stopPropagation();
                  handleRemoveTag(tag);
                }}
                className='text-primary px-1 focus:outline-hidden bg-primary-base rounded-sm'
              >
                &times;
              </button>
            </div>
          )}
        </div>
      ))}
      <input
        type='text'
        value={input}
        onChange={(e) => setInput(e.target.value)}
        onKeyDown={handleAddTag}
        className={`grow px-2 py-1 text-sm border-none outline-hidden  dark:bg-neutral-800 bg-neutral-50 rounded-md ${
          editingIndex !== null ? 'opacity-0' : 'opacity-100'
        }`}
        placeholder='Add a tag...'
      />
    </div>
  );
};

demo.tsx
'use client';
import { TagsInput } from '@/components/ui/tags-input';
import React, { useState } from 'react';

export default function App() {
  const [tags, setTags] = useState<string[]>(['react', 'nextjs']);

  return (
    <div className='max-w-xl mx-auto py-16'>
      <TagsInput tags={tags} setTags={setTags} className='lg:w-96' />
    </div>
  );
}
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
