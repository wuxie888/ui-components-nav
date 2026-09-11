<!-- Accordion · @preetsuthar17 · https://21st.dev/@preetsuthar17/components/accordion-1
     license: unspecified · category: accordion
     A vertically stacked set of interactive headings that each reveal an associated section of content. -->

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
components/ui/accordion.tsx
"use client";

import * as AccordionPrimitive from "@radix-ui/react-accordion";
import { ChevronDownIcon } from "lucide-react";
import type * as React from "react";

import { cn } from "@/lib/utils";

function Accordion({
  ...props
}: React.ComponentProps<typeof AccordionPrimitive.Root>) {
  return <AccordionPrimitive.Root data-slot="accordion" {...props} />;
}

function AccordionItem({
  className,
  ...props
}: React.ComponentProps<typeof AccordionPrimitive.Item>) {
  return (
    <AccordionPrimitive.Item
      className={cn("border-b last:border-b-0", className)}
      data-slot="accordion-item"
      {...props}
    />
  );
}

function AccordionTrigger({
  className,
  children,
  ...props
}: React.ComponentProps<typeof AccordionPrimitive.Trigger>) {
  return (
    <AccordionPrimitive.Header className="flex">
      <AccordionPrimitive.Trigger
        className={cn(
          "flex min-h-11 flex-1 touch-manipulation items-start justify-between gap-4 rounded-md py-4 text-left font-medium text-sm outline-none transition-colors hover:underline focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50 disabled:pointer-events-none disabled:opacity-50 motion-safe:duration-200 [&[data-state=open]>svg]:rotate-180",
          className
        )}
        data-slot="accordion-trigger"
        type="button"
        {...props}
      >
        {children}
        <ChevronDownIcon
          aria-hidden="true"
          className="pointer-events-none size-4 shrink-0 translate-y-0.5 text-muted-foreground transition-transform duration-200"
        />
      </AccordionPrimitive.Trigger>
    </AccordionPrimitive.Header>
  );
}

function AccordionContent({
  className,
  children,
  ...props
}: React.ComponentProps<typeof AccordionPrimitive.Content>) {
  return (
    <AccordionPrimitive.Content
      className="overflow-hidden text-sm data-[state=closed]:animate-accordion-up data-[state=open]:animate-accordion-down motion-reduce:animate-none"
      data-slot="accordion-content"
      {...props}
    >
      <div className={cn("pt-0 pb-4", className)}>{children}</div>
    </AccordionPrimitive.Content>
  );
}

export { Accordion, AccordionItem, AccordionTrigger, AccordionContent };

demo.tsx
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger,
} from "@/components/ui/accordion-1";
import { Star, Shield, Zap, Heart,Palette,TrendingUp,BarChart3,DollarSign,MessageCircle} from "lucide-react";


export default function DemoOne() {
  return(
    <>
    <Accordion type="single" collapsible className="w-full">
      <AccordionItem value="features">
        <AccordionTrigger icon={<Star className="h-4 w-4" />}>
          Features
        </AccordionTrigger>
        <AccordionContent>
          Our platform includes advanced features like real-time collaboration,
          version control, and automated deployments.
        </AccordionContent>
      </AccordionItem>
      
      <AccordionItem value="security">
        <AccordionTrigger icon={<Shield className="h-4 w-4" />}>
          Security
        </AccordionTrigger>
        <AccordionContent>
          We implement enterprise-grade security with end-to-end encryption,
          two-factor authentication, and regular security audits.
        </AccordionContent>
      </AccordionItem>
      
      <AccordionItem value="pricing">
        <AccordionTrigger icon={<DollarSign className="h-4 w-4" />}>
          Pricing
        </AccordionTrigger>
        <AccordionContent>
          Choose from our flexible pricing plans: Starter ($9/month), Pro ($29/month), 
          and Enterprise (custom pricing). All plans include a 14-day free trial.
        </AccordionContent>
      </AccordionItem>
      
      <AccordionItem value="support">
        <AccordionTrigger icon={<MessageCircle className="h-4 w-4" />}>
          Support
        </AccordionTrigger>
        <AccordionContent>
          Get help when you need it with 24/7 chat support, comprehensive documentation, 
          video tutorials, and dedicated account managers for enterprise customers.
        </AccordionContent>
      </AccordionItem>
      
      <AccordionItem value="integrations">
        <AccordionTrigger icon={<Zap className="h-4 w-4" />}>
          Integrations
        </AccordionTrigger>
        <AccordionContent>
          Connect with over 100+ popular tools including Slack, GitHub, Jira, 
          Google Workspace, Microsoft 365, and many more through our API.
        </AccordionContent>
      </AccordionItem>
      
      <AccordionItem value="performance">
        <AccordionTrigger icon={<TrendingUp className="h-4 w-4" />}>
          Performance
        </AccordionTrigger>
        <AccordionContent>
          Experience lightning-fast load times with our global CDN, 99.9% uptime SLA, 
          and automatic scaling to handle traffic spikes seamlessly.
        </AccordionContent>
      </AccordionItem>
      
      <AccordionItem value="analytics">
        <AccordionTrigger icon={<BarChart3 className="h-4 w-4" />}>
          Analytics & Reporting
        </AccordionTrigger>
        <AccordionContent>
          Gain insights with detailed analytics, custom dashboards, automated reports, 
          and real-time monitoring of your key performance metrics.
        </AccordionContent>
      </AccordionItem>
      
      <AccordionItem value="customization">
        <AccordionTrigger icon={<Palette className="h-4 w-4" />}>
          Customization
        </AccordionTrigger>
        <AccordionContent>
          Tailor the platform to your brand with custom themes, white-label options, 
          personalized workflows, and configurable user permissions.
        </AccordionContent>
      </AccordionItem>
    </Accordion>
    </>
  )
}
```

Install NPM dependencies:
```bash
npm install @radix-ui/react-accordion class-variance-authority lucide-react
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
