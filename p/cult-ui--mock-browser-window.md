<!-- Mock Browser Window · cult-ui · https://www.cult-ui.com/docs/components/mock-browser-window
     license: MIT · category: navigation-menu
     A customizable browser window mockup component with support for Chrome, Safari, and generic styles, customizable sidebars, and themes -->

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
components/ui/mock-browser-window.tsx
import type React from "react"

import { cn } from "@/lib/utils"

interface SidebarItem {
  icon?: React.ReactNode
  label: string
  active?: boolean
  badge?: string | number
}

interface WindowControlsProps {
  variant?: "macos" | "windows" | "chrome" | "safari"
  headerStyle?: "minimal" | "full"
}

interface AddressBarProps {
  url?: string
  secure?: boolean
  variant?: "chrome" | "safari"
  className?: string
}

interface SidebarContentProps {
  items?: SidebarItem[]
  variant?: "navigation" | "bookmarks" | "history" | "extensions"
  className?: string
}

interface BrowserWindowProps {
  children?: React.ReactNode
  className?: string
  size?: "sm" | "md" | "lg" | "xl"
  showSidebar?: boolean
  sidebarPosition?: "left" | "right" | "top" | "bottom"
  headerStyle?: "minimal" | "full"
  variant?: "chrome" | "safari" | "generic"
  theme?: "light" | "dark" | "auto"
  url?: string
  sidebarItems?: Array<{
    icon?: React.ReactNode
    label: string
    active?: boolean
    badge?: string | number
  }>
}

function WindowControls({
  variant = "macos",
  headerStyle = "full",
}: WindowControlsProps) {
  const sizeClasses = "size-2"

  if (variant === "macos" || variant === "safari") {
    const dotColors =
      headerStyle === "minimal"
        ? {
            red: "bg-muted border  border-foreground/20",
            yellow: "bg-muted border border-foreground/20",
            green: "bg-muted border border-foreground/20",
          }
        : {
            red: "bg-red-500 hover:bg-red-600 border border-foreground/20",
            yellow:
              "bg-yellow-500 hover:bg-yellow-600 border border-foreground/20",
            green:
              "bg-green-500 hover:bg-green-600 border border-foreground/20 ",
          }

    return (
      <div className="flex gap-2">
        <div
          className={cn(
            sizeClasses,
            "rounded-full",
            dotColors.red,
            "transition-colors cursor-pointer flex items-center justify-center group"
          )}
        >
          {headerStyle !== "minimal" && (
            <div className="w-1.5 h-0.5 bg-red-900/60 opacity-0 group-hover:opacity-100 transition-opacity"></div>
          )}
        </div>
        <div
          className={cn(
            sizeClasses,
            "rounded-full",
            dotColors.yellow,
            "transition-colors cursor-pointer flex items-center justify-center group"
          )}
        >
          {headerStyle !== "minimal" && (
            <div className="w-1.5 h-0.5 bg-yellow-900/60 opacity-0 group-hover:opacity-100 transition-opacity"></div>
          )}
        </div>
        <div
          className={cn(
            sizeClasses,
            "rounded-full",
            dotColors.green,
            "transition-colors cursor-pointer flex items-center justify-center group"
          )}
        >
          {headerStyle !== "minimal" && (
            <div className="w-1 h-1 border border-green-900/60 opacity-0 group-hover:opacity-100 transition-opacity"></div>
          )}
        </div>
      </div>
    )
  }

  if (variant === "windows") {
    return (
      <div className="flex gap-1">
        <div className="w-6 h-4 bg-muted/50 hover:bg-muted transition-colors cursor-pointer flex items-center justify-center">
          <div className="w-2 h-0.5 bg-foreground/60"></div>
        </div>
        <div className="w-6 h-4 bg-muted/50 hover:bg-muted transition-colors cursor-pointer flex items-center justify-center">
          <div className="w-2 h-2 border border-foreground/60"></div>
        </div>
        <div className="w-6 h-4 bg-red-500/80 hover:bg-red-500 transition-colors cursor-pointer flex items-center justify-center">
          <div className="w-2 h-0.5 bg-white rotate-45"></div>
          <div className="w-2 h-0.5 bg-white -rotate-45 absolute"></div>
        </div>
      </div>
    )
  }

  if (variant === "chrome") {
    return (
      <div className="flex gap-1.5">
        <div
          className={cn(
            sizeClasses,
            "rounded-full bg-red-500 hover:bg-red-600 transition-colors cursor-pointer"
          )}
        ></div>
        <div
          className={cn(
            sizeClasses,
            "rounded-full bg-yellow-500 hover:bg-yellow-600 transition-colors cursor-pointer"
          )}
        ></div>
        <div
          className={cn(
            sizeClasses,
            "rounded-full bg-green-500 hover:bg-green-600 transition-colors cursor-pointer"
          )}
        ></div>
      </div>
    )
  }

  return (
    <div className="flex gap-1.5">
      <div
        className={`${sizeClasses} rounded-full border border-foreground/20 bg-foreground/10`}
      ></div>
      <div
        className={`${sizeClasses} rounded-full border border-foreground/20 bg-foreground/10`}
      ></div>
      <div
        className={`${sizeClasses} rounded-full border border-foreground/20 bg-foreground/10`}
      ></div>
    </div>
  )
}

