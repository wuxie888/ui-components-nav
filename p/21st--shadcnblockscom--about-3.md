<!-- About 3 · @shadcnblockscom · https://21st.dev/@shadcnblockscom/components/about-3
     license: unspecified · category: team
     An About Us Landing page with breakout images, logos and stats about your company and team. -->

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
components/ui/about3.tsx

import {
  Marquee,
  MarqueeContent,
  MarqueeFade,
  MarqueeItem,
} from "@/components/kibo-ui/marquee";
import { Button } from "@/components/ui/button";
import { cn } from "@/lib/utils";

interface AboutCompanySection {
  title: string;
  content: string;
  label?: string;
}
interface AboutCompanyLogo {
  name?: string;
}
interface Image {
  src: string;
  alt: string;
  srcDark?: string;
}
interface Stat {
  value: string;
  label: string;
  description?: string;
}

interface AboutCompanyProps {
  heading: string;
  description?: string;
  statsHeading?: string;
  statsDescription?: string;
  images?: Image[];
  stats?: Stat[];
  logos?: AboutCompanyLogo[];
  sections?: AboutCompanySection[];
  className?: string;
}

interface About3Props extends AboutCompanyProps {
  breakout?: About3Breakout;
}
type Props = Partial<About3Props>;

const defaultProps: About3Props = {
  heading: "About Us",
  description: "We are a passionate team dedicated to creating innovative solutions that empower businesses to thrive in the digital age. With years of experience in design and development, we craft beautiful, accessible components that help teams build faster.",
  statsHeading: "Our Achievements in Numbers",
  statsDescription: "Providing businesses with effective tools to improve workflows, boost efficiency, and encourage growth.",
  images: [
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/modern/about-company/photo-1-4x3.jpg",
      alt: "Conference room with a long wood table and red chairs",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/modern/about-company/photo-2-4x3.jpg",
      alt: "Team meeting around a conference table",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/modern/about-company/photo-3-3x4.jpg",
      alt: "Team member smiling in the office",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/modern/about-company/photo-4-4x3.jpg",
      alt: "Person walking past a modern office lobby",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/modern/about-company/photo-5-3x4.jpg",
      alt: "Colleagues talking in the workplace",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/modern/about-company/photo-6-4x3.jpg",
      alt: "Looking out over a modern interior atrium",
    },
  ],
  stats: [
    {
      value: "21M",
      label: "Global Reach of Users",
      description:
        "Streamline tasks and boost efficiency by up to 80% using our tools.",
    },
    {
      value: "12+",
      label: "Years of Expertise",
      description:
        "Years building products, improving processes, and shaping thoughtful systems.",
    },
    {
      value: "654",
      label: "Projects Completed",
      description:
        "Projects delivered across diverse industries, from food and beverage to fintech.",
    },
    {
      value: "113k+",
      label: "Monthly Active Users",
    },
    {
      value: "461k",
      label: "Registered Accounts",
    },
    {
      value: "98+",
      label: "Daily Users",
    },
  ],
  logos: [
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/placeholder/logos/fictional-company-logo-1.svg",
      alt: "Acme",
      name: "Acme",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/placeholder/logos/fictional-company-logo-2.svg",
      alt: "Creative",
      name: "Creative",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/placeholder/logos/fictional-company-logo-3.svg",
      alt: "Octan",
      name: "Octan",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/placeholder/logos/fictional-company-logo-4.svg",
      alt: "Newco",
      name: "Newco",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/placeholder/logos/fictional-company-logo-5.svg",
      alt: "Contoso",
      name: "Contoso",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/placeholder/logos/fictional-company-logo-6.svg",
      alt: "Fabrikam",
      name: "Fabrikam",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/placeholder/logos/fictional-company-logo-7.svg",
      alt: "Litware",
      name: "Litware",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/placeholder/logos/fictional-company-logo-8.svg",
      alt: "Northwind",
      name: "Northwind",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/placeholder/logos/fictional-company-logo-9.svg",
      alt: "Adventure Works",
      name: "Adventure Works",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/placeholder/logos/fictional-company-logo-10.svg",
      alt: "Wide World",
      name: "Wide World",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/placeholder/logos/fictional-company-logo-11.svg",
      alt: "Alpine",
      name: "Alpine",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/placeholder/logos/fictional-company-logo-12.svg",
      alt: "Horizon",
      name: "Horizon",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/placeholder/logos/fictional-company-logo-1.svg",
      alt: "Acme",
      name: "Acme",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/placeholder/logos/fictional-company-logo-2.svg",
      alt: "Creative",
      name: "Creative",
    },
    {
      src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/placeholder/logos/fictional-company-logo-3.svg",
      alt: "Octan",
      name: "Octan",
    },
  ],
  sections: [
    {
      title: "Our Vision",
      content:
        "For years, the process of building custom software has remained challenging. Today, visual builders exist, but tailored solutions still require technical expertise and a lot of time. This is a problem for businesses and individuals alike.\n\nWhat if you could create custom software without writing a single line of code? What if you could build your own tools.\n\nWith our platform, you can! Our tools let you design layouts and create functionality—all without needing to code.\n\nWe believe that everyone should be able to build their own solutions, regardless of their technical background.",
    },
    {
      title: "Our Creators",
      content:
        "Our company has been building web tools for over a decade, focusing on efficiency and user control in every project. We know that the best solutions are the ones that you can create yourself.\n\nWe initially developed these solutions for our own team, and now everyone can benefit from them too. We are proud to offer a platform that is accessible to all, regardless of technical expertise.\n\nOur team is made up of talented individuals who are passionate about creating tools that empower users to build their own solutions with ease. We are dedicated to helping you achieve your goals.",
    },
    {
      label: "Our mission",
      title: "We make creating software easy.",
      content:
        "We aim to help empower 1,000,000 teams to create their own software. Here is how we plan on doing it.",
    },
    {
      label: "What drives us",
      title:
        "We are a team of creators, thinkers, and builders who believe in crafting experiences that truly connect. Our story is built on passion, innovation, and the drive to bring meaningful ideas to life.",
      content:
        "We start from the purpose, the people it serves, and the simplest path forward. Clarity first, then the work gets better.",
    },
  ],
  breakout: {
    src: "https://deifkwefumgah.cloudfront.net/shadcnblocks/image-set/placeholder/logos/fictional-company-logo-1.svg",
    alt: "logo",
    title: "Hundreds of blocks at Shadcnblocks.com",
    description:
      "Providing businesses with effective tools to improve workflows, boost efficiency, and encourage growth.",
    buttonText: "Discover more",
    buttonUrl: "https://www.shadcnblocks.com",
  },
};

