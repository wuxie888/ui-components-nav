<!-- Testimonial Section · @solaceui · https://21st.dev/@solaceui/components/testimonial-section-1
     license: no-license · category: testimonials
     A responsive bento-grid testimonial section with animated quote cards showing customer names, roles, and avatars. -->

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
"use client";

import React from "react";
import Image from "next/image";
import { cn } from "@/lib/utils";
import { motion, Variants } from "motion/react";

interface Testimonial {
  name: string;
  role: string;
  image: string;
  quote: string;
  className: string;
  imageBorderColor: string;
}

const testimonials: Testimonial[] = [
  {
    name: "Sarah Chen",
    role: "CEO of DataFlow Technologies",
    image: "https://assets.solaceui.com/solaceui-member-five.png",
    quote:
      "SolaceUI transformed our design workflow. What used to take weeks now takes days, and our product consistency has never been better.",
    className:
      "lg:col-span-1 lg:row-span-2 bg-(--color-primary) text-white border-transparent",
    imageBorderColor: "border-black",
  },
  {
    name: "Marcus Rodriguez",
    role: "CEO of Quantum Labs",
    image: "https://assets.solaceui.com/solaceui-member-one.png",
    quote:
      "We've tried every UI framework out there. SolaceUI is the first that actually delivers. Our time-to-market improved by 40%",
    className:
      "lg:col-span-1 bg-white dark:bg-black border border-neutral-300 dark:border-neutral-600 text-black dark:text-white",
    imageBorderColor: "border-(--color-primary)",
  },
  {
    name: "Oilivia Koe",
    role: "CEO of Nexus Digital",
    image: "https://assets.solaceui.com/solaceui-member-six.png",
    quote:
      "SolaceUI has become non-negotiable in our tech stack. It's elegant, powerful, and scaled beautifully with us from seed to Series B",
    className:
      "lg:col-span-1 lg:row-span-2 bg-black dark:bg-white text-white dark:text-black border-transparent ",
    imageBorderColor: "border-(--color-primary)",
  },
  {
    name: "David Kim",
    role: "CEO of Streamline Software",
    image: "https://assets.solaceui.com/solaceui-member-two.png",
    quote:
      "Perfect balance between flexibility and structure. Our design system went from scattered mess to cohesive asset. ROI was evident in Q1.",
    className:
      "lg:col-span-1 bg-white dark:bg-black border border-neutral-300 dark:border-neutral-600 text-black dark:text-white",
    imageBorderColor: "border-(--color-primary)",
  },
  {
    name: "James Mitchell",
    role: "CEO of Visionary Apps",
    image: "https://assets.solaceui.com/solaceui-member-four.png",
    quote:
      "The accessibility features are built-in, not bolted-on. It's rare to find a tool that makes both designers and developers genuinely happy",
    className:
      "lg:col-span-1 bg-black  dark:bg-white text-white dark:text-black border-neutral-800",
    imageBorderColor: "border-(--color-primary)",
  },
  {
    name: "Amara Okonkwo",
    role: "CEO of Horizon Platforms",
    image: "https://assets.solaceui.com/solaceui-member-three.png",
    quote:
      "We migrated our entire product in under three months. The performance improvements alone justified the switch. Highly recommend",
    className:
      "lg:col-span-2 bg-(--color-primary) text-white border-transparent",
    imageBorderColor: "border-black",
  },
];

export default function Testimonial1() {
  const containerVariants = {
    hidden: { opacity: 0 },
    visible: {
      opacity: 1,
      transition: {
        staggerChildren: 0.1,
      },
    },
  };

  const itemVariants: Variants = {
    hidden: { opacity: 0, y: 20, filter: "blur(4px)" },
    visible: {
      opacity: 1,
      y: 0,
      filter: "blur(0px)",
      transition: {
        duration: 0.4,
        ease: "easeOut" as const,
      },
    },
  };

  return (
    <div className="w-full py-10 [--color-primary:#003AF9] bg-white dark:bg-black text-neutral-900 dark:text-neutral-100">
      <div className="max-w-7xl mx-auto px-4 md:px-8">
        <h2 className="text-2xl md:text-3xl font-bold text-center mb-10 tracking-tight">
          Trusted By The Best People
        </h2>
        <motion.div
          className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 auto-rows-auto"
          variants={containerVariants}
          initial="hidden"
          whileInView="visible"
          viewport={{ once: true, margin: "-100px" }}
        >
          {testimonials.map((testimonial, index) => (
            <motion.div
              key={index}
              variants={itemVariants}
              whileHover={{ scale: 1.02 }}
              className={cn(
                "p-8 rounded-lg flex flex-col justify-between",
                testimonial.className,
              )}
            >
              <p
                className={cn(
                  "font-medium leading-relaxed mb-8",
                  testimonial.className.includes("lg:row-span-2") ||
                    testimonial.className.includes("col-span-2")
                    ? "text-xl md:text-2xl lg:text-3xl"
                    : "text-base",
                )}
              >
                {testimonial.quote}
              </p>
              <div className="flex items-center gap-4">
                <div
                  className={cn(
                    "relative w-12 h-12 rounded-full overflow-hidden border-[1.5px]",
                    testimonial.imageBorderColor,
                  )}
                >
                  <Image
                    src={testimonial.image}
                    alt={testimonial.name}
                    fill
                    className="object-cover object-top"
                    sizes="48px"
                    loading="lazy"
                    unoptimized
                  />
                </div>
                <div>
                  <h4 className="font-bold text-base">{testimonial.name}</h4>
                  <p className="text-sm opacity-80">{testimonial.role}</p>
                </div>
              </div>
            </motion.div>
          ))}
        </motion.div>
      </div>
    </div>
  );
}

demo.tsx
import Testimonial1 from "@/components/ui/testimonial-section-1";

export default function DemoTestimonialSection() {
  return <Testimonial1 />;
}
```

Install NPM dependencies:
```bash
npm install motion
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