function AddressBar({
  url = "https://example.com",
  secure = true,
  variant = "chrome",
  className = "",
}: AddressBarProps) {
  const variantStyles = {
    chrome:
      "bg-muted/30 rounded-full border border-foreground/5 shadow-[0px_1px_2px_0px_rgba(0,0,0,0.03)_inset] backdrop-blur-sm",
    safari:
      "bg-muted/20 rounded-lg border border-foreground/5 shadow-[0px_1px_2px_0px_rgba(0,0,0,0.03)_inset] backdrop-blur-sm",
  }

  const iconColors = {
    chrome: "text-muted-foreground/60",
    safari: "text-muted-foreground/60",
  }

  return (
    <div className={`flex-1 flex justify-center ${className}`}>
      <div
        className={`${variantStyles[variant]} px-4 py-2 text-xs text-muted-foreground/70 min-w-[200px] max-w-md flex items-center gap-2 transition-colors`}
      >
        {secure && (
          <div className={`w-3 h-3 ${iconColors[variant]}`}>
            <svg viewBox="0 0 12 12" fill="currentColor">
              <title>Secure</title>
              <path d="M6 1a2.5 2.5 0 0 1 2.5 2.5V5h.5a1 1 0 0 1 1 1v4a1 1 0 0 1-1 1H3a1 1 0 0 1-1-1V6a1 1 0 0 1 1-1h.5V3.5A2.5 2.5 0 0 1 6 1z" />
            </svg>
          </div>
        )}
        <span className="truncate">{url}</span>
      </div>
    </div>
  )
}

function SidebarContent({
  items = [
    { label: "Dashboard", active: true },
    { label: "Analytics", badge: "3" },
    { label: "Settings" },
    { label: "Profile" },
  ],
  className = "",
}: SidebarContentProps) {
  return (
    <div className={`p-3 space-y-1 ${className}`}>
      {items.map((item, index) => (
        <div
          key={`${item.label}-${index}`}
          className={`
            flex items-center gap-2 px-2 py-1.5 rounded text-sm transition-colors cursor-pointer
            ${
              item.active
                ? "bg-primary/5 text-primary border border-primary/5"
                : "text-muted-foreground hover:text-foreground hover:bg-muted/20"
            }
          `}
        >
          {item.icon && (
            <div className="w-4 h-4 flex-shrink-0">{item.icon}</div>
          )}
          <span className="flex-1 truncate">{item.label}</span>
          {item.badge && (
            <div className="bg-primary/5 text-primary text-xs px-1.5 py-0.5 rounded-full min-w-[16px] text-center">
              {item.badge}
            </div>
          )}
        </div>
      ))}
    </div>
  )
}

