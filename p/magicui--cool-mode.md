<!-- Cool Mode · Magic UI · https://magicui.design/docs/components/cool-mode
     license: MIT · category: button
     Cool mode effect for buttons, links, and other DOMs -->

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
components/ui/cool-mode.tsx
"use client"

import React, { useEffect, useRef, type ReactNode } from "react"

export interface BaseParticle {
  element: HTMLElement | SVGSVGElement
  left: number
  size: number
  top: number
}

export interface BaseParticleOptions {
  particle?: string
  size?: number
}

export interface CoolParticle extends BaseParticle {
  direction: number
  speedHorz: number
  speedUp: number
  spinSpeed: number
  spinVal: number
}

export interface CoolParticleOptions extends BaseParticleOptions {
  particleCount?: number
  speedHorz?: number
  speedUp?: number
}

const SVG_NS = "http://www.w3.org/2000/svg"

const getContainer = () => {
  const id = "_coolMode_effect"
  const existingContainer = document.getElementById(id)

  if (existingContainer) {
    return existingContainer
  }

  const container = document.createElement("div")
  container.setAttribute("id", id)
  container.setAttribute(
    "style",
    "overflow:hidden; position:fixed; height:100%; top:0; left:0; right:0; bottom:0; pointer-events:none; z-index:2147483647"
  )

  document.body.appendChild(container)

  return container
}

let instanceCounter = 0

