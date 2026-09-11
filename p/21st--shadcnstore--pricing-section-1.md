<!-- Pricing Section 1 · @shadcnstore · https://21st.dev/@shadcnstore/components/pricing-section-1
     license: MIT · category: cta
     A responsive three-tier pricing section with feature lists, a highlighted popular plan, and call-to-action buttons. -->

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
components/ui/page.tsx
import { PricingSection1 } from "@/components/blocks/marketing/pricing/pricing-section-1/components/pricing-section-1"

export default function Page() {
  return (
    <div className="flex min-h-svh w-full items-center justify-center">
      <div className="w-full">
        <PricingSection1 />
      </div>
    </div>
  )
}

components/ui/pricing-section-1.tsx
import { ArrowRight, Check } from 'lucide-react'
import { Button } from '@/components/ui/button'
import { Card, CardContent, CardFooter, CardHeader, CardTitle } from '@/components/ui/card'
import { cn } from '@/lib/utils'
import { Badge } from '@/components/ui/badge'

interface PricingTier {
  id: string
  name: string
  description: string
  price: string
  frequency: string
  features: string[]
  popular?: boolean
}

const pricingTiers: PricingTier[] = [
  {
    id: 'basic',
    name: 'Basic',
    description: 'Perfect for individuals getting started with our products',
    price: '$29',
    frequency: '/month',
    features: ['Up to 5 products', 'Basic analytics', 'Email support', 'Access to community forum'],
  },
  {
    id: 'professional',
    name: 'Professional',
    description: 'Ideal for growing businesses',
    price: '$79',
    frequency: '/month',
    features: ['Up to 50 products', 'Advanced analytics', 'Priority email support', 'API access', 'Custom domain'],
    popular: true,
  },
  {
    id: 'enterprise',
    name: 'Enterprise',
    description: 'For large scale businesses with advanced needs',
    price: '$199',
    frequency: '/month',
    features: [
      'Unlimited products',
      'Advanced analytics dashboard',
      '24/7 priority support',
      'Custom integrations',
      'Dedicated account manager',
    ],
  },
]

export function PricingSection1() {
  return (
    <section className='py-16' id='pricing'>
      <div className='mx-auto w-full max-w-7xl px-4 sm:px-6 lg:px-8'>
        <header className='mb-12 text-center'>
          <p className='text-muted-foreground mb-2 text-sm font-medium'>Our Pricing</p>
          <h2 className='text-3xl font-bold text-balance lg:text-5xl'>Find the perfect plan for your business</h2>
        </header>

        <div className='grid gap-6 lg:grid-cols-3 lg:gap-8'>
        {pricingTiers.map(tier => (
          <Card key={tier.name} className={cn('overflow-hidden py-6', { 'shadow-lg': tier.popular })}>
            <CardHeader className='px-6'>
              <div className='flex items-center justify-between gap-4'>
                <CardTitle className='text-xl font-bold lg:text-2xl'>{tier.name}</CardTitle>
                {tier.popular ? <Badge className="px-2.5 py-0.5 font-semibold rounded-full">Popular</Badge> : null}
              </div>
              <p className='text-muted-foreground text-sm'>{tier.description}</p>
            </CardHeader>
            <CardContent className='flex flex-col flex-1 px-6'>
              <div className='mb-6 flex items-baseline gap-2'>
                <span className='text-3xl font-bold lg:text-5xl'>{tier.price}</span>
                <span className='text-muted-foreground text-sm'>{tier.frequency}</span>
              </div>
              <ul className='flex flex-col gap-3'>
                {tier.features.map(feature => (
                  <li key={feature} className='flex items-center gap-2'>
                    <Check className='size-4 shrink-0' />
                    <span className='text-muted-foreground text-sm'>{feature}</span>
                  </li>
                ))}
              </ul>
            </CardContent>
            <CardFooter className='border-0 bg-transparent px-6 pb-6'>
              <Button
                className="h-10 px-8 w-full cursor-pointer gap-2"
                size='lg'
                variant={tier.popular ? 'default' : 'outline'}
                aria-label={`Get started with ${tier.name} plan`}
              >
                Get Started <ArrowRight />
              </Button>
            </CardFooter>
          </Card>
        ))}
        </div>
      </div>
    </section>
  )
}

export default PricingSection1

demo.tsx
import PricingSection1 from "@/components/ui/pricing-section-1";

export default function Default() {
  return (
    <div className="bg-background text-foreground">
      <PricingSection1 />
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
npx shadcn@latest add badge button card
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
