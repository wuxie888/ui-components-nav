<!-- Contact 01 - Project Inquiry Form · @shadcnspace · https://21st.dev/@shadcnspace/components/contact-01
     license: MIT · category: form
     A contact section with a project inquiry form, contact details, and a trust-badge brand marquee for agencies and freelancers to capture qualified leads. -->

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
components/shadcn-space/blocks/contact-01/page.tsx
import Contact from "@/components/shadcn-space/blocks/contact-01/index";

const Page = () => {
  return <Contact />;
};

export default Page;

components/shadcn-space/blocks/contact-01/index.tsx
import ContactInfo from "@/components/shadcn-space/blocks/contact-01/contact-info";
import ContactForm from "@/components/shadcn-space/blocks/contact-01/contact-form";

const Contact = () => {
  return (
    <section className="py-10 md:py-20">
      <div className="max-w-7xl xl:px-16 lg:px-8 px-4 mx-auto">
        <div className="grid grid-cols-12 content-center justify-between gap-6 sm:gap-8 md:gap-0">
          <div className="w-full col-span-12 md:col-span-6">
            <ContactInfo />
          </div>
          <div className="col-span-1"></div>
          <div className="w-full col-span-12 md:col-span-5">
            <ContactForm />
          </div>
        </div>
      </div>
    </section>
  );
};

export default Contact;

components/shadcn-space/blocks/contact-01/contact-info.tsx
"use client";

import { Separator } from "@/components/ui/separator";
import { Marquee } from "@/components/shadcn-space/animations/marquee";

type BrandList = {
  image: string;
  name: string;
  lightimg: string;
};

const brandList: BrandList[] = [
  {
    image:
      "https://images.shadcnspace.com/assets/brand-logo/logoipsum-muted-1.svg",
    lightimg:
      "https://images.shadcnspace.com/assets/brand-logo/logoipsum-muted-white-1.svg",
    name: "Brand 1",
  },
  {
    image:
      "https://images.shadcnspace.com/assets/brand-logo/logoipsum-muted-2.svg",
    lightimg:
      "https://images.shadcnspace.com/assets/brand-logo/logoipsum-muted-white-2.svg",
    name: "Brand 2",
  },
  {
    image:
      "https://images.shadcnspace.com/assets/brand-logo/logoipsum-muted-3.svg",
    lightimg:
      "https://images.shadcnspace.com/assets/brand-logo/logoipsum-muted-white-3.svg",
    name: "Brand 3",
  },
  {
    image:
      "https://images.shadcnspace.com/assets/brand-logo/logoipsum-muted-4.svg",
    lightimg:
      "https://images.shadcnspace.com/assets/brand-logo/logoipsum-muted-white-4.svg",
    name: "Brand 4",
  },
  {
    image:
      "https://images.shadcnspace.com/assets/brand-logo/logoipsum-muted-5.svg",
    lightimg:
      "https://images.shadcnspace.com/assets/brand-logo/logoipsum-muted-white-5.svg",
    name: "Brand 5",
  },
];

