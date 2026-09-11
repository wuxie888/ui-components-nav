<!-- Footer Section 5 · @solaceui · https://21st.dev/@solaceui/components/footer-section-5
     license: no-license · category: footer
     A marketing website footer with an oversized outlined brand wordmark and a blue fluted-glass shader panel containing the logo, tagline, social icons, and grouped navigation link columns. -->

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
components/ui/index.tsx
"use client";
import React from "react";
import { FlutedGlass } from "@paper-design/shaders-react";
import Link from "next/link";

const companyName = "SolaceUI";


const SolaceUILogo = ({ className }: { className?: string }) => {
  return (
    <svg className={className} width="64" height="38" viewBox="0 0 64 38" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M0 20.1032L39.8387 20.1032C44.7808 20.1032 48.7871 24.1095 48.7871 29.0516C48.7871 33.9937 44.7808 38 39.8387 38L1.56459e-06 38L0 20.1032Z" fill="currentColor" />
      <path d="M63.4968 17.8968L23.6581 17.8968C18.716 17.8968 14.7097 13.8904 14.7097 8.94839C14.7097 4.00633 18.716 0 23.6581 0L63.4968 0V17.8968Z" fill="currentColor" />
    </svg>
  )
}

const footerLinks = [
  {
    title: "Product",
    links: [
      { name: "Features", href: "#" },
      { name: "Solution", href: "#" },
      { name: "Customers", href: "#" },
      { name: "Pricing", href: "#" },
      { name: "Help", href: "#" },
      { name: "Terms", href: "#" },
    ],
  },
  {
    title: "Company",
    links: [
      { name: "About", href: "#" },
      { name: "Careers", href: "#" },
      { name: "Blogs", href: "#" },
      { name: "Pricing", href: "#" },
      { name: "Contact", href: "#" },
      { name: "Privacy", href: "#" },
    ],
  },
  {
    title: "Social",
    links: [
      { name: "X", href: "#" },
      { name: "LinkedIn", href: "#" },
      { name: "Facebook", href: "#" },
      { name: "Threads", href: "#" },
      { name: "Instagram", href: "#" },
      { name: "Youtube", href: "#" },
    ],
  },
];

