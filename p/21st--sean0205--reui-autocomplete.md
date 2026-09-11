<!-- ReUI Autocomplete · @sean0205 · https://21st.dev/@sean0205/components/reui-autocomplete
     license: MIT · category: search
     An accessible autocomplete input from ReUI with filtering, labels, clear and trigger controls, grouped people, asynchronous search results, and compact sizing. -->

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
components/ui/autocomplete.tsx
"use client"

import { Autocomplete as AutocompletePrimitive } from "@base-ui/react/autocomplete"
import { cva, type VariantProps } from "class-variance-authority"

import { cn } from "@/lib/utils"
import { ScrollArea } from "@/components/ui/scroll-area"
import { IconPlaceholder } from "@/app/(create)/components/icon-placeholder"

const inputVariants = cva(
  "outline-none flex w-full text-foreground placeholder:text-muted-foreground disabled:pointer-events-none disabled:cursor-not-allowed disabled:opacity-50 [[readonly]]:bg-muted/80 [[readonly]]:cursor-not-allowed border border-input focus-visible:border-ring aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 aria-invalid:border-destructive dark:aria-invalid:border-destructive/50 rounded-lg bg-transparent dark:bg-input/30 text-sm transition-colors focus-visible:ring-ring/50 focus-visible:ring-3 aria-invalid:ring-3",
  {
    variants: {
      size: {
        sm: "h-7 px-2 [&~[data-slot=autocomplete-clear]]:end-1.5 [&~[data-slot=autocomplete-trigger]]:end-1.5",
        default:
          "h-8 px-2.5 [&~[data-slot=autocomplete-clear]]:end-1.75 [&~[data-slot=autocomplete-trigger]]:end-1.75",
        lg: "h-9 px-2.5 [&~[data-slot=autocomplete-clear]]:end-2 [&~[data-slot=autocomplete-trigger]]:end-2",
      },
    },
    defaultVariants: {
      size: "default",
    },
  }
)

const Autocomplete = AutocompletePrimitive.Root

function AutocompleteValue({ ...props }: AutocompletePrimitive.Value.Props) {
  return (
    <AutocompletePrimitive.Value data-slot="autocomplete-value" {...props} />
  )
}

function AutocompleteInput({
  className,
  size = "default",
  showClear = false,
  showTrigger = false,
  ...props
}: Omit<AutocompletePrimitive.Input.Props, "size"> &
  VariantProps<typeof inputVariants> & {
    showClear?: boolean
    showTrigger?: boolean
  }) {
  return (
    <div className="relative w-full">
      <AutocompletePrimitive.Input
        data-slot="autocomplete-input"
        data-size={size}
        className={cn(inputVariants({ size }), className)}
        {...props}
      />
      {showTrigger && <AutocompleteTrigger />}
      {showClear && <AutocompleteClear />}
    </div>
  )
}

function AutocompleteStatus({
  className,
  ...props
}: AutocompletePrimitive.Status.Props) {
  return (
    <AutocompletePrimitive.Status
      data-slot="autocomplete-status"
      className={cn(
        "text-muted-foreground px-2 py-1.5 text-sm empty:m-0 empty:p-0",
        className
      )}
      {...props}
    />
  )
}

function AutocompletePortal({ ...props }: AutocompletePrimitive.Portal.Props) {
  return (
    <AutocompletePrimitive.Portal data-slot="autocomplete-portal" {...props} />
  )
}

function AutocompleteBackdrop({
  ...props
}: AutocompletePrimitive.Backdrop.Props) {
  return (
    <AutocompletePrimitive.Backdrop
      data-slot="autocomplete-backdrop"
      {...props}
    />
  )
}

function AutocompletePositioner({
  className,
  ...props
}: AutocompletePrimitive.Positioner.Props) {
  return (
    <AutocompletePrimitive.Positioner
      data-slot="autocomplete-positioner"
      className={cn("z-50 outline-none", className)}
      {...props}
    />
  )
}

