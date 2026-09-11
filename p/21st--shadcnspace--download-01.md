<!-- Multi-Platform Download Grid · @shadcnspace · https://21st.dev/@shadcnspace/components/download-01
     license: no-license · category: features
     A responsive download section with cards for every platform — iOS, Android, web, desktop, and browser extensions — so users can quickly grab the right app. -->

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
components/shadcn-space/blocks/download-01/page.tsx
import Download from "@/components/shadcn-space/blocks/download-01/download";

const Page = () => {
    return <Download />;
};

export default Page;

components/shadcn-space/blocks/download-01/download.tsx
"use client";

import { Badge } from "@/components/ui/badge";
import { Button } from "@/components/ui/button";
import { ArrowRight } from "lucide-react";
import { motion } from "motion/react";

interface Platform {
    iconSrc: string;
    iconDarkSrc?: string;
    iconAlt: string;
    title: string;
    description: string;
    buttonLabel: string;
}

const platforms: Platform[] = [
    {
        iconSrc: "https://images.shadcnspace.com/assets/svgs/apple-dark.svg",
        iconDarkSrc: "https://images.shadcnspace.com/assets/svgs/apple-light.svg",
        iconAlt: "iOS",
        title: "IOS App",
        description: "Manage tasks and stay productive wherever your work takes you.",
        buttonLabel: "Download on the app store",
    },
    {
        iconSrc: "https://images.shadcnspace.com/assets/svgs/android-2.svg",
        iconAlt: "Android",
        title: "Android App",
        description: "Access projects and communicate with your team from any Android device.",
        buttonLabel: "Download on the play store",
    },
    {
        iconSrc: "https://images.shadcnspace.com/assets/svgs/html-5.svg",
        iconAlt: "HTML5",
        title: "Web App",
        description: "Work directly from your browser with access to all features and updates.",
        buttonLabel: "Open web app",
    },
    {
        iconSrc: "https://images.shadcnspace.com/assets/svgs/apple-dark.svg",
        iconDarkSrc: "https://images.shadcnspace.com/assets/svgs/apple-light.svg",
        iconAlt: "Mac",
        title: "Mac App",
        description: "Enjoy a native desktop experience with enhanced performance.",
        buttonLabel: "Download",
    },
    {
        iconSrc: "https://images.shadcnspace.com/assets/svgs/linux.svg",
        iconAlt: "Linux",
        title: "Linux App",
        description: "Built for flexibility and speed, giving users full access to every platform feature.",
        buttonLabel: "Download",
    },
    {
        iconSrc: "https://images.shadcnspace.com/assets/svgs/windows.svg",
        iconAlt: "Windows",
        title: "Windows App",
        description: "Stay focused with a powerful desktop application.",
        buttonLabel: "Download",
    },
    {
        iconSrc: "https://images.shadcnspace.com/assets/svgs/safari.svg",
        iconAlt: "Safari",
        title: "Safari Extension",
        description: "Capture ideas, save resources, & access all information without leaving browser.",
        buttonLabel: "Add extension",
    },
    {
        iconSrc: "https://images.shadcnspace.com/assets/svgs/chrome.svg",
        iconAlt: "Chrome",
        title: "Chrome Extension",
        description: "Quickly save content, manage bookmarks, & streamline your workflow across web.",
        buttonLabel: "Add extension",
    },
    {
        iconSrc: "https://images.shadcnspace.com/assets/svgs/firefox.svg",
        iconAlt: "Firefox",
        title: "Firefox Extension",
        description: "Boost productivity with one-click access to tools, notes, and saved content.",
        buttonLabel: "Add extension",
    },
];

const containerVariants = {
    hidden: { opacity: 0 },
    visible: {
        opacity: 1,
        transition: { staggerChildren: 0.08 },
    },
};

const itemVariants = {
    hidden: { opacity: 0, y: 20 },
    visible: {
        opacity: 1,
        y: 0,
        transition: { duration: 0.4, ease: "easeOut" },
    },
};

export default function Download() {
    return (
        <section className="bg-muted dark:bg-background">
            <div className="py-16 sm:py-20 lg:py-20">
                <div className="max-w-7xl mx-auto px-4 lg:px-8 xl:px-16">
                    {/* Header */}
                    <motion.div
                        initial={{ opacity: 0, y: 20 }}
                        whileInView={{ opacity: 1, y: 0 }}
                        viewport={{ once: true }}
                        transition={{ duration: 0.5, ease: "easeOut" }}
                        className="flex flex-col items-center gap-4 mb-12"
                    >
                        <Badge variant={"outline"} className="px-3 py-1 h-7 text-sm font-normal rounded-full bg-background text-foreground">
                            Platforms
                        </Badge>
                        <h2 className="text-3xl sm:text-4xl font-medium text-center tracking-tight max-w-xl leading-tight">
                            Available on every device you could possibly think about.
                        </h2>
                    </motion.div>
                    <motion.div
                        variants={containerVariants}
                        initial="hidden"
                        whileInView="visible"
                        viewport={{ once: true }}
                        className="grid lg:grid-cols-3 md:grid-cols-2 grid-cols-1 gap-6"
                    >
                        {platforms.map((platform) => (
                            <motion.div
                                key={platform.title}
                                variants={itemVariants}
                                className="bg-card border border-border rounded-2xl p-8 flex flex-col gap-10 w-full overflow-hidden">
                                <div className="border border-border rounded-lg p-3 w-fit">
                                    {platform.iconDarkSrc ? (
                                        <>
                                            <img
                                                src={platform.iconSrc}
                                                alt={platform.iconAlt}
                                                width={24}
                                                height={24}
                                                className="size-6 dark:hidden"
                                            />
                                            <img
                                                src={platform.iconDarkSrc}
                                                alt={platform.iconAlt}
                                                width={24}
                                                height={24}
                                                className="size-6 hidden dark:block"
                                            />
                                        </>
                                    ) : (
                                        <img
                                            src={platform.iconSrc}
                                            alt={platform.iconAlt}
                                            width={24}
                                            height={24}
                                            className="size-6"
                                        />
                                    )}
                                </div>
                                <div className="flex flex-col gap-5">
                                    <div className="flex flex-col gap-1">
                                        <p className="text-lg font-medium text-card-foreground">{platform.title}</p>
                                        <p className="text-base font-normal text-muted-foreground">{platform.description}</p>
                                    </div>
                                    <Button
                                        variant="outline"
                                        className="w-fit h-9 px-4 rounded-full text-sm font-medium gap-1.5 shadow-xs cursor-pointer bg-transparent group"
                                    >
                                        {platform.buttonLabel}
                                        <ArrowRight className="size-4 group-hover:translate-x-1 group-hover:-rotate-45 transition-all duration-200 ease-in-out" />
                                    </Button>
                                </div>
                            </motion.div>
                        ))}
                    </motion.div>
                </div>
            </div>
        </section>
    );
}

demo.tsx
import Download from "@/components/ui/download-01";

export default function DownloadDemo() {
  return <Download />;
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge button
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
