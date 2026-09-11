<!-- Animated Number · cult-ui · https://www.cult-ui.com/docs/components/animated-number
     license: MIT · category: input
     Number component with smooth counting animations and customizable formatting -->

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
components/ui/animated-number.tsx
"use client"

import { useEffect } from "react"
import { motion, MotionValue, useSpring, useTransform } from "motion/react"

interface AnimatedNumberProps {
  value: number
  mass?: number
  stiffness?: number
  damping?: number
  precision?: number
  format?: (value: number) => string
  onAnimationStart?: () => void
  onAnimationComplete?: () => void
}

export function AnimatedNumber({
  value,
  mass = 0.8,
  stiffness = 75,
  damping = 15,
  precision = 0,
  format = (num) => num.toLocaleString(),
  onAnimationStart,
  onAnimationComplete,
}: AnimatedNumberProps) {
  const spring = useSpring(value, { mass, stiffness, damping })
  const display: MotionValue<string> = useTransform(spring, (current) =>
    format(parseFloat(current.toFixed(precision)))
  )

  useEffect(() => {
    spring.set(value)
    if (onAnimationStart) onAnimationStart()
    const unsubscribe = spring.on("change", () => {
      if (spring.get() === value && onAnimationComplete) onAnimationComplete()
    })
    return () => unsubscribe()
  }, [spring, value, onAnimationStart, onAnimationComplete])

  return <motion.span>{display}</motion.span>
}

demo.tsx
"use client"

import { useState } from "react"
import { Minus, Plus } from "lucide-react"
import { toast } from "sonner"

import { Button } from "@/components/ui/button"
import { Slider } from "@/components/ui/slider"
import { AnimatedNumber } from "@/registry/default/ui/animated-number"
import { GradientHeading } from "@/registry/default/ui/gradient-heading"
import {
  TextureCardContent,
  TextureCardHeader,
  TextureCardStyled,
} from "@/registry/default/ui/texture-card"

function PrecisionExample() {
  const [value, setValue] = useState(14.5678)

  return (
    <TextureCardStyled>
      <TextureCardHeader className="pl-3">
        <GradientHeading size="xxs">Precision</GradientHeading>
      </TextureCardHeader>
      <TextureCardContent>
        <div className="flex gap-2">
          <div
            className="text-2xl font-bold"
            style={{ minWidth: "80px", textAlign: "left" }}
          >
            <AnimatedNumber value={value} precision={2} />
          </div>
          <Button
            size="sm"
            variant="ghost"
            className="border border-primary/10 rounded-full ml-auto py-5"
            onClick={() => setValue(value + 13.456)}
          >
            <Plus className="h-4 w-4" />
          </Button>
        </div>
      </TextureCardContent>
    </TextureCardStyled>
  )
}

function FormatExample() {
  const [value, setValue] = useState(10)

  const customFormat = (num: number) => `$${num.toFixed(2)}`

  return (
    <TextureCardStyled>
      <TextureCardHeader className="pl-3">
        <GradientHeading size="xxs">Format</GradientHeading>
      </TextureCardHeader>
      <TextureCardContent>
        <div className="flex gap-2">
          <div
            className="text-2xl font-bold"
            style={{ minWidth: "120px", textAlign: "left" }}
          >
            <AnimatedNumber value={value} format={customFormat} />
          </div>
          <Button
            size="sm"
            variant="ghost"
            className="border border-primary/10 rounded-full ml-auto py-5"
            onClick={() => setValue(value + 50)}
          >
            <Plus className="h-4 w-4" />
          </Button>
        </div>
      </TextureCardContent>
    </TextureCardStyled>
  )
}

function HooksExample() {
  const [value, setValue] = useState(10)

  const handleAnimationStart = () => {
    toast("🏁 Animation started ")
  }

  const handleAnimationComplete = () => {
    toast("✅ Animation completed ")
  }

  return (
    <TextureCardStyled>
      <TextureCardHeader className="pl-3">
        <GradientHeading size="xxs">Callbacks</GradientHeading>
      </TextureCardHeader>
      <TextureCardContent>
        <div className="flex gap-2">
          <div
            className="text-2xl font-bold"
            style={{ minWidth: "50px", textAlign: "left" }}
          >
            <AnimatedNumber
              value={value}
              onAnimationStart={handleAnimationStart}
              onAnimationComplete={handleAnimationComplete}
            />
          </div>
          <Button
            size="sm"
            variant="ghost"
            className="border border-primary/10 rounded-full ml-auto py-5"
            onClick={() => setValue(value + 20)}
          >
            <Plus className="h-4 w-4" />
          </Button>
        </div>
      </TextureCardContent>
    </TextureCardStyled>
  )
}

function CustomSpringExample() {
  const [value, setValue] = useState(1000)
  const [mass, setMass] = useState(1)
  const [stiffness, setStiffness] = useState(100)
  const [damping, setDamping] = useState(40)

  const handleValueChange =
    (setter: (value: number) => void, minValue: number) =>
    (values: number[]) => {
      const newValue = Math.max(values[0], minValue)
      setter(newValue)
    }

  return (
    <TextureCardStyled className="w-full">
      <TextureCardHeader className="px-3">
        <GradientHeading size="sm">Custom Spring Properties</GradientHeading>
      </TextureCardHeader>
      <TextureCardContent className="flex flex-col sm:flex-row justify-between items-center gap-8">
        <div
          className="text-6xl font-bold mr-auto flex"
          style={{ minWidth: "150px", textAlign: "right" }}
        >
          <AnimatedNumber
            value={value}
            mass={mass}
            stiffness={stiffness}
            damping={damping}
          />
        </div>

        <div className="flex flex-col gap-3 px-2">
          <Button
            className="border border-primary/10 rounded-full py-5"
            onClick={() => setValue(value + 500)}
          >
            <Plus className="h-4 w-4 mr-2" />
            Increase
          </Button>
          <Button
            className="border border-primary/10 rounded-full py-5"
            disabled={value <= 500}
            onClick={() => setValue(value - 300)}
          >
            <Minus className="h-4 w-4 mr-2" />
            Decrease
          </Button>
        </div>
        <div className="ml-auto w-full">
          <div className="flex flex-col gap-4">
            <div className="ml-auto w-full">
              <label>Mass: {mass}</label>
              <Slider
                defaultValue={[mass]}
                max={5}
                step={0.1}
                onValueChange={handleValueChange(setMass, 0.1)}
              />
            </div>
            <div className="ml-auto w-full">
              <label>Stiffness: {stiffness}</label>
              <Slider
                defaultValue={[stiffness]}
                max={200}
                step={1}
                onValueChange={handleValueChange(setStiffness, 1)}
              />
            </div>
            <div className="ml-auto w-full">
              <label>Damping: {damping}</label>
              <Slider
                defaultValue={[damping]}
                max={50}
                step={1}
                onValueChange={handleValueChange(setDamping, 1)}
              />
            </div>
          </div>
        </div>
      </TextureCardContent>
    </TextureCardStyled>
  )
}

export default function AnimatedNumberExamples() {
  return (
    <div className="  max-w-xl gap-4 py-6 mx-auto ">
      <div className="w-full flex flex-col gap-2 justify-between">
        <CustomSpringExample />
        <div className="flex   flex-col sm:flex-row gap-2">
          <PrecisionExample />
          <FormatExample />
          <HooksExample />
        </div>
      </div>
    </div>
  )
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