export default function FooterSection5() {
  return (
    <footer className="w-full bg-white relative overflow-hidden antialiased [font-synthesis:none]">
      {/* Large Stroke Text Section */}
      <div className="relative w-full flex justify-center items-end pt-24 md:pt-32 pb-0 z-0">
        <h1 className="text-[120px] sm:text-[160px] md:text-[210px] font-semibold text-transparent [-webkit-text-stroke:1px_rgba(0,0,0,0.4)] leading-[0.75] select-none -mb-4 md:-mb-6 opacity-50">
          {companyName}
        </h1>
      </div>

      {/* Blue Panel Section */}
      <div className="relative w-full [--color-primary:#1C76F8] bg-(--color-primary) z-10 min-h-[400px]">
        {/* Background Shader */}
        <div className="absolute inset-0 z-0 pointer-events-none">
          <FlutedGlass
            size={0.89}
            shape="lines"
            angle={0}
            distortionShape="prism"
            distortion={0.5}
            shift={0}
            blur={0}
            edges={0.25}
            stretch={0}
            scale={1.11}
            fit="cover"
            highlights={0.1}
            shadows={0.2}
            grainMixer={0.1}
            grainOverlay={0.1}
            colorBack="#00000000"
            colorHighlight="#FFFFFF"
            colorShadow="#000000"
            className="w-full h-full bg-transparent"
          />
        </div>

        {/* Content */}
        <div className="relative z-10 max-w-7xl mx-auto px-6 md:px-12 lg:px-24 py-16 md:py-24 flex flex-col lg:flex-row justify-between gap-16 lg:gap-8">

          {/* Left Side */}
          <div className="flex flex-col justify-between max-w-sm w-full">
            <div className="flex flex-col">
              {/* Logo SVG */}
              <SolaceUILogo className="text-white w-9 h-auto shrink-0 mb-2" />
              <h2 className="text-white text-xl md:text-[22px] font-medium leading-tight">
                Ship Tastefully Crafted<br />Marketing Pages
              </h2>
            </div>

            <div className="flex flex-col gap-3 mt-12 lg:mt-auto pt-8">
              {/* Social Icons SVG from reference */}
              <svg width="312" height="34" viewBox="0 0 312 34" fill="none" xmlns="http://www.w3.org/2000/svg" className="shrink-0 w-[180px] md:w-[200px] h-auto">
                <path d="M12.673 20.703L18.427 28.375H26.885L17.39 15.714L25.29 6.625H22.088L15.905 13.737L10.573 6.625H2.115L11.189 18.727L2.803 28.375H6.005L12.673 20.703ZM19.635 25.958L6.948 9.042H9.364L22.052 25.958H19.635Z" fill="#FFFFFF" />
                <path d="M68.75 5.75C69.413 5.75 70.049 6.013 70.518 6.482C70.987 6.951 71.25 7.587 71.25 8.25V25.75C71.25 26.413 70.987 27.049 70.518 27.518C70.049 27.987 69.413 28.25 68.75 28.25H51.25C50.587 28.25 49.951 27.987 49.482 27.518C49.013 27.049 48.75 26.413 48.75 25.75V8.25C48.75 7.587 49.013 6.951 49.482 6.482C49.951 6.013 50.587 5.75 51.25 5.75H68.75ZM68.125 25.125V18.5C68.125 17.419 67.696 16.383 66.931 15.618C66.167 14.854 65.131 14.425 64.05 14.425C62.987 14.425 61.75 15.075 61.15 16.05V14.662H57.663V25.125H61.15V18.962C61.15 18 61.925 17.212 62.888 17.212C63.352 17.212 63.797 17.397 64.125 17.725C64.453 18.053 64.638 18.498 64.638 18.962V25.125H68.125ZM53.6 12.7C54.157 12.7 54.691 12.479 55.085 12.085C55.479 11.691 55.7 11.157 55.7 10.6C55.7 9.438 54.763 8.488 53.6 8.488C53.04 8.488 52.502 8.71 52.106 9.106C51.71 9.502 51.487 10.04 51.487 10.6C51.487 11.762 52.438 12.7 53.6 12.7ZM55.337 25.125V14.662H51.875V25.125H55.337Z" fill="#FFFFFF" />
                <path d="M124.167 17C124.167 9.18 117.82 2.833 110 2.833C102.18 2.833 95.833 9.18 95.833 17C95.833 23.857 100.707 29.566 107.167 30.883V21.25H104.333V17H107.167V13.458C107.167 10.724 109.391 8.5 112.125 8.5H115.667V12.75H112.833C112.054 12.75 111.417 13.387 111.417 14.167V17H115.667V21.25H111.417V31.096C118.571 30.387 124.167 24.352 124.167 17Z" fill="#FFFFFF" />
                <path d="M167.458 12.922C165.619 6.078 159.292 6.506 159.292 6.506C159.292 6.506 150.542 5.923 150.542 17C150.542 28.078 159.292 27.495 159.292 27.495C159.292 27.495 164.493 27.841 166.875 22.924C167.653 20.757 167.458 16.422 159.875 16.422C159.875 16.422 156.375 16.422 156.375 19.339C156.375 20.478 157.542 21.672 159.292 21.672C161.042 21.672 162.991 20.474 163.375 18.172C164.542 11.172 158.125 10.589 156.375 13.506" stroke="#FFFFFF" strokeLinecap="round" strokeLinejoin="round" />
                <path d="M200.1 6.333H209.9C213.633 6.333 216.667 9.367 216.667 13.1V22.9C216.667 24.695 215.954 26.416 214.685 27.685C213.416 28.954 211.695 29.667 209.9 29.667H200.1C196.367 29.667 193.333 26.633 193.333 22.9V13.1C193.333 11.305 194.046 9.584 195.315 8.315C196.584 7.046 198.305 6.333 200.1 6.333ZM199.867 8.667C198.753 8.667 197.684 9.109 196.897 9.897C196.109 10.684 195.667 11.753 195.667 12.867V23.133C195.667 25.455 197.545 27.333 199.867 27.333H210.133C211.247 27.333 212.315 26.891 213.103 26.103C213.891 25.316 214.333 24.247 214.333 23.133V12.867C214.333 10.545 212.455 8.667 210.133 8.667H199.867ZM211.125 10.417C211.512 10.417 211.883 10.57 212.156 10.844C212.43 11.117 212.583 11.488 212.583 11.875C212.583 12.262 212.43 12.633 212.156 12.906C211.883 13.18 211.512 13.333 211.125 13.333C210.738 13.333 210.367 13.18 210.094 12.906C209.82 12.633 209.667 12.262 209.667 11.875C209.667 11.488 209.82 11.117 210.094 10.844C210.367 10.57 210.738 10.417 211.125 10.417ZM205 12.167C206.547 12.167 208.031 12.781 209.125 13.875C210.219 14.969 210.833 16.453 210.833 18C210.833 19.547 210.219 21.031 209.125 22.125C208.031 23.219 206.547 23.833 205 23.833C203.453 23.833 201.969 23.219 200.875 22.125C199.781 21.031 199.167 19.547 199.167 18C199.167 16.453 199.781 14.969 200.875 13.875C201.969 12.781 203.453 12.167 205 12.167ZM205 14.5C204.072 14.5 203.181 14.869 202.525 15.525C201.869 16.181 201.5 17.072 201.5 18C201.5 18.928 201.869 19.819 202.525 20.475C203.181 21.131 204.072 21.5 205 21.5C205.928 21.5 206.818 21.131 207.475 20.475C208.131 19.819 208.5 18.928 208.5 18C208.5 17.072 208.131 16.181 207.475 15.525C206.818 14.869 205.928 14.5 205 14.5Z" fill="#FFFFFF" />
                <path d="M256.367 9.79C255.569 8.879 255.13 7.71 255.13 6.5H251.525V20.967C251.498 21.75 251.167 22.492 250.604 23.036C250.04 23.58 249.287 23.884 248.503 23.883C246.847 23.883 245.47 22.53 245.47 20.85C245.47 18.843 247.407 17.338 249.402 17.957V14.27C245.377 13.733 241.853 16.86 241.853 20.85C241.853 24.735 245.073 27.5 248.492 27.5C252.155 27.5 255.13 24.525 255.13 20.85V13.512C256.592 14.562 258.347 15.125 260.147 15.122V11.517C260.147 11.517 257.953 11.622 256.367 9.79Z" fill="#FFFFFF" />
                <path d="M311.209 8.588C310.88 7.356 309.909 6.385 308.677 6.055C306.443 5.457 297.484 5.457 297.484 5.457C297.484 5.457 288.525 5.457 286.291 6.055C285.059 6.385 284.088 7.356 283.758 8.588C283.159 10.822 283.159 15.484 283.159 15.484C283.159 15.484 283.159 20.145 283.758 22.379C284.088 23.612 285.059 24.583 286.291 24.912C288.525 25.511 297.484 25.511 297.484 25.511C297.484 25.511 306.443 25.511 308.677 24.912C309.909 24.583 310.88 23.612 311.209 22.379C311.808 20.145 311.808 15.484 311.808 15.484C311.808 15.484 311.808 10.822 311.209 8.588ZM294.619 19.781V11.186L302.062 15.484L294.619 19.781Z" fill="#FFFFFF" />
              </svg>
              <p className="font-light text-white/80 text-xs md:text-[13px] mt-1">
                © 2026 {companyName}, All rights reserved
              </p>
            </div>
          </div>

          {/* Right Side - Links */}
          <div className="flex gap-12 md:gap-24 flex-wrap lg:flex-nowrap">
            {footerLinks.map((section) => (
              <div key={section.title} className="flex flex-col gap-5">
                <h3 className="text-white font-semibold text-lg md:text-xl">
                  {section.title}
                </h3>
                <ul className="flex flex-col gap-3 md:gap-4">
                  {section.links.map((link) => (
                    <li key={link.name}>
                      <Link
                        href={link.href}
                        className="text-white/70 hover:text-white transition-colors text-sm md:text-[15px] font-medium"
                      >
                        {link.name}
                      </Link>
                    </li>
                  ))}
                </ul>
              </div>
            ))}
          </div>

        </div>
      </div>
    </footer>
  );
}