interface About3Breakout {
  src?: string;
  alt?: string;
  title: string;
  description: string;
  buttonText?: string;
  buttonUrl?: string;
}

const MAX_HERO_IMAGES = 2;
const MAX_STORY_IMAGES = 4;
const MAX_LOGOS = 6;
const MAX_STATS = 4;
const MAX_SECTIONS = 2;

const About3 = (props: Props) => {
  const {
    heading,
    description,
    images,
    logos,
    stats,
    statsHeading,
    statsDescription,
    sections,
    breakout,
    className,
  } = {
    ...defaultProps,
    ...props,
  };

  const gallery = (images ?? []).slice(0, MAX_HERO_IMAGES);
  const storyGallery = (images ?? []).slice(
    MAX_HERO_IMAGES,
    MAX_HERO_IMAGES + MAX_STORY_IMAGES,
  );
  const companies = (logos ?? []).slice(0, MAX_LOGOS);
  const achievements = (stats ?? []).slice(0, MAX_STATS);
  const contentSections = (sections ?? []).slice(0, MAX_SECTIONS);

  return (
    <section className={cn("py-32", className)}>
      <div className="container mx-auto">
        <div className="mb-14 flex flex-col gap-5 lg:w-2/3">
          <h1 className="text-5xl font-semibold tracking-tighter lg:text-6xl">
            {heading}
          </h1>
          {description && (
            <p className="text-lg text-muted-foreground md:text-xl">
              {description}
            </p>
          )}
        </div>
        <div className="grid gap-7 lg:grid-cols-3">
          {gallery[0] && (
            <img
              src={gallery[0].src}
              alt={gallery[0].alt}
              className="aspect-4/3 max-h-155 w-full rounded-xl object-cover lg:col-span-2"
            />
          )}
          <div className="flex flex-col gap-7 md:flex-row lg:flex-col">
            {breakout && (
              <div className="flex flex-col justify-between gap-6 rounded-xl bg-muted p-7 md:w-1/2 lg:w-auto">
                {breakout.src && (
                  <img
                    src={breakout.src}
                    alt={breakout.alt}
                    className="mr-auto h-12 dark:invert"
                  />
                )}
                <div>
                  <p className="mb-2 text-lg font-semibold">{breakout.title}</p>
                  <p className="text-muted-foreground">
                    {breakout.description}
                  </p>
                </div>
                {breakout.buttonText && breakout.buttonUrl && (
                  <Button variant="outline" className="mr-auto" asChild>
                    <a href={breakout.buttonUrl} target="_blank">
                      {breakout.buttonText}
                    </a>
                  </Button>
                )}
              </div>
            )}
            {gallery[1] && (
              <img
                src={gallery[1].src}
                alt={gallery[1].alt}
                className="aspect-4/3 w-full rounded-xl object-cover md:w-1/2 lg:w-auto"
              />
            )}
          </div>
        </div>
        {companies.length > 0 && (
          <div className="py-32">
            <Marquee>
              <MarqueeContent speed={40}>
                {companies.map((company, idx) => (
                  <MarqueeItem
                    key={company.src + idx}
                    className="mx-8 flex items-center"
                  >
                    <img
                      src={company.src}
                      alt={company.alt}
                      className="h-7 w-auto md:h-8 dark:invert"
                    />
                  </MarqueeItem>
                ))}
              </MarqueeContent>
              <MarqueeFade side="left" />
              <MarqueeFade side="right" />
            </Marquee>
          </div>
        )}
        <div className="relative overflow-hidden rounded-xl bg-muted p-7 md:p-16">
          <div className="flex flex-col gap-4 text-center md:text-left">
            {statsHeading && (
              <h2 className="text-3xl font-medium md:text-4xl">
                {statsHeading}
              </h2>
            )}
            {statsDescription && (
              <p className="max-w-xl text-muted-foreground">
                {statsDescription}
              </p>
            )}
          </div>
          <div className="mt-10 grid grid-cols-2 gap-x-4 gap-y-8 md:flex md:flex-wrap md:justify-between">
            {achievements.map((item) => (
              <div
                className="flex flex-col gap-2 text-center md:text-left"
                key={item.label}
              >
                <span className="font-mono text-4xl font-semibold md:text-5xl">
                  {item.value}
                </span>
                <p className="text-sm md:text-base">{item.label}</p>
              </div>
            ))}
          </div>
        </div>
        {contentSections.length > 0 && (
          <div className="grid gap-16 py-28 md:grid-cols-12">
            {contentSections[0] && (
              <div className="flex flex-col gap-5 md:col-span-5">
                <h2 className="text-4xl font-medium tracking-tight">
                  {contentSections[0].title}
                </h2>
                <p className="text-lg leading-7 whitespace-pre-line text-muted-foreground">
                  {contentSections[0].content}
                </p>
              </div>
            )}
            {contentSections[1] && (
              <div className="flex flex-col gap-5 md:col-span-5 md:col-start-8 md:mt-28">
                <h2 className="text-4xl font-medium tracking-tight">
                  {contentSections[1].title}
                </h2>
                <p className="text-lg leading-7 whitespace-pre-line text-muted-foreground">
                  {contentSections[1].content}
                </p>
              </div>
            )}
          </div>
        )}
        {storyGallery.length > 0 && (
          <div className="grid gap-6 md:grid-cols-12">
            {storyGallery[0] && (
              <img
                src={storyGallery[0].src}
                alt={storyGallery[0].alt}
                className="aspect-3/4 w-full rounded-xl object-cover md:col-span-5"
              />
            )}
            {storyGallery[1] && (
              <img
                src={storyGallery[1].src}
                alt={storyGallery[1].alt}
                className="aspect-4/3 w-full self-end rounded-xl object-cover md:col-span-7"
              />
            )}
            {storyGallery[2] && (
              <img
                src={storyGallery[2].src}
                alt={storyGallery[2].alt}
                className="aspect-3/4 w-full rounded-xl object-cover md:col-span-5 md:col-start-8 md:row-start-2"
              />
            )}
            {storyGallery[3] && (
              <img
                src={storyGallery[3].src}
                alt={storyGallery[3].alt}
                className="aspect-4/3 w-full rounded-xl object-cover md:col-span-7 md:col-start-1 md:row-start-2"
              />
            )}
          </div>
        )}
      </div>
    </section>
  );
};