function AutocompleteList({
  className,
  scrollAreaClassName,
  ...props
}: AutocompletePrimitive.List.Props & {
  scrollAreaClassName?: string
  scrollFade?: boolean
  scrollbarGutter?: boolean
}) {
  return (
    <ScrollArea
      className={cn(
        "size-full min-h-0 **:data-[slot=scroll-area-viewport]:h-full **:data-[slot=scroll-area-viewport]:overscroll-contain",
        scrollAreaClassName
      )}
    >
      <AutocompletePrimitive.List
        data-slot="autocomplete-list"
        className={cn(
          "not-empty:px-1 not-empty:py-1 not-empty:scroll-py-1 in-data-has-overflow-y:me-3",
          className
        )}
        {...props}
      />
    </ScrollArea>
  )
}

function AutocompleteCollection({
  ...props
}: React.ComponentProps<typeof AutocompletePrimitive.Collection>) {
  return (
    <AutocompletePrimitive.Collection
      data-slot="autocomplete-collection"
      {...props}
    />
  )
}

function AutocompleteRow({
  className,
  ...props
}: React.ComponentProps<typeof AutocompletePrimitive.Row>) {
  return (
    <AutocompletePrimitive.Row
      data-slot="autocomplete-row"
      className={cn("flex items-center gap-2", className)}
      {...props}
    />
  )
}

function AutocompleteItem({
  className,
  ...props
}: React.ComponentProps<typeof AutocompletePrimitive.Item>) {
  return (
    <AutocompletePrimitive.Item
      data-slot="autocomplete-item"
      className={cn(
        "text-foreground data-highlighted:text-foreground data-highlighted:before:bg-accent gap-1.5",
        "rounded-md",
        "data-highlighted:before:rounded-md",
        "px-1.5 py-1 text-sm ([class*='size-'])]:size-4 ([class*='size-'])]:size-4 [&_svg:not([class*='size-'])]:size-4 ([class*='size-'])]:size-4 ([class*='size-'])]:size-3.5 ([class*='size-'])]:size-4 ([class*='size-'])]:size-3.5 relative flex cursor-default items-center outline-hidden transition-colors select-none data-disabled:pointer-events-none data-disabled:opacity-50 data-highlighted:relative data-highlighted:z-0 data-highlighted:before:absolute data-highlighted:before:inset-x-0 data-highlighted:before:inset-y-0 data-highlighted:before:z-[-1] [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([role=img]):not([class*=text-])]:opacity-60",
        className
      )}
      {...props}
    />
  )
}

export interface AutocompleteContentProps extends React.ComponentProps<
  typeof AutocompletePrimitive.Popup
> {
  align?: AutocompletePrimitive.Positioner.Props["align"]
  sideOffset?: AutocompletePrimitive.Positioner.Props["sideOffset"]
  alignOffset?: AutocompletePrimitive.Positioner.Props["alignOffset"]
  side?: AutocompletePrimitive.Positioner.Props["side"]
  anchor?: AutocompletePrimitive.Positioner.Props["anchor"]
  showBackdrop?: boolean
}

function AutocompleteContent({
  className,
  children,
  showBackdrop = false,
  align = "start",
  sideOffset = 4,
  alignOffset = 0,
  side = "bottom",
  anchor,
  ...props
}: AutocompleteContentProps) {
  return (
    <AutocompletePortal>
      {showBackdrop && <AutocompleteBackdrop />}
      <AutocompletePositioner
        align={align}
        sideOffset={sideOffset}
        alignOffset={alignOffset}
        side={side}
        anchor={anchor}
      >
        <div className="relative flex max-h-full">
          <AutocompletePrimitive.Popup
            data-slot="autocomplete-popup"
            className={cn(
              "bg-popover text-popover-foreground rounded-lg shadow-md ring-foreground/10 flex max-h-[min(var(--available-height),24rem)] w-(--anchor-width) max-w-(--available-width) origin-(--transform-origin) scroll-pt-2 scroll-pb-2 flex-col overscroll-contain py-0.5 ring-1 transition-[scale,opacity] has-data-starting-style:scale-98 has-data-starting-style:opacity-0 has-data-[side=none]:scale-100 has-data-[side=none]:transition-none",
              className
            )}
            {...props}
          >
            {children}
          </AutocompletePrimitive.Popup>
        </div>
      </AutocompletePositioner>
    </AutocompletePortal>
  )
}