components/ui/social-cloud.tsx
"use client";
import React from "react";
import { cn } from "@/lib/utils";
import { motion, MotionProps } from "motion/react";

interface SocialIconProps extends React.SVGProps<SVGSVGElement> {}

export const XIcon = ({ className, ...props }: SocialIconProps) => (
  <motion.svg
    className={cn("h-5 w-5", className)}
    width="23"
    height="20"
    viewBox="0 0 23 20"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    whileHover={{ scale: 1.2, rotate: 5 }}
    whileTap={{ scale: 0.9 }}
    transition={{ type: "spring", stiffness: 400, damping: 17 }}
  >
    <path
      d="M9.46617 12.6219L14.625 19.5H22.2083L13.6955 8.14883L20.7783 0H17.9075L12.3641 6.3765L7.58333 0H0L8.13583 10.8496L0.6175 19.5H3.48833L9.46617 12.6219ZM15.7083 17.3333L4.33333 2.16667H6.5L17.875 17.3333H15.7083Z"
      fill="currentColor"
    />
  </motion.svg>
);

export const LinkedInIcon = ({ className, ...props }: SocialIconProps) => (
  <motion.svg
    className={cn("h-5 w-5", className)}
    width="23"
    height="23"
    viewBox="0 0 23 23"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    whileHover={{ scale: 1.2, rotate: -5 }}
    whileTap={{ scale: 0.9 }}
    transition={{ type: "spring", stiffness: 400, damping: 17 }}
  >
    <path
      d="M20 0C20.663 0 21.2989 0.263392 21.7678 0.732233C22.2366 1.20107 22.5 1.83696 22.5 2.5V20C22.5 20.663 22.2366 21.2989 21.7678 21.7678C21.2989 22.2366 20.663 22.5 20 22.5H2.5C1.83696 22.5 1.20107 22.2366 0.732233 21.7678C0.263392 21.2989 0 20.663 0 20V2.5C0 1.83696 0.263392 1.20107 0.732233 0.732233C1.20107 0.263392 1.83696 0 2.5 0H20ZM19.375 19.375V12.75C19.375 11.6692 18.9457 10.6328 18.1815 9.86854C17.4172 9.10433 16.3808 8.675 15.3 8.675C14.2375 8.675 13 9.325 12.4 10.3V8.9125H8.9125V19.375H12.4V13.2125C12.4 12.25 13.175 11.4625 14.1375 11.4625C14.6016 11.4625 15.0467 11.6469 15.3749 11.9751C15.7031 12.3033 15.8875 12.7484 15.8875 13.2125V19.375H19.375ZM4.85 6.95C5.40695 6.95 5.9411 6.72875 6.33492 6.33492C6.72875 5.9411 6.95 5.40695 6.95 4.85C6.95 3.6875 6.0125 2.7375 4.85 2.7375C4.28973 2.7375 3.75241 2.96007 3.35624 3.35624C2.96007 3.75241 2.7375 4.28973 2.7375 4.85C2.7375 6.0125 3.6875 6.95 4.85 6.95ZM6.5875 19.375V8.9125H3.125V19.375H6.5875Z"
      fill="currentColor"
    />
  </motion.svg>
);