export function BrowserWindow({
  children,
  className = "",
  size = "md",
  showSidebar = false,
  sidebarPosition = "left",
  headerStyle = "minimal",
  variant = "generic",
  theme = "auto",
  url,
  sidebarItems,
}: BrowserWindowProps) {
  const sizeClasses = {
    sm: "h-64 max-w-sm",
    md: "h-80 max-w-2xl",
    lg: "h-96 max-w-4xl",
    xl: "h-[32rem] max-w-6xl",
  }

  const sidebarSizes = {
    sm: "w-32",
    md: "w-48",
    lg: "w-56",
    xl: "w-64",
  }

  const themeClasses =
    theme === "dark"
      ? "bg-background border-border"
      : theme === "light"
        ? "bg-background border-border"
        : "bg-background border-border"

  const getHeaderStyles = () => {
    const baseStyles =
      "h-11 border-b border-foreground/5 flex items-center px-4"

    if (variant === "chrome") {
      return `${baseStyles} bg-muted/10 overflow-hidden`
    }

    if (variant === "safari") {
      return `${baseStyles} bg-muted/10 overflow-hidden border-b border-border/30`
    }

    return `${baseStyles} bg-muted/20`
  }

  return (
    <div
      className={`
        relative mask-b-from-50% rounded-2xl border shadow-[0px_1px_1px_0px_rgba(0,_0,_0,_0.05),_0px_1px_1px_0px_rgba(255,_252,_240,_0.5)_inset,_0px_0px_0px_1px_hsla(0,_0%,_100%,_0.1)_inset,_0px_0px_1px_0px_rgba(28,_27,_26,_0.5)] dark:shadow-[0px_1px_1px_0px_rgba(0,_0,_0,_0.2),_0px_1px_1px_0px_rgba(0,_0,_0,_0.3)_inset,_0px_0px_0px_1px_hsla(0,_0%,_0%,_0.2)_inset,_0px_0px_1px_0px_rgba(255,_255,_255,_0.1)]
        ${sizeClasses[size]} ${themeClasses} ${className} flex flex-col
      `}
    >
      <div className={getHeaderStyles()}>
        <WindowControls
          variant={variant === "generic" ? "macos" : variant}
          headerStyle={headerStyle}
        />

        {headerStyle === "full" && (
          <AddressBar
            url={url}
            variant={variant === "generic" ? "chrome" : variant}
            className="ml-4"
          />
        )}
      </div>

      {showSidebar && sidebarPosition === "top" && (
        <div className="border-b border-foreground/5 bg-muted/20 h-16">
          <SidebarContent
            items={sidebarItems}
            variant="navigation"
            className="flex-row"
          />
        </div>
      )}

      <div className="flex flex-1 h-0">
        {/* Left Sidebar */}
        {showSidebar && sidebarPosition === "left" && (
          <div
            className={`border-r border-foreground/5 bg-muted/20 ${sidebarSizes[size]} flex-shrink-0 h-full`}
          >
            <SidebarContent items={sidebarItems} />
          </div>
        )}

        {/* Main Content Area */}
        <div className="flex-1 relative min-w-0 h-full">
          {children || <div className="absolute inset-0">{children}</div>}
        </div>

        {/* Right Sidebar */}
        {showSidebar && sidebarPosition === "right" && (
          <div
            className={`border-l border-foreground/5 bg-muted/20 ${sidebarSizes[size]} flex-shrink-0 h-full`}
          >
            <SidebarContent items={sidebarItems} />
          </div>
        )}
      </div>

      {/* Bottom Sidebar */}
      {showSidebar && sidebarPosition === "bottom" && (
        <div className="border-t border-foreground/5 bg-muted/20 h-16">
          <SidebarContent
            items={sidebarItems}
            variant="navigation"
            className="flex-row"
          />
        </div>
      )}
    </div>
  )
}

demo.tsx
"use client"

import { useState } from "react"

import { Button } from "@/components/ui/button"
import { Input } from "@/components/ui/input"
import { Label } from "@/components/ui/label"
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select"
import { Switch } from "@/components/ui/switch"
import { BrowserWindow } from "@/registry/default/ui/mock-browser-window"