function AutocompleteGroup({
  ...props
}: React.ComponentProps<typeof AutocompletePrimitive.Group>) {
  return (
    <AutocompletePrimitive.Group data-slot="autocomplete-group" {...props} />
  )
}

function AutocompleteGroupLabel({
  className,
  ...props
}: React.ComponentProps<typeof AutocompletePrimitive.GroupLabel>) {
  return (
    <AutocompletePrimitive.GroupLabel
      data-slot="autocomplete-group-label"
      className={cn(
        "text-muted-foreground px-1.5 py-1 text-xs font-medium",
        className
      )}
      {...props}
    />
  )
}

function AutocompleteEmpty({
  className,
  ...props
}: React.ComponentProps<typeof AutocompletePrimitive.Empty>) {
  return (
    <AutocompletePrimitive.Empty
      data-slot="autocomplete-empty"
      className={cn(
        "text-muted-foreground px-2 py-1.5 text-sm text-center empty:m-0 empty:p-0",
        className
      )}
      {...props}
    />
  )
}

function AutocompleteClear({
  className,
  ...props
}: React.ComponentProps<typeof AutocompletePrimitive.Clear>) {
  return (
    <AutocompletePrimitive.Clear
      data-slot="autocomplete-clear"
      className={cn(
        "ring-offset-background focus:ring-ring absolute top-1/2 -translate-y-1/2 cursor-pointer opacity-70 transition-opacity hover:opacity-100 focus:ring-2 focus:ring-offset-2 focus:outline-none disabled:pointer-events-none data-disabled:pointer-events-none",
        className
      )}
      {...props}
    >
      <IconPlaceholder
        lucide="XIcon"
        tabler="IconX"
        hugeicons="Cancel01Icon"
        phosphor="XIcon"
        remixicon="RiCloseLine"
        className="size-4"
      />
    </AutocompletePrimitive.Clear>
  )
}

function AutocompleteTrigger({
  className,
  ...props
}: React.ComponentProps<typeof AutocompletePrimitive.Trigger>) {
  return (
    <AutocompletePrimitive.Trigger
      data-slot="autocomplete-trigger"
      className={cn(
        "focus:ring-ring ring-offset-background absolute top-1/2 -translate-y-1/2 cursor-pointer focus:ring-2 focus:ring-offset-2 focus:outline-none disabled:pointer-events-none has-[+[data-slot=autocomplete-clear]]:hidden data-disabled:pointer-events-none",
        className
      )}
      {...props}
    >
      <IconPlaceholder
        lucide="ChevronsUpDownIcon"
        tabler="IconSelector"
        hugeicons="UnfoldMoreIcon"
        phosphor="CaretUpDownIcon"
        remixicon="RiExpandUpDownLine"
        className="size-4 opacity-70"
      />
    </AutocompletePrimitive.Trigger>
  )
}

function AutocompleteArrow({
  ...props
}: React.ComponentProps<typeof AutocompletePrimitive.Arrow>) {
  return (
    <AutocompletePrimitive.Arrow data-slot="autocomplete-arrow" {...props} />
  )
}

function AutocompleteSeparator({
  className,
  ...props
}: React.ComponentProps<typeof AutocompletePrimitive.Separator>) {
  return (
    <AutocompletePrimitive.Separator
      data-slot="autocomplete-separator"
      className={cn(
        "bg-border my-1.5 h-px",
        className
      )}
      {...props}
    />
  )
}

export {
  Autocomplete,
  AutocompleteValue,
  AutocompleteTrigger,
  AutocompleteInput,
  AutocompleteStatus,
  AutocompletePortal,
  AutocompleteBackdrop,
  AutocompletePositioner,
  AutocompleteContent,
  AutocompleteList,
  AutocompleteCollection,
  AutocompleteRow,
  AutocompleteItem,
  AutocompleteGroup,
  AutocompleteGroupLabel,
  AutocompleteEmpty,
  AutocompleteClear,
  AutocompleteArrow,
  AutocompleteSeparator,
}

demo.tsx
"use client"

import {
  type HTMLAttributes,
  type ImgHTMLAttributes,
  type ReactNode,
  useEffect,
  useState,
} from "react"
import { LoaderCircleIcon } from "lucide-react"
import {
  Autocomplete,
  AutocompleteContent,
  AutocompleteInput,
  AutocompleteItem,
  AutocompleteList,
  AutocompleteStatus,
} from "@/components/ui/reui-autocomplete"
import { Autocomplete as AutocompletePrimitive } from "@base-ui/react/autocomplete"