export const FacebookIcon = ({ className, ...props }: SocialIconProps) => (
  <motion.svg
    className={cn("h-5 w-5", className)}
    width="29"
    height="29"
    viewBox="0 0 29 29"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    whileHover={{ scale: 1.2, rotate: 5 }}
    whileTap={{ scale: 0.9 }}
    transition={{ type: "spring", stiffness: 400, damping: 17 }}
  >
    <path
      d="M28.3333 14.1667C28.3333 6.34667 21.9867 0 14.1667 0C6.34667 0 0 6.34667 0 14.1667C0 21.0233 4.87333 26.7325 11.3333 28.05V18.4167H8.5V14.1667H11.3333V10.625C11.3333 7.89083 13.5575 5.66667 16.2917 5.66667H19.8333V9.91667H17C16.2208 9.91667 15.5833 10.5542 15.5833 11.3333V14.1667H19.8333V18.4167H15.5833V28.2625C22.7375 27.5542 28.3333 21.5192 28.3333 14.1667Z"
      fill="currentColor"
    />
  </motion.svg>
);

export const ThreadsIcon = ({ className, ...props }: SocialIconProps) => (
  <motion.svg
    className={cn("h-5 w-5", className)}
    width="18"
    height="22"
    viewBox="0 0 18 22"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    whileHover={{ scale: 1.2, rotate: -5 }}
    whileTap={{ scale: 0.9 }}
    transition={{ type: "spring", stiffness: 400, damping: 17 }}
  >
    <path
      d="M17.4167 6.92196C15.5768 0.0771293 9.25 0.505296 9.25 0.505296C9.25 0.505296 0.5 -0.0780373 0.5 10.9995C0.5 22.077 9.25 21.4948 9.25 21.4948C9.25 21.4948 14.451 21.8401 16.8333 16.9238C17.6115 14.7561 17.4167 10.422 9.83333 10.422C9.83333 10.422 6.33333 10.422 6.33333 13.3386C6.33333 14.4773 7.5 15.672 9.25 15.672C11 15.672 12.9495 14.4738 13.3333 12.172C14.5 5.17196 8.08333 4.58863 6.33333 7.5053"
      stroke="currentColor"
      strokeLinecap="round"
      strokeLinejoin="round"
    />
  </motion.svg>
);

