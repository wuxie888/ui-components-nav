<!-- Reveal images · @hari · https://21st.dev/@hari/components/reveal-images
     license: MIT · category: gallery
     Reveals the images on hover. -->

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
components/ui/reveal-images.tsx
import { cn } from "@/lib/utils"

interface ImageSource {
  src: string
  alt: string
}

interface ShowImageListItemProps {
  text: string
  images: [ImageSource, ImageSource]
}

function RevealImageListItem({ text, images }: ShowImageListItemProps) {
  const container = "absolute right-8 -top-1 z-40 h-20 w-16"
  const effect =
    "relative duration-500 delay-100 shadow-none group-hover:shadow-xl scale-0 group-hover:scale-100 opacity-0 group-hover:opacity-100 group-hover:w-full group-hover:h-full w-16 h-16 overflow-hidden transition-all rounded-md"

  return (
    <div className="group relative h-fit w-fit overflow-visible py-8">
      <h1 className="text-7xl font-black text-foreground transition-all duration-500 group-hover:opacity-40">
        {text}
      </h1>
      <div className={container}>
        <div className={effect}>
          <img
            alt={images[1].alt}
            src={images[1].src}
            className="h-full w-full object-cover"
          />
        </div>
      </div>
      <div
        className={cn(
          container,
          "translate-x-0 translate-y-0 rotate-0 transition-all delay-150 duration-500 group-hover:translate-x-6 group-hover:translate-y-6 group-hover:rotate-12"
        )}
      >
        <div className={cn(effect, "duration-200")}>
          <img
            alt={images[0].alt}
            src={images[0].src}
            className="h-full w-full object-cover"
          />
        </div>
      </div>
    </div>
  )
}

function RevealImageList() {
  const items: ShowImageListItemProps[] = [
    {
      text: "Branding",
      images: [
        {
          src: "https://cdn.21st.dev/assets/mirror/ed/edd2d257dfcaa4ddc45bc155244ca3712266444956c1c45c0ffb1539525943f7.jpg",
          alt: "Image 1",
        },
        {
          src: "https://cdn.21st.dev/assets/mirror/d9/d9de7955883c7d06fa312a44bfa62d69088739a242edded789f302b9af831a7a.jpg",
          alt: "Image 2",
        },
      ],
    },
    {
      text: "Web design",
      images: [
        {
          src: "https://cdn.21st.dev/assets/mirror/98/9850bacb9ec1d83f36d29a34f4c645a0ab969e3de65143d8feee49981ece79cc.jpg",
          alt: "Image 1",
        },
        {
          src: "https://cdn.21st.dev/assets/mirror/c2/c2204e51d543c4d6b1f0157bfd1d4f0a460bc22826fb82ba359010f9b2faaacc.jpg",
          alt: "Image 2",
        },
      ],
    },
    {
      text: "Illustration",
      images: [
        {
          src: "https://cdn.21st.dev/assets/mirror/be/be5d186f9dcb2f5a4babf291fcdc6ddaffd6655e24f43d4a15bc977d59644629.jpg",
          alt: "Image 1",
        },
        {
          src: "https://cdn.21st.dev/assets/mirror/75/75061d61b1a21f6b1f377a3b56fddb0b917018c85c4203c143eabf301086934a.jpg",
          alt: "Image 2",
        },
      ],
    },
  ]

  return (
    <div className="flex flex-col gap-1 rounded-sm bg-background px-8 py-4">
      <h3 className="text-sm font-black uppercase text-muted-foreground">
        Our services
      </h3>
      {items.map((item, index) => (
        <RevealImageListItem key={index} text={item.text} images={item.images} />
      ))}
    </div>
  )
}

export { RevealImageList }

demo.tsx
import { RevealImageList } from "@/components/ui/reveal-images"

function RevealImageListDemo() {
  return (
    <div className="block">
      < RevealImageList />
    </div>
  );
}

export { RevealImageListDemo };
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