const ContactInfo = () => {
  return (
    <div className="flex flex-col md:gap-12 gap-8">
      <div className="flex flex-col gap-6 animate-in fade-in slide-in-from-left-10 duration-1000 ease-in-out fill-mode-both">
        <div className="flex gap-3 items-center">
          <div className="w-2 h-2 rounded-full bg-teal-400"></div>
          <p className="text-base font-normal text-muted-foreground">
            We can help
          </p>
        </div>
        <p className="text-3xl  md:text-4xl font-medium text-foreground">
          Let’s discuss about your project and take it the next level.
        </p>
      </div>
      <div className="flex flex-col sm:flex-row justify-between gap-6 animate-in fade-in slide-in-from-left-10 duration-1000 delay-100 ease-in-out fill-mode-both">
        <div className="flex flex-col gap-1">
          <p className="text-sm font-normal text-muted-foreground">Phone</p>
          <a
            href="tel:+323-25-8964"
            className="text-base font-medium text-primary"
          >
            +323-25-8964
          </a>
        </div>
        <div className="flex flex-col gap-1">
          <p className="text-sm font-normal text-muted-foreground">Email</p>
          <a
            href="mailto:me@shadcnspace.com"
            className="text-base font-medium text-primary"
          >
            me@shadcnspace.com
          </a>
        </div>
      </div>
      <div className="flex flex-col gap-1 animate-in fade-in slide-in-from-left-10 duration-1000 delay-100 ease-in-out fill-mode-both">
        <p className="text-sm font-normal text-muted-foreground">Location</p>
        <p className="text-base font-medium text-primary">
          Mark Avenue, Dalls Road, New York
        </p>
      </div>
      <Separator orientation="horizontal" />
      <div className="flex flex-col gap-6 animate-in fade-in slide-in-from-bottom-10 duration-1000 delay-100 ease-in-out fill-mode-both">
        <p className="text-base font-normal text-muted-foreground ">
          Trusted by
        </p>
        <Marquee pauseOnHover className="[--duration:20s] p-0">
          {brandList.map((brand, index) => (
            <div key={index}>
              <img
                src={brand.image}
                alt={brand.name}
                className="w-36 h-8 mr-6 lg:mr-20 dark:hidden"
              />
              <img
                src={brand.lightimg}
                alt={brand.name}
                className="hidden dark:block w-36 h-8 mr-12 lg:mr-20"
              />
            </div>
          ))}
        </Marquee>
      </div>
    </div>
  );
};

export default ContactInfo;

components/shadcn-space/blocks/contact-01/contact-form.tsx
"use client";

import React, { useState } from "react";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";
import { Checkbox } from "@/components/ui/checkbox";
import { Label } from "@/components/ui/label";
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "@/components/ui/select";

interface ContactFormData {
  firstName: string;
  lastName: string;
  email: string;
  country: string;
  message: string;
  terms: boolean;
}

const ContactForm = () => {
  const [formData, setFormData] = useState<ContactFormData>({
    firstName: "",
    lastName: "",
    email: "",
    country: "",
    message: "",
    terms: false,
  });

  const handleChange = (
    e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>
  ) => {
    const { name, value } = e.target;
    setFormData((prev) => ({ ...prev, [name]: value }));
  };

  const handleCheckboxChange = (checked: boolean) => {
    setFormData((prev) => ({ ...prev, terms: checked }));
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
  };

  return (
    <div className="w-full">
      <Card className="ring-0 p-8 gap-6 md:gap-8 border rounded-2xl animate-in fade-in slide-in-from-right-10 duration-1000 delay-100 ease-in-out fill-mode-both">
        <CardHeader className="p-0">
          <CardTitle className="text-2xl font-semibold text-primary">
            Start the project
          </CardTitle>
        </CardHeader>
        <CardContent className="p-0">
          <form onSubmit={handleSubmit} className="space-y-5">
            <div className="flex flex-col gap-8">
              <div className="flex flex-col gap-6">
                {/* form inputs */}
                <div className="grid grid-cols-1 lg:grid-cols-2 gap-6 sm:gap-4">
                  <div>
                    <Input
                      id="firstName"
                      name="firstName"
                      placeholder="First name"
                      value={formData.firstName}
                      onChange={handleChange}
                      className="dark:bg-background h-9 shadow-xs"
                      required
                    />
                  </div>
                  <div>
                    <Input
                      id="lastName"
                      name="lastName"
                      placeholder="Last name"
                      value={formData.lastName}
                      onChange={handleChange}
                      className="dark:bg-background h-9 shadow-xs"
                      required
                    />
                  </div>
                </div>

                <div>
                  <Input
                    id="email"
                    name="email"
                    placeholder="youremail@website.com"
                    type="email"
                    value={formData.email}
                    onChange={handleChange}
                    className="dark:bg-background h-9 shadow-xs"
                    required
                  />
                </div>

                <div>
                  <Select
                    value={formData.country}
                    onValueChange={(value) =>
                      setFormData((prev) => ({ ...prev, country: value ?? "" }))
                    }
                  >
                    <SelectTrigger id="country" className="w-full h-9! dark:bg-background shadow-xs">
                      <SelectValue />
                    </SelectTrigger>
                    <SelectContent>
                      <SelectItem value="United States">
                        United States
                      </SelectItem>
                      <SelectItem value="United Kingdom">
                        United Kingdom
                      </SelectItem>
                      <SelectItem value="Canada">Canada</SelectItem>
                      <SelectItem value="Australia">Australia</SelectItem>
                      <SelectItem value="Germany">Germany</SelectItem>
                      <SelectItem value="France">France</SelectItem>
                      <SelectItem value="India">India</SelectItem>
                    </SelectContent>
                  </Select>
                </div>

                <div>
                  <Textarea
                    id="message"
                    name="message"
                    placeholder="Let us know about your project"
                    value={formData.message}
                    onChange={handleChange}
                    className="h-20 resize-none dark:bg-background shadow-xs"
                    required
                  />
                </div>

                <div className="flex items-center gap-3">
                  <Checkbox
                    id="terms"
                    checked={formData.terms}
                    onCheckedChange={handleCheckboxChange}
                    required
                  />
                  <Label
                    htmlFor="terms"
                    className="text-sm font-normal text-primary select-none"
                  >
                    I have read and acknowledge the Terms and Conditions
                  </Label>
                </div>
              </div>
              {/* submit button */}
              <Button
                type="submit"
                size="lg"
                className="rounded-xl bg-blue-500 hover:bg-blue-500/80 text-white hover:cursor-pointer h-10"
              >
                Submit Inquiry
              </Button>
            </div>
          </form>
        </CardContent>
      </Card>
    </div>
  );
};