export const InstagramIcon = ({ className, ...props }: SocialIconProps) => (
  <motion.svg
    className={cn("h-5 w-5", className)}
    width="24"
    height="24"
    viewBox="0 0 24 24"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    whileHover={{ scale: 1.2, rotate: 5 }}
    whileTap={{ scale: 0.9 }}
    transition={{ type: "spring", stiffness: 400, damping: 17 }}
  >
    <path
      d="M6.76667 0H16.5667C20.3 0 23.3333 3.03333 23.3333 6.76667V16.5667C23.3333 18.3613 22.6204 20.0824 21.3514 21.3514C20.0824 22.6204 18.3613 23.3333 16.5667 23.3333H6.76667C3.03333 23.3333 0 20.3 0 16.5667V6.76667C0 4.97204 0.712915 3.25091 1.98191 1.98191C3.25091 0.712915 4.97204 0 6.76667 0ZM6.53333 2.33333C5.41942 2.33333 4.35114 2.77583 3.56349 3.56349C2.77583 4.35114 2.33333 5.41942 2.33333 6.53333V16.8C2.33333 19.1217 4.21167 21 6.53333 21H16.8C17.9139 21 18.9822 20.5575 19.7699 19.7699C20.5575 18.9822 21 17.9139 21 16.8V6.53333C21 4.21167 19.1217 2.33333 16.8 2.33333H6.53333ZM17.7917 4.08333C18.1784 4.08333 18.5494 4.23698 18.8229 4.51047C19.0964 4.78396 19.25 5.15489 19.25 5.54167C19.25 5.92844 19.0964 6.29937 18.8229 6.57286C18.5494 6.84635 18.1784 7 17.7917 7C17.4049 7 17.034 6.84635 16.7605 6.57286C16.487 6.29937 16.3333 5.92844 16.3333 5.54167C16.3333 5.15489 16.487 4.78396 16.7605 4.51047C17.034 4.23698 17.4049 4.08333 17.7917 4.08333ZM11.6667 5.83333C13.2138 5.83333 14.6975 6.44792 15.7915 7.54188C16.8854 8.63584 17.5 10.1196 17.5 11.6667C17.5 13.2138 16.8854 14.6975 15.7915 15.7915C14.6975 16.8854 13.2138 17.5 11.6667 17.5C10.1196 17.5 8.63584 16.8854 7.54188 15.7915C6.44792 14.6975 5.83333 13.2138 5.83333 11.6667C5.83333 10.1196 6.44792 8.63584 7.54188 7.54188C8.63584 6.44792 10.1196 5.83333 11.6667 5.83333ZM11.6667 8.16667C10.7384 8.16667 9.84817 8.53542 9.19179 9.19179C8.53542 9.84817 8.16667 10.7384 8.16667 11.6667C8.16667 12.5949 8.53542 13.4852 9.19179 14.1415C9.84817 14.7979 10.7384 15.1667 11.6667 15.1667C12.5949 15.1667 13.4852 14.7979 14.1415 14.1415C14.7979 13.4852 15.1667 12.5949 15.1667 11.6667C15.1667 10.7384 14.7979 9.84817 14.1415 9.19179C13.4852 8.53542 12.5949 8.16667 11.6667 8.16667Z"
      fill="currentColor"
    />
  </motion.svg>
);