export default function DemoPage() {
  const [config, setConfig] = useState({
    variant: "chrome" as "chrome" | "safari",
    headerStyle: "full" as "minimal" | "full",
    size: "lg" as "sm" | "md" | "lg" | "xl",
    showSidebar: true,
    sidebarPosition: "left" as "left" | "right" | "top" | "bottom",
    url: "https://example.com/dashboard",
  })

  const [sidebarItems] = useState([
    { label: "Overview", active: true },
    { label: "Users", badge: "12" },
    { label: "Analytics", badge: "new" },
    { label: "Settings" },
  ])

  return (
    <div className="min-h-screen ">
      <main className="container ">
        <div className="mx-auto max-w-7xl">
          <div className="space-y-12">
            <div className="space-y-8">
              <div>
                <h2 className="text-lg font-semibold mb-6">Preview</h2>
                <div className="flex justify-center p-8 bg-muted/30 rounded-xl border">
                  <BrowserWindow
                    variant={config.variant}
                    headerStyle={config.headerStyle}
                    size={config.size}
                    showSidebar={config.showSidebar}
                    sidebarPosition={config.sidebarPosition}
                    url={config.url}
                    sidebarItems={config.showSidebar ? sidebarItems : undefined}
                    className="w-full max-w-4xl"
                  />
                </div>
              </div>
            </div>

            <div className="space-y-8">
              <h2 className="text-lg font-semibold">Window Settings</h2>
              <div className="grid gap-6 md:grid-cols-2 lg:grid-cols-3">
                <div className="space-y-2">
                  <Label className="text-sm">Browser</Label>
                  <Select
                    value={config.variant}
                    onValueChange={(value: "chrome" | "safari") =>
                      setConfig((prev) => ({ ...prev, variant: value }))
                    }
                  >
                    <SelectTrigger>
                      <SelectValue />
                    </SelectTrigger>
                    <SelectContent>
                      <SelectItem value="chrome">Chrome</SelectItem>
                      <SelectItem value="safari">Safari</SelectItem>
                    </SelectContent>
                  </Select>
                </div>

                <div className="space-y-2">
                  <Label className="text-sm">Header Style</Label>
                  <Select
                    value={config.headerStyle}
                    onValueChange={(value: "minimal" | "full") =>
                      setConfig((prev) => ({ ...prev, headerStyle: value }))
                    }
                  >
                    <SelectTrigger>
                      <SelectValue />
                    </SelectTrigger>
                    <SelectContent>
                      <SelectItem value="minimal">Minimal</SelectItem>
                      <SelectItem value="full">Address Bar</SelectItem>
                    </SelectContent>
                  </Select>
                </div>

                <div className="space-y-2">
                  <Label className="text-sm">Size</Label>
                  <Select
                    value={config.size}
                    onValueChange={(value: "sm" | "md" | "lg" | "xl") =>
                      setConfig((prev) => ({ ...prev, size: value }))
                    }
                  >
                    <SelectTrigger>
                      <SelectValue />
                    </SelectTrigger>
                    <SelectContent>
                      <SelectItem value="sm">Small</SelectItem>
                      <SelectItem value="md">Medium</SelectItem>
                      <SelectItem value="lg">Large</SelectItem>
                      <SelectItem value="xl">Extra Large</SelectItem>
                    </SelectContent>
                  </Select>
                </div>

                <div className="space-y-2 md:col-span-2 lg:col-span-1">
                  <Label className="text-sm">URL</Label>
                  <Input
                    value={config.url}
                    onChange={(e) =>
                      setConfig((prev) => ({
                        ...prev,
                        url: e.target.value,
                      }))
                    }
                    placeholder="https://example.com"
                  />
                </div>

                <div className="space-y-2">
                  <div className="flex items-center justify-between">
                    <Label className="text-sm">Show Sidebar</Label>
                    <Switch
                      checked={config.showSidebar}
                      onCheckedChange={(checked) =>
                        setConfig((prev) => ({
                          ...prev,
                          showSidebar: checked,
                        }))
                      }
                    />
                  </div>
                </div>

                {config.showSidebar && (
                  <div className="space-y-2">
                    <Label className="text-sm">Sidebar Position</Label>
                    <Select
                      value={config.sidebarPosition}
                      onValueChange={(
                        value: "left" | "right" | "top" | "bottom"
                      ) =>
                        setConfig((prev) => ({
                          ...prev,
                          sidebarPosition: value,
                        }))
                      }
                    >
                      <SelectTrigger>
                        <SelectValue />
                      </SelectTrigger>
                      <SelectContent>
                        <SelectItem value="left">Left</SelectItem>
                        <SelectItem value="right">Right</SelectItem>
                      </SelectContent>
                    </Select>
                  </div>
                )}

                <div className="space-y-2 md:col-span-2 lg:col-span-1">
                  <Label className="text-sm">Quick Presets</Label>
                  <div className="flex gap-2">
                    <Button
                      variant="outline"
                      size="sm"
                      className="flex-1 bg-transparent"
                      onClick={() =>
                        setConfig((prev) => ({
                          ...prev,
                          variant: "chrome",
                          headerStyle: "full",
                          showSidebar: true,
                          size: "lg",
                        }))
                      }
                    >
                      Chrome Dashboard
                    </Button>
                    <Button
                      variant="outline"
                      size="sm"
                      className="flex-1 bg-transparent"
                      onClick={() =>
                        setConfig((prev) => ({
                          ...prev,
                          variant: "safari",
                          headerStyle: "minimal",
                          showSidebar: false,
                          size: "md",
                        }))
                      }
                    >
                      Safari Minimal
                    </Button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </main>
    </div>
  )
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add texture-overlay
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