function Avatar({ className = "", ...props }: HTMLAttributes<HTMLDivElement>) {
  return (
    <div
      className={`relative flex shrink-0 overflow-hidden rounded-full bg-muted ${className}`}
      {...props}
    />
  )
}

function AvatarImage(props: ImgHTMLAttributes<HTMLImageElement>) {
  return <img className="absolute inset-0 size-full object-cover" {...props} />
}

function AvatarFallback({
  className = "",
  ...props
}: HTMLAttributes<HTMLDivElement>) {
  return (
    <div
      className={`flex size-full items-center justify-center text-xs font-medium ${className}`}
      {...props}
    />
  )
}

export default function Pattern() {
  const [searchValue, setSearchValue] = useState("")
  const [isLoading, setIsLoading] = useState(false)
  const [searchResults, setSearchResults] = useState<Developer[]>([])
  const [error, setError] = useState<string | null>(null)

  const { contains } = AutocompletePrimitive.useFilter({ sensitivity: "base" })

  useEffect(() => {
    if (!searchValue) {
      setSearchResults([])
      setIsLoading(false)
      return undefined
    }

    setIsLoading(true)
    setError(null)

    let ignore = false

    async function fetchDevelopers() {
      try {
        const results = await searchDevelopers(searchValue, contains)
        if (!ignore) {
          setSearchResults(results)
        }
      } catch {
        if (!ignore) {
          setError("Failed to fetch developers. Please try again.")
          setSearchResults([])
        }
      } finally {
        if (!ignore) {
          setIsLoading(false)
        }
      }
    }

    const timeoutId = setTimeout(fetchDevelopers, 300)

    return () => {
      clearTimeout(timeoutId)
      ignore = true
    }
  }, [searchValue, contains])

  let status: ReactNode = ""

  if (isLoading) {
    status = (
      <div className="flex items-center gap-2">
        <LoaderCircleIcon className="size-4 animate-spin" aria-hidden />
        Searching developers...
      </div>
    )
  } else if (error) {
    status = error
  } else if (searchResults.length === 0 && searchValue) {
    status = `No developers found for "${searchValue}"`
  } else if (searchResults.length > 0) {
    status = `${searchResults.length} developer${searchResults.length === 1 ? "" : "s"} found`
  } else if (!searchValue) {
    status = "Start typing to search developers..."
  }

  const shouldRenderPopup = searchValue !== ""

  return (
    <div className="w-full max-w-xs">
      <Autocomplete
        items={searchResults}
        value={searchValue}
        onValueChange={setSearchValue}
        itemToStringValue={(item: unknown) => (item as Developer).name}
        filter={null}
      >
        <AutocompleteInput
          placeholder="e.g. John Smith, React, San Francisco"
          showTrigger
          showClear
        />
        {shouldRenderPopup && (
          <AutocompleteContent>
            <AutocompleteStatus>{status}</AutocompleteStatus>
            <AutocompleteList>
              {(developer: Developer) => (
                <AutocompleteItem
                  key={developer.id}
                  value={developer}
                  className="rounded-lg"
                >
                  <div className="flex items-center gap-2.5 truncate">
                    <Avatar className="size-9">
                      <AvatarImage
                        src={developer.avatar}
                        alt={developer.name}
                      />
                      <AvatarFallback>
                        {developer.name
                          .split(" ")
                          .map((n) => n[0])
                          .join("")}
                      </AvatarFallback>
                    </Avatar>
                    <div className="min-w-0 flex-1">
                      <div className="truncate font-medium">
                        {developer.name}
                      </div>
                      <div className="text-muted-foreground truncate text-sm">
                        {developer.role} • {developer.location}
                      </div>
                    </div>
                  </div>
                </AutocompleteItem>
              )}
            </AutocompleteList>
          </AutocompleteContent>
        )}
      </Autocomplete>
    </div>
  )
}