export const TikTokIcon = ({ className, ...props }: SocialIconProps) => (
  <motion.svg
    className={cn("h-5 w-5", className)}
    width="19"
    height="21"
    viewBox="0 0 19 21"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    whileHover={{ scale: 1.2, rotate: -5 }}
    whileTap={{ scale: 0.9 }}
    transition={{ type: "spring", stiffness: 400, damping: 17 }}
  >
    <path
      d="M14.5133 3.29C13.716 2.37945 13.2765 1.2103 13.2767 0H9.67167V14.4667C9.64444 15.2497 9.31411 15.9916 8.75036 16.5357C8.18661 17.0799 7.43353 17.3838 6.65 17.3833C4.99333 17.3833 3.61667 16.03 3.61667 14.35C3.61667 12.3433 5.55333 10.8383 7.54833 11.4567V7.77C3.52333 7.23333 0 10.36 0 14.35C0 18.235 3.22 21 6.63833 21C10.3017 21 13.2767 18.025 13.2767 14.35V7.01167C14.7385 8.06149 16.4936 8.62475 18.2933 8.62167V5.01667C18.2933 5.01667 16.1 5.12167 14.5133 3.29Z"
      fill="currentColor"
    />
  </motion.svg>
);

export const YouTubeIcon = ({ className, ...props }: SocialIconProps) => (
  <motion.svg
    className={cn("h-5 w-5", className)}
    width="29"
    height="21"
    viewBox="0 0 29 21"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
    whileHover={{ scale: 1.2, rotate: 5 }}
    whileTap={{ scale: 0.9 }}
    transition={{ type: "spring", stiffness: 400, damping: 17 }}
  >
    <path
      d="M28.0501 3.13163C27.7206 1.8991 26.7497 0.928222 25.5172 0.59876C23.2832 -1.51787e-07 14.3244 0 14.3244 0C14.3244 0 5.36561 -1.51787e-07 3.13164 0.59876C1.8991 0.928222 0.928222 1.8991 0.59876 3.13163C0 5.36561 0 10.0271 0 10.0271C0 10.0271 0 14.6886 0.59876 16.9225C0.928222 18.1551 1.8991 19.126 3.13164 19.4554C5.36561 20.0542 14.3244 20.0542 14.3244 20.0542C14.3244 20.0542 23.2832 20.0542 25.5172 19.4554C26.7497 19.126 27.7206 18.1551 28.0501 16.9225C28.6488 14.6886 28.6488 10.0271 28.6488 10.0271C28.6488 10.0271 28.6488 5.36561 28.0501 3.13163ZM11.4595 14.3244V5.72977L18.9025 10.0271L11.4595 14.3244Z"
      fill="currentColor"
    />
  </motion.svg>
);

interface SocialCloudProps
  extends
    Omit<React.HTMLAttributes<HTMLDivElement>, keyof MotionProps>,
    MotionProps {}

export function SocialCloud({ className, ...props }: SocialCloudProps) {
  return (
    <motion.div
      className={cn(
        "flex flex-wrap items-center gap-6 cursor-pointer",
        className,
      )}
      {...props}
    >
      <XIcon />
      <LinkedInIcon />
      <FacebookIcon />
      <ThreadsIcon />
      <InstagramIcon />
      <TikTokIcon />
      <YouTubeIcon />
    </motion.div>
  );
}

demo.tsx
import FooterSection5 from "@/components/ui/footer-section-5";

export default function Default() {
  return <FooterSection5 />;
}
```

Install NPM dependencies:
```bash
npm install @paper-design/shaders-react motion
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