export { About3 };

demo.tsx
import { About3 } from "@/components/ui/about-3";

const DemoOne = () => {
  return (
    <About3
      title="About Us"
      description="Shadcnblocks is a passionate team dedicated to creating innovative solutions that empower businesses to thrive in the digital age."
      mainImage={{
        src: "https://cdn.21st.dev/assets/mirror/9f/9fb9487617e0903ab5bb8012f11530f61a39e4f1f19f169a87a367887fdaeecb.svg",
        alt: "placeholder",
      }}
      secondaryImage={{
        src: "https://cdn.21st.dev/assets/mirror/f0/f0899c6c439a70c9602ca80add37c87c8398b7dc9566778a5894584a412749c4.svg",
        alt: "placeholder",
      }}
      breakout={{
        src: "https://cdn.21st.dev/assets/mirror/06/067c72836298829da27d230af61c2b4be0e09da5103dc2789639d18beea789f4.svg",
        alt: "logo",
        title: "Hundreds of blocks at Shadcnblocks.com",
        description:
          "Providing businesses with effective tools to improve workflows, boost efficiency, and encourage growth.",
        buttonText: "Discover more",
        buttonUrl: "https://shadcnblocks.com",
      }}
      companiesTitle="Valued by clients worldwide"
      companies={[
        {
          src: "https://cdn.21st.dev/assets/mirror/01/01eed6b231e04442ccc351628f02a2a115a69c5d21e14a5646d006c68d9ac3de.svg",
          alt: "Arc",
        },
        {
          src: "https://cdn.21st.dev/assets/mirror/51/51129a2a091b1e72cfd668d53fd5bea734a95ff5fd071fd25b866a0c1454f0e0.svg",
          alt: "Descript",
        },
        {
          src: "https://cdn.21st.dev/assets/mirror/00/0088937b0b0c70cfabea9beeab64949ab3a0b5ea52f8684a4ce7875b45a2a706.svg",
          alt: "Mercury",
        },
        {
          src: "https://cdn.21st.dev/assets/mirror/76/760366384a3523aa7b033c30a1cb3d64d0b7c93d5c2535f89a770383ee329aab.svg",
          alt: "Ramp",
        },
        {
          src: "https://cdn.21st.dev/assets/mirror/ed/ed0e41fdd91fa17618e67bb21dcf842eb10c4bd82d9196246d231d9069556406.svg",
          alt: "Retool",
        },
        {
          src: "https://cdn.21st.dev/assets/mirror/b6/b6951d37dfa577ecbd68657f8799da4a15e03e55fb110438bd7222ccc9335494.svg",
          alt: "Watershed",
        }
      ]}
      achievementsTitle="Our Achievements in Numbers"
      achievementsDescription="Providing businesses with effective tools to improve workflows, boost efficiency, and encourage growth."
      achievements={
        [
          { label: "Companies Supported", value: "300+" },
          { label: "Projects Finalized", value: "800+" },
          { label: "Happy Customers", value: "99%" },
          { label: "Recognized Awards", value: "10+" },
        ]
      }
    />
  );
};

export { DemoOne };
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button marquee.json utils
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
