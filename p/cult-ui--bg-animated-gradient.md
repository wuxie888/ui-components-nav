<!-- Bg Animated Gradient · cult-ui · https://www.cult-ui.com/docs/components/bg-animated-gradient
     license: MIT · category: background
     Animated gradient background component with customizable color transitions -->

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
components/ui/bg-animated-gradient.tsx
"use client"

import React, { useEffect } from "react"
import { motion, useAnimation } from "motion/react"

interface GradientStop {
  color: string
  position: number
}

interface GradientType {
  stops: GradientStop[]
  centerX: number
  centerY: number
}

interface GradientAnimationProps {
  gradients: GradientType[]
  animationDuration: number
  className?: string
}

export const GradientAnimation: React.FC<GradientAnimationProps> = ({
  gradients,
  animationDuration,
  className = "",
}) => {
  const controls = useAnimation()

  useEffect(() => {
    controls.start({
      background: gradients.map(
        (g) =>
          `radial-gradient(circle at ${g.centerX}% ${g.centerY}%, ${g.stops
            .map((s) => `${s.color} ${s.position}%`)
            .join(", ")})`
      ),
      transition: {
        duration: animationDuration,
        repeat: Infinity,
        repeatType: "reverse",
        ease: "linear",
      },
    })
  }, [controls, gradients, animationDuration])

  return (
    <motion.div
      className={`absolute inset-0 h-full w-full ${className}`}
      animate={controls}
    />
  )
}

export default React.memo(GradientAnimation)

demo.tsx
"use client"

import React, { useState } from "react"
import { Check, Copy, Minus, Plus, Trash2 } from "lucide-react"

import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion"
import { Button } from "@/components/ui/button"
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"
import { Input } from "@/components/ui/input"
import { Label } from "@/components/ui/label"
import { Slider } from "@/components/ui/slider"
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs"
import { useToast } from "@/components/ui/use-toast"

import { GradientAnimation } from "../ui/bg-animated-gradient"

type GradientStop = {
  color: string
  position: number
}

type Gradient = {
  stops: GradientStop[]
  centerX: number
  centerY: number
}

const initialConfig = {
  animationDuration: 5,
  gradients: [
    {
      stops: [
        { color: "#3498DB", position: 0 },
        { color: "#2980B9", position: 25 },
        { color: "#1ABC9C", position: 50 },
        { color: "transparent", position: 75 },
      ],
      centerX: 30,
      centerY: 70,
    },
    {
      stops: [
        { color: "#16A085", position: 0 },
        { color: "#2980B9", position: 25 },
        { color: "#3498DB", position: 50 },
        { color: "transparent", position: 75 },
      ],
      centerX: 70,
      centerY: 30,
    },
  ],
}

