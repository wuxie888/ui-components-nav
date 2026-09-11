<!-- GitHub Repository Cards · @cnippet-dev · https://21st.dev/@cnippet-dev/components/v-card-16
     license: MIT · category: stat
     A vertical list of GitHub repository cards showing name, description, visibility badge, language, star and fork counts. -->

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
components/ui/v-card-16.tsx
import { GitBranch, GitForkIcon, StarIcon } from "lucide-react";
import { Badge } from "@/registry/default/ui/badge";
import { Card, CardContent } from "@/registry/default/ui/card";

const repos = [
  {
    description: "A UI component library built with Base UI and Tailwind CSS.",
    forks: 248,
    langColor: "bg-blue-500",
    language: "TypeScript",
    name: "ui-cnippet",
    stars: 1_420,
  },
  {
    description: "Minimal blogging starter with MDX, Tailwind, and Next.js.",
    forks: 91,
    langColor: "bg-blue-500",
    language: "TypeScript",
    name: "next-blog-starter",
    stars: 673,
  },
  {
    description: "Lightweight state manager for React with zero boilerplate.",
    forks: 57,
    langColor: "bg-yellow-400",
    language: "JavaScript",
    name: "micro-store",
    stars: 312,
  },
];

function fmt(n: number) {
  return n >= 1000 ? `${(n / 1000).toFixed(1)}k` : String(n);
}

export function Pattern() {
  return (
    <div className="flex w-full max-w-sm flex-col gap-3">
      {repos.map((repo) => (
        <Card className="w-full" key={repo.name}>
          <CardContent className="flex flex-col gap-2.5">
            <div className="flex items-start justify-between gap-2">
              <div className="flex items-center gap-2">
                <GitBranch className="size-4 shrink-0 text-muted-foreground" />
                <span className="font-semibold text-sm">{repo.name}</span>
              </div>
              <Badge size="sm" variant="secondary">
                Public
              </Badge>
            </div>
            <p className="text-muted-foreground text-xs leading-relaxed">
              {repo.description}
            </p>
            <div className="flex items-center gap-4 text-muted-foreground text-xs">
              <span className="flex items-center gap-1">
                <span className={`size-2.5 rounded-full ${repo.langColor}`} />
                {repo.language}
              </span>
              <span className="flex items-center gap-1">
                <StarIcon className="size-3" />
                {fmt(repo.stars)}
              </span>
              <span className="flex items-center gap-1">
                <GitForkIcon className="size-3" />
                {fmt(repo.forks)}
              </span>
            </div>
          </CardContent>
        </Card>
      ))}
    </div>
  );
}

demo.tsx
import { Pattern } from "@/components/ui/v-card-16";

export default function Default() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center p-6">
      <Pattern />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add badge card
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