async function searchDevelopers(
  query: string,
  filter: (item: string, query: string) => boolean
): Promise<Developer[]> {
  // Simulate network delay
  await new Promise((resolve) => {
    setTimeout(resolve, Math.random() * 800 + 200)
  })

  // Simulate occasional network errors (2% chance)
  if (Math.random() < 0.02 || query === "error") {
    throw new Error("Network error")
  }

  return topDevelopers.filter(
    (developer) =>
      filter(developer.name, query) ||
      filter(developer.role, query) ||
      filter(developer.location, query) ||
      developer.skills.some((skill) => filter(skill, query))
  )
}

interface Developer {
  id: string
  name: string
  role: string
  location: string
  skills: string[]
  experience: number
  rating: number
  avatar: string
}

const topDevelopers: Developer[] = [
  {
    id: "1",
    name: "Alex Chen",
    role: "Senior Full-Stack Developer",
    location: "San Francisco, CA",
    skills: ["React", "Node.js", "TypeScript", "AWS", "Docker"],
    experience: 8,
    rating: 4.9,
    avatar: "https://cdn.21st.dev/assets/mirror/b1/b1f6209ae26207ebe11c243a659f0e5e15a0a48232261ecf3c05211a40af2225.jpg",
  },
  {
    id: "2",
    name: "Sarah Johnson",
    role: "Frontend Architect",
    location: "New York, NY",
    skills: ["Vue.js", "JavaScript", "CSS", "Webpack", "Figma"],
    experience: 10,
    rating: 4.8,
    avatar: "https://cdn.21st.dev/assets/mirror/e7/e7a0b30cb92ca533b2f8dbf57649e4b60129a9e84f3fc36d45b09e2dfcaec61d.jpg",
  },
  {
    id: "3",
    name: "Michael Rodriguez",
    role: "Backend Engineer",
    location: "Austin, TX",
    skills: ["Python", "Django", "PostgreSQL", "Redis", "Kubernetes"],
    experience: 6,
    rating: 4.7,
    avatar: "https://cdn.21st.dev/assets/mirror/4c/4cff4f892ece6dca0865313df96f11ac30e11b6dcbf3b9a86bad86a3049aa6e1.jpg",
  },
  {
    id: "4",
    name: "Emily Wang",
    role: "DevOps Engineer",
    location: "Seattle, WA",
    skills: ["AWS", "Terraform", "Jenkins", "Docker", "Linux"],
    experience: 7,
    rating: 4.9,
    avatar: "https://cdn.21st.dev/assets/mirror/55/55d0cf713811843ffbd3412ee403668a82597bb83aabbc684a87f66c1fc962e4.jpg",
  },
  {
    id: "5",
    name: "David Kim",
    role: "Mobile Developer",
    location: "Los Angeles, CA",
    skills: ["React Native", "Swift", "Kotlin", "Firebase", "GraphQL"],
    experience: 5,
    rating: 4.6,
    avatar: "https://cdn.21st.dev/assets/mirror/32/32afb68c9233445d08f7c4af3e781f648c6eeeb7dadeb5bdd341a003684d1c93.jpg",
  },
  {
    id: "6",
    name: "Lisa Thompson",
    role: "Data Engineer",
    location: "Boston, MA",
    skills: ["Python", "Spark", "Airflow", "Snowflake", "dbt"],
    experience: 9,
    rating: 4.8,
    avatar: "https://cdn.21st.dev/assets/mirror/7f/7f2f1b6a4c09f5092437fe960232360d1e2dcf7a198c8580f3c5478c7b2d9386.jpg",
  },
  {
    id: "7",
    name: "James Wilson",
    role: "Cloud Solutions Architect",
    location: "Chicago, IL",
    skills: ["AWS", "Azure", "Terraform", "Kubernetes", "Microservices"],
    experience: 12,
    rating: 4.9,
    avatar: "https://cdn.21st.dev/assets/mirror/f2/f25b1b7a6a351c0f748d81bf4fcaf8c5a2f8ed036563c2693d4c1ca3718d9d5d.jpg",
  },
  {
    id: "8",
    name: "Maria Garcia",
    role: "UI/UX Developer",
    location: "Miami, FL",
    skills: ["React", "Figma", "CSS-in-JS", "Storybook", "Accessibility"],
    experience: 4,
    rating: 4.5,
    avatar: "https://cdn.21st.dev/assets/mirror/41/417105f5784df0a25c3486becfe5c967d448e3c98b3c0231ef4ea0c59d27cb4b.jpg",
  },
  {
    id: "9",
    name: "Robert Taylor",
    role: "Machine Learning Engineer",
    location: "Denver, CO",
    skills: ["Python", "TensorFlow", "PyTorch", "MLOps", "Docker"],
    experience: 6,
    rating: 4.7,
    avatar: "https://cdn.21st.dev/assets/mirror/62/6252a3b6790cbb48919cb8ea756a4e1ce829f3271a141731226871b3c3df9d6d.jpg",
  },
  {
    id: "10",
    name: "Jennifer Lee",
    role: "Blockchain Developer",
    location: "San Diego, CA",
    skills: ["Solidity", "Web3.js", "Ethereum", "Smart Contracts", "Rust"],
    experience: 5,
    rating: 4.6,
    avatar: "https://cdn.21st.dev/assets/mirror/54/54ebea0e1cad66565de28318ff2f512398bf5732f6f3f3fecea8ad4338b78778.jpg",
  },
  {
    id: "11",
    name: "Christopher Brown",
    role: "Security Engineer",
    location: "Portland, OR",
    skills: ["Cybersecurity", "Penetration Testing", "OWASP", "SIEM", "Python"],
    experience: 8,
    rating: 4.8,
    avatar: "https://cdn.21st.dev/assets/mirror/21/21d6722e3baa38cb075afdc599a60c8861440a6dea431e7a8ff43f86d05190b9.jpg",
  },
  {
    id: "12",
    name: "Amanda Davis",
    role: "Game Developer",
    location: "Orlando, FL",
    skills: ["Unity", "C#", "Unreal Engine", "3D Modeling", "VR/AR"],
    experience: 7,
    rating: 4.7,
    avatar: "https://cdn.21st.dev/assets/mirror/20/20bb8458e0bb0345aae5ab6a975650d1210fdfc5721729b456f7342fc59b3113.jpg",
  },
  {
    id: "13",
    name: "Kevin Zhang",
    role: "AI Research Engineer",
    location: "Palo Alto, CA",
    skills: ["Python", "PyTorch", "Transformers", "NLP", "Computer Vision"],
    experience: 6,
    rating: 4.9,
    avatar: "https://cdn.21st.dev/assets/mirror/56/56eb13218bfcbdfa4f6f950990d4666d616ad1f9e17777c976ccc79c781b68c6.jpg",
  },
  {
    id: "14",
    name: "Rachel Green",
    role: "QA Automation Engineer",
    location: "Phoenix, AZ",
    skills: ["Selenium", "Cypress", "Jest", "Python", "Test Automation"],
    experience: 5,
    rating: 4.6,
    avatar: "https://cdn.21st.dev/assets/mirror/4c/4c5eaf184e978fcf67bed792f0fa88543b664347c98727aa25da4c16e32eb367.jpg",
  },
  {
    id: "15",
    name: "Daniel Martinez",
    role: "System Administrator",
    location: "Dallas, TX",
    skills: [
      "Linux",
      "Windows Server",
      "Active Directory",
      "PowerShell",
      "VMware",
    ],
    experience: 10,
    rating: 4.8,
    avatar: "https://cdn.21st.dev/assets/mirror/e8/e86e44eaa9cb076c9d359973ce68af0e0cd85bb5dac2e72b259582941a57621b.jpg",
  },
  {
    id: "16",
    name: "Sophie Anderson",
    role: "Product Manager",
    location: "San Jose, CA",
    skills: [
      "Product Strategy",
      "Agile",
      "Analytics",
      "User Research",
      "Figma",
    ],
    experience: 8,
    rating: 4.7,
    avatar: "https://cdn.21st.dev/assets/mirror/cc/cc6b757fbf1174ae601b39aa711d6dfcda1b236001a2f3a67c4293d73c9fd714.jpg",
  },
  {
    id: "17",
    name: "Mark Thompson",
    role: "Database Administrator",
    location: "Atlanta, GA",
    skills: ["PostgreSQL", "MySQL", "MongoDB", "Redis", "Performance Tuning"],
    experience: 11,
    rating: 4.9,
    avatar: "https://cdn.21st.dev/assets/mirror/e8/e86e44eaa9cb076c9d359973ce68af0e0cd85bb5dac2e72b259582941a57621b.jpg",
  },
  {
    id: "18",
    name: "Jessica White",
    role: "Technical Writer",
    location: "Raleigh, NC",
    skills: [
      "Technical Writing",
      "Markdown",
      "API Documentation",
      "Git",
      "Confluence",
    ],
    experience: 6,
    rating: 4.5,
    avatar: "https://cdn.21st.dev/assets/mirror/1c/1c09f1ab9440e4f0922ad6c7494fb9efa1ae446ac5d39c4fb5843ed3fbc5c952.jpg",
  },
  {
    id: "19",
    name: "Andrew Clark",
    role: "Site Reliability Engineer",
    location: "Nashville, TN",
    skills: [
      "Monitoring",
      "Incident Response",
      "Python",
      "Terraform",
      "Kubernetes",
    ],
    experience: 7,
    rating: 4.8,
    avatar: "https://cdn.21st.dev/assets/mirror/c4/c493b0a6d9a42ed0a102bcd31360d00491e23ac5cb4f7cbf8ae9c61f577ccccc.jpg",
  },
  {
    id: "20",
    name: "Nicole Taylor",
    role: "Frontend Developer",
    location: "Minneapolis, MN",
    skills: ["Angular", "TypeScript", "RxJS", "SCSS", "Jest"],
    experience: 4,
    rating: 4.6,
    avatar: "https://cdn.21st.dev/assets/mirror/56/56cfb2a08032e82843ccac91504bbf42ababde4aea91bbacd9b683912cd8b21a.jpg",
  },
  {
    id: "21",
    name: "Ryan Murphy",
    role: "Backend Developer",
    location: "Kansas City, MO",
    skills: ["Java", "Spring Boot", "Microservices", "MongoDB", "Docker"],
    experience: 6,
    rating: 4.7,
    avatar: "https://cdn.21st.dev/assets/mirror/35/3560ff7cbc9e86c333fccefe248e3ea5cdade4e46f6b2fc85d84755896cb2e5a.jpg",
  },
  {
    id: "22",
    name: "Stephanie Lewis",
    role: "Full-Stack Developer",
    location: "Salt Lake City, UT",
    skills: ["React", "Node.js", "GraphQL", "PostgreSQL", "AWS"],
    experience: 5,
    rating: 4.6,
    avatar: "https://cdn.21st.dev/assets/mirror/aa/aa4787be04406deac036c92ff766754aa511214f00a4ee181ada4fc2c6622b6f.jpg",
  },
  {
    id: "23",
    name: "Brandon Wright",
    role: "Cloud Engineer",
    location: "Las Vegas, NV",
    skills: ["AWS", "Terraform", "Docker", "Kubernetes", "Python"],
    experience: 8,
    rating: 4.8,
    avatar: "https://cdn.21st.dev/assets/mirror/ca/ca627d33f20754d25814a1d622a9f4837d56d5809c6fa7c14f2f2be7e3f36a05.jpg",
  },
  {
    id: "24",
    name: "Ashley Hall",
    role: "DevOps Engineer",
    location: "Columbus, OH",
    skills: ["CI/CD", "Jenkins", "GitLab", "Docker", "AWS"],
    experience: 6,
    rating: 4.7,
    avatar: "https://cdn.21st.dev/assets/mirror/aa/aaab4a0fbd8e2ad7a7ec4ccaa827918df0d6af1732227caa84d309cb49b45c21.jpg",
  },
  {
    id: "25",
    name: "Tyler Young",
    role: "Mobile App Developer",
    location: "Tampa, FL",
    skills: ["Flutter", "Dart", "Firebase", "REST APIs", "Git"],
    experience: 4,
    rating: 4.5,
    avatar: "https://cdn.21st.dev/assets/mirror/f6/f61f4b3793465be0aa4c3b577a6ccb95ae48ccfb751e7aaa6b5477aadc5255a8.jpg",
  },
]
```

Install NPM dependencies:
```bash
npm install @base-ui/react class-variance-authority lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add scroll-area
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