const applyParticleEffect = (
  element: HTMLElement,
  options?: CoolParticleOptions
): (() => void) => {
  instanceCounter++

  const defaultParticle = "circle"
  const particleType = options?.particle || defaultParticle
  const sizes = [15, 20, 25, 35, 45]
  const limit = 45

  let particles: CoolParticle[] = []
  let autoAddParticle = false
  let mouseX = 0
  let mouseY = 0

  const container = getContainer()

  const appendCircleParticle = (particle: HTMLDivElement, size: number) => {
    const circleSVG = document.createElementNS(SVG_NS, "svg")
    const circle = document.createElementNS(SVG_NS, "circle")

    circle.setAttributeNS(null, "cx", (size / 2).toString())
    circle.setAttributeNS(null, "cy", (size / 2).toString())
    circle.setAttributeNS(null, "r", (size / 2).toString())
    circle.setAttributeNS(null, "fill", `hsl(${Math.random() * 360}, 70%, 50%)`)

    circleSVG.appendChild(circle)
    circleSVG.setAttribute("width", size.toString())
    circleSVG.setAttribute("height", size.toString())

    particle.appendChild(circleSVG)
  }

  const appendImageParticle = (
    particle: HTMLDivElement,
    imageSrc: string,
    size: number
  ) => {
    const image = document.createElement("img")
    image.src = imageSrc
    image.width = size
    image.height = size
    image.alt = ""
    image.style.borderRadius = "50%"

    particle.appendChild(image)
  }

  const appendTextParticle = (
    particle: HTMLDivElement,
    particleContent: string,
    size: number
  ) => {
    const fontSizeMultiplier = 3
    const emojiSize = size * fontSizeMultiplier
    const content = document.createElement("div")

    content.textContent = particleContent
    content.style.fontSize = `${emojiSize}px`
    content.style.lineHeight = "1"
    content.style.textAlign = "center"
    content.style.width = `${size}px`
    content.style.height = `${size}px`
    content.style.display = "flex"
    content.style.alignItems = "center"
    content.style.justifyContent = "center"
    content.style.transform = `scale(${fontSizeMultiplier})`
    content.style.transformOrigin = "center"

    particle.appendChild(content)
  }

  function generateParticle() {
    const size =
      options?.size || sizes[Math.floor(Math.random() * sizes.length)]
    const speedHorz = options?.speedHorz || Math.random() * 10
    const speedUp = options?.speedUp || Math.random() * 25
    const spinVal = Math.random() * 360
    const spinSpeed = Math.random() * 35 * (Math.random() <= 0.5 ? -1 : 1)
    const top = mouseY - size / 2
    const left = mouseX - size / 2
    const direction = Math.random() <= 0.5 ? -1 : 1

    const particle = document.createElement("div")

    if (particleType === "circle") {
      appendCircleParticle(particle, size)
    } else if (
      particleType.startsWith("http") ||
      particleType.startsWith("/")
    ) {
      appendImageParticle(particle, particleType, size)
    } else {
      appendTextParticle(particle, particleType, size)
    }

    particle.style.position = "absolute"
    particle.style.transform = `translate3d(${left}px, ${top}px, 0px) rotate(${spinVal}deg)`

    container.appendChild(particle)

    particles.push({
      direction,
      element: particle,
      left,
      size,
      speedHorz,
      speedUp,
      spinSpeed,
      spinVal,
      top,
    })
  }

  function refreshParticles() {
    particles.forEach((p) => {
      p.left = p.left - p.speedHorz * p.direction
      p.top = p.top - p.speedUp
      p.speedUp = Math.min(p.size, p.speedUp - 1)
      p.spinVal = p.spinVal + p.spinSpeed

      if (
        p.top >=
        Math.max(window.innerHeight, document.body.clientHeight) + p.size
      ) {
        particles = particles.filter((o) => o !== p)
        p.element.remove()
      }

      p.element.setAttribute(
        "style",
        [
          "position:absolute",
          "will-change:transform",
          `top:${p.top}px`,
          `left:${p.left}px`,
          `transform:rotate(${p.spinVal}deg)`,
        ].join(";")
      )
    })
  }

  let animationFrame: number | undefined

  let lastParticleTimestamp = 0
  const particleGenerationDelay = 30

  function loop() {
    const currentTime = performance.now()
    if (
      autoAddParticle &&
      particles.length < limit &&
      currentTime - lastParticleTimestamp > particleGenerationDelay
    ) {
      generateParticle()
      lastParticleTimestamp = currentTime
    }

    refreshParticles()
    animationFrame = requestAnimationFrame(loop)
  }

  loop()

  const isTouchInteraction = "ontouchstart" in window

  const tap = isTouchInteraction ? "touchstart" : "mousedown"
  const tapEnd = isTouchInteraction ? "touchend" : "mouseup"
  const move = isTouchInteraction ? "touchmove" : "mousemove"

  const updateMousePosition = (e: MouseEvent | TouchEvent) => {
    if ("touches" in e) {
      mouseX = e.touches?.[0].clientX
      mouseY = e.touches?.[0].clientY
    } else {
      mouseX = e.clientX
      mouseY = e.clientY
    }
  }

  const tapHandler = (e: MouseEvent | TouchEvent) => {
    updateMousePosition(e)
    autoAddParticle = true
  }

  const disableAutoAddParticle = () => {
    autoAddParticle = false
  }

  element.addEventListener(move, updateMousePosition, { passive: true })
  element.addEventListener(tap, tapHandler, { passive: true })
  element.addEventListener(tapEnd, disableAutoAddParticle, { passive: true })
  element.addEventListener("mouseleave", disableAutoAddParticle, {
    passive: true,
  })

  return () => {
    element.removeEventListener(move, updateMousePosition)
    element.removeEventListener(tap, tapHandler)
    element.removeEventListener(tapEnd, disableAutoAddParticle)
    element.removeEventListener("mouseleave", disableAutoAddParticle)

    const interval = setInterval(() => {
      if (animationFrame && particles.length === 0) {
        cancelAnimationFrame(animationFrame)
        clearInterval(interval)

        if (--instanceCounter === 0) {
          container.remove()
        }
      }
    }, 500)
  }
}

interface CoolModeProps {
  children: ReactNode
  options?: CoolParticleOptions
}

export const CoolMode: React.FC<CoolModeProps> = ({ children, options }) => {
  const ref = useRef<HTMLSpanElement>(null)

  useEffect(() => {
    const element = ref.current
    let cleanup: (() => void) | null = null

    if (element) {
      cleanup = applyParticleEffect(element, options)
    }

    return () => {
      if (cleanup) {
        cleanup()
      }
    }
  }, [options])

  return <span ref={ref}>{children}</span>
}

demo.tsx
import { Button } from "@/components/ui/button"
import { CoolMode } from "@/registry/magicui/cool-mode"

export default function CoolModeDemo() {
  return (
    <div className="relative justify-center">
      <CoolMode>
        <Button>Click Me!</Button>
      </CoolMode>
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