export default ContactForm;

components/shadcn-space/animations/marquee.tsx
import { ComponentPropsWithoutRef } from "react";
 
import { cn } from "@/lib/utils";
 
interface MarqueeProps extends ComponentPropsWithoutRef<"div"> {
  className?: string;
  /**
   * Whether to reverse the animation direction
   * @default false
   */
  reverse?: boolean;
  /**
   * Whether to pause the animation on hover
   * @default false
   */
  pauseOnHover?: boolean;
  /**
   * Content to be displayed in the marquee
   */
  children: React.ReactNode;
  /**
   * Whether to animate vertically instead of horizontally
   * @default false
   */
  vertical?: boolean;
  /**
   * Number of times to repeat the content
   * @default 4
   */
  repeat?: number;
}
 
export function Marquee({
  className,
  reverse = false,
  pauseOnHover = false,
  children,
  vertical = false,
  repeat = 4,
  ...props
}: MarqueeProps) {
  return (
    <>
      <style>
        {`
          @keyframes marquee {
            from {
              transform: translateX(0);
            }
            to {
              transform: translateX(calc(-100% - var(--gap)));
            }
          }
 
          @keyframes marquee-vertical {
            from {
              transform: translateY(0);
            }
            to {
              transform: translateY(calc(-100% - var(--gap)));
            }
          }
 
          @keyframes scroll {
            to {
              transform: translate(calc(-50% - 0.5rem));
            }
          }
 
          .animate-marquee {
            animation: marquee var(--duration) linear infinite;
          }
 
          .animate-marquee-vertical {
            animation: marquee-vertical var(--duration) linear infinite;
          }
 
          .animate-reverse {
            animation-direction: reverse !important;
          }
 
          .pause-on-hover:hover .animate-marquee,
          .pause-on-hover:hover .animate-marquee-vertical {
            animation-play-state: paused !important;
          }
 
          .animate-scroll {
            animation: scroll var(--animation-duration, 40s) var(--animation-direction, forwards) linear infinite;
          }
        `}
      </style>
      <div
        {...props}
        className={cn(
          "group flex gap-(--gap) overflow-hidden p-2 [--duration:40s] [--gap:1rem]",
          {
            "flex-row": !vertical,
            "flex-col": vertical,
            "pause-on-hover": pauseOnHover,
          },
          className,
        )}
      >
        {Array(repeat)
          .fill(0)
          .map((_, i) => (
            <div
              key={i}
              className={cn("flex shrink-0 justify-around gap-(--gap)", {
                "animate-marquee flex-row": !vertical,
                "animate-marquee-vertical flex-col": vertical,
                "animate-reverse": reverse,
              })}
            >
              {children}
            </div>
          ))}
      </div>
    </>
  );
}

demo.tsx
import Contact from "@/components/ui/contact-01";

export default function ContactDemo() {
  return <Contact />;
}
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button card checkbox input label select separator textarea
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
