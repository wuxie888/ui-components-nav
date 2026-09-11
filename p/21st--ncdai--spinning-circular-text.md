<!-- Spinning Circular Text · @ncdai · https://21st.dev/@ncdai/components/spinning-circular-text
     license: MIT · category: text
     Text arranged in a circle with a continuous spinning animation, with adjustable character spacing and font size. -->

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
components/spinning-circular-text/spinning-circular-text.tsx
import { cn } from "@/lib/utils"

export type SpinningCircularTextProps = Omit<
  React.ComponentProps<"div">,
  "children"
> & {
  text: string

  /**
   * @defaultValue 1
   * */
  charSpacing?: number

  /**
   * @defaultValue 1rem
   * */
  fontSize?: string

  /**
   * Class names applied to the spinning ring, e.g. to override the
   * animation duration (`duration-[10s]`) or easing.
   * */
  spinClassName?: string

  /**
   * Customize how each character is rendered, e.g. to wrap it in a
   * `motion.span` for per-character effects. The returned node is placed
   * inside a positioned wrapper, so positioning is handled for you.
   * */
  renderChar?: (char: string, index: number) => React.ReactNode
}

export function SpinningCircularText({
  text,
  charSpacing = 1,
  fontSize = "1rem",
  spinClassName,
  renderChar,
  className,
  style,
  ...props
}: SpinningCircularTextProps) {
  return (
    <div
      className={cn(
        "grid size-(--sc-container-size) place-items-center font-mono font-medium uppercase select-none",
        className
      )}
      style={
        {
          "--sc-size": fontSize,
          "--sc-char-count": text.length,
          "--sc-char-spacing": charSpacing,
          "--sc-inner-angle": "calc((360 / var(--sc-char-count)) * 1deg)",
          "--sc-radius-factor":
            "calc(var(--sc-char-spacing) / sin(var(--sc-inner-angle)))",
          "--sc-radius": "calc(var(--sc-radius-factor) * -1ch)",
          "--sc-container-size":
            "calc(var(--sc-radius-factor) * var(--sc-size) * 2)",
          ...style,
        } as React.CSSProperties
      }
      {...props}
    >
      <div
        className={cn(
          "relative animate-spin-ccw text-(size:--sc-size) leading-none",
          "*:absolute *:top-1/2 *:left-1/2 *:inline-block",
          "*:[--sc-char-rotate:calc(var(--sc-inner-angle)*var(--sc-char-index))]",
          "*:transform-[translate(-50%,-50%)_rotate(var(--sc-char-rotate))_translateY(var(--sc-radius))]",
          spinClassName
        )}
        aria-hidden
      >
        {text.split("").map((char, index) => (
          <span
            key={index}
            style={{ "--sc-char-index": index } as React.CSSProperties}
          >
            {renderChar ? renderChar(char, index) : char}
          </span>
        ))}
      </div>
      <span className="sr-only">{text}</span>
    </div>
  )
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