export function ConfigurableGradientAnimation() {
  const [config, setConfig] = useState(initialConfig)
  const [isCopied, setIsCopied] = useState(false)
  const { toast } = useToast()

  const updateConfig = (key: string, value: any) => {
    setConfig((prev) => ({ ...prev, [key]: value }))
  }

  const updateGradient = (index: number, updatedGradient: Gradient) => {
    const newGradients = [...config.gradients]
    newGradients[index] = updatedGradient
    updateConfig("gradients", newGradients)
  }

  const addGradient = () => {
    const newGradient: Gradient = {
      stops: [
        { color: "#000000", position: 0 },
        { color: "#FFFFFF", position: 100 },
      ],
      centerX: 50,
      centerY: 50,
    }
    updateConfig("gradients", [...config.gradients, newGradient])
  }

  const removeGradient = (index: number) => {
    const newGradients = config.gradients.filter((_, i) => i !== index)
    updateConfig("gradients", newGradients)
  }

  const copyConfigToClipboard = () => {
    const configString = `
"use client"

import { GradientAnimation } from "./gradient-animation"

export function GradientAnimationExample() {
  return (
    <div className="h-screen w-screen relative">
      <GradientAnimation
        animationDuration={${config.animationDuration}}
        gradients={${JSON.stringify(config.gradients, null, 2)}}
      />
    </div>
  )
}
`

    navigator.clipboard.writeText(configString).then(() => {
      setIsCopied(true)
      toast({
        title: "Configuration Copied",
        description:
          "The current configuration has been copied to your clipboard.",
      })
      setTimeout(() => setIsCopied(false), 2000)
    })
  }

  return (
    <div className="container mx-auto py-8">
      <h1 className="text-3xl font-bold mb-6 text-center">
        Configurable GradientAnimation
      </h1>
      <div className="grid grid-cols-1 lg:grid-cols-1 gap-8">
        <Button
          variant="outline"
          size="sm"
          onClick={copyConfigToClipboard}
          className="ml-2"
        >
          {isCopied ? (
            <Check className="h-4 w-4 mr-2" />
          ) : (
            <Copy className="h-4 w-4 mr-2" />
          )}
          {isCopied ? "Copied!" : "Copy Config"}
        </Button>
        <div className="overflow-hidden bg-white rounded-lg">
          <div className="h-[500px] w-full relative">
            <GradientAnimation {...config} />
          </div>
        </div>
        <Card className="overflow-hidden bg-gradient-to-b from-primary/5 to-primary/5">
          <CardHeader>
            <CardTitle>Configuration</CardTitle>
          </CardHeader>
          <CardContent>
            <Tabs defaultValue="general">
              <TabsList className="grid w-full grid-cols-2">
                <TabsTrigger value="general">General</TabsTrigger>
                <TabsTrigger value="gradients">Gradients</TabsTrigger>
              </TabsList>
              <TabsContent value="general">
                <Accordion type="single" collapsible className="w-full">
                  <AccordionItem value="animation">
                    <AccordionTrigger>Animation</AccordionTrigger>
                    <AccordionContent>
                      <div className="space-y-4">
                        <div>
                          <Label htmlFor="animationDuration">
                            Animation Duration
                          </Label>
                          <Slider
                            id="animationDuration"
                            min={1}
                            max={30}
                            step={1}
                            value={[config.animationDuration]}
                            onValueChange={([value]) =>
                              updateConfig("animationDuration", value)
                            }
                          />
                        </div>
                      </div>
                    </AccordionContent>
                  </AccordionItem>
                </Accordion>
              </TabsContent>
              <TabsContent value="gradients">
                <div className="space-y-4">
                  {config.gradients.map((gradient, index) => (
                    <Card
                      key={index}
                      className="bg-gradient-to-b from-primary/5 to-primary/10"
                    >
                      <CardHeader>
                        <CardTitle className="text-lg">
                          Gradient {index + 1}
                        </CardTitle>
                      </CardHeader>
                      <CardContent>
                        <div className="space-y-4">
                          <div className="grid grid-cols-2 gap-4">
                            <div>
                              <Label htmlFor={`centerX-${index}`}>
                                Center X
                              </Label>
                              <Slider
                                id={`centerX-${index}`}
                                min={0}
                                max={100}
                                step={1}
                                value={[gradient.centerX]}
                                onValueChange={([value]) =>
                                  updateGradient(index, {
                                    ...gradient,
                                    centerX: value,
                                  })
                                }
                              />
                            </div>
                            <div>
                              <Label htmlFor={`centerY-${index}`}>
                                Center Y
                              </Label>
                              <Slider
                                id={`centerY-${index}`}
                                min={0}
                                max={100}
                                step={1}
                                value={[gradient.centerY]}
                                onValueChange={([value]) =>
                                  updateGradient(index, {
                                    ...gradient,
                                    centerY: value,
                                  })
                                }
                              />
                            </div>
                          </div>
                          {gradient.stops.map((stop, stopIndex) => (
                            <div
                              key={stopIndex}
                              className="flex items-center space-x-2"
                            >
                              <Input
                                type="color"
                                value={stop.color}
                                onChange={(e) => {
                                  const newStops = [...gradient.stops]
                                  newStops[stopIndex] = {
                                    ...stop,
                                    color: e.target.value,
                                  }
                                  updateGradient(index, {
                                    ...gradient,
                                    stops: newStops,
                                  })
                                }}
                                className="w-16 h-10"
                              />
                              <Slider
                                min={0}
                                max={100}
                                step={1}
                                value={[stop.position]}
                                onValueChange={([value]) => {
                                  const newStops = [...gradient.stops]
                                  newStops[stopIndex] = {
                                    ...stop,
                                    position: value,
                                  }
                                  updateGradient(index, {
                                    ...gradient,
                                    stops: newStops,
                                  })
                                }}
                                className="flex-grow"
                              />
                              <Button
                                variant="outline"
                                size="icon"
                                onClick={() => {
                                  const newStops = gradient.stops.filter(
                                    (_, i) => i !== stopIndex
                                  )
                                  updateGradient(index, {
                                    ...gradient,
                                    stops: newStops,
                                  })
                                }}
                              >
                                <Minus className="h-4 w-4" />
                              </Button>
                            </div>
                          ))}
                          <div className="flex justify-between">
                            <Button
                              variant="outline"
                              onClick={() => {
                                const newStops = [
                                  ...gradient.stops,
                                  { color: "#000000", position: 100 },
                                ]
                                updateGradient(index, {
                                  ...gradient,
                                  stops: newStops,
                                })
                              }}
                            >
                              <Plus className="h-4 w-4 mr-2" />
                              Add Stop
                            </Button>
                            <Button
                              variant="destructive"
                              onClick={() => removeGradient(index)}
                            >
                              <Trash2 className="h-4 w-4 mr-2" />
                              Remove Gradient
                            </Button>
                          </div>
                        </div>
                      </CardContent>
                    </Card>
                  ))}
                  <Button onClick={addGradient} className="w-full">
                    <Plus className="h-4 w-4 mr-2" />
                    Add Gradient
                  </Button>
                </div>
              </TabsContent>
            </Tabs>
          </CardContent>
        </Card>
      </div>
    </div>
  )
}

export default ConfigurableGradientAnimation
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
