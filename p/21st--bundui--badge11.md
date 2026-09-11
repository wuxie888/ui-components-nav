<!-- Badge with Avatar · @bundui · https://21st.dev/@bundui/components/badge11
     license: MIT · category: avatar
     A dismissible outline badge that shows a user avatar with name and a remove button, useful for selected users or tags. -->

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
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar";
import { Badge } from "@/components/ui/badge";
import { XIcon } from "lucide-react";

interface User {
  id: number;
  name: string;
  image: string;
}

const users: User[] = [
  {
    id: 1,
    name: "Alessandro",
    image: "https://i.pravatar.cc/150?img=1",
  },
  {
    id: 2,
    name: "Jane Smith",
    image: "https://i.pravatar.cc/150?img=2",
  },
];

export default function BadgeComponent() {
  return (
    <div className="flex flex-wrap gap-2">
      {users.map((user) => (
        <Badge key={user.id} variant="outline" className="pr-1.5 pl-0.5">
          <Avatar className="size-4">
            <AvatarImage src={user.image} alt={user.name} />
            <AvatarFallback>
              {user.name
                .split(" ")
                .map((n) => n[0])
                .join("")}
            </AvatarFallback>
          </Avatar>
          <span>{user.name}</span>
          <button type="button">
            <XIcon className="size-3 cursor-pointer opacity-60 hover:opacity-100" />
          </button>
        </Badge>
      ))}
    </div>
  );
}

demo.tsx
import BadgeComponent from "@/components/ui/badge11";

export default function Default() {
  return (
    <div className="flex min-h-[200px] w-full items-center justify-center p-8">
      <BadgeComponent />
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
npx shadcn@latest add avatar badge
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
