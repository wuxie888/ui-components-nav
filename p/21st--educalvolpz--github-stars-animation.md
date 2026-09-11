<!-- GitHub Stars Animation · @educalvolpz · https://21st.dev/@educalvolpz/components/github-stars-animation
     license: MIT · category: hero
     Displays GitHub stargazer avatars with a staggered entrance and an animated count-up star counter for hero and landing sections. -->

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

import { cn } from "@/lib/utils";
import { Star } from "lucide-react";
import { motion, useReducedMotion, useSpring } from "motion/react";
import { useEffect, useState } from "react";

const TRANSITION_DURATION = 0.3;
const EASE_OUT_CUBIC = [0.215, 0.61, 0.355, 1] as const;
const COUNTDOWN_DURATION = 2000;
const AVATAR_COUNT = 5;
const STAGGER_DELAY = 0.05;

export interface Stargazer {
  avatar_url: string;
  html_url: string;
  login: string;
}

export interface GitHubStarsAnimationProps {
  apiEndpoint?: string;
  avatarClassName?: string;
  className?: string;
  countClassName?: string;
  maxAvatars?: number;
  owner?: string;
  repo?: string;
  showAvatars?: boolean;
  starCount?: number;
  stargazers?: Stargazer[];
}

export default function GitHubStarsAnimation({
  owner = "educlopez",
  repo = "smoothui",
  stargazers: providedStargazers,
  starCount: providedStarCount,
  apiEndpoint,
  className = "",
  avatarClassName = "",
  countClassName = "",
  showAvatars = true,
  maxAvatars = AVATAR_COUNT,
}: GitHubStarsAnimationProps) {
  const [stargazers, setStargazers] = useState<Stargazer[]>(
    providedStargazers || []
  );
  const [starCount, setStarCount] = useState(providedStarCount || 0);
  const [displayCount, setDisplayCount] = useState(0);
  const [isLoading, setIsLoading] = useState(!providedStargazers);
  const [error, setError] = useState(false);
  const shouldReduceMotion = useReducedMotion();

  const countSpring = useSpring(0, {
    damping: 30,
    stiffness: 100,
  });

  // Fetch stargazers and star count
  useEffect(() => {
    if (providedStargazers && providedStarCount !== undefined) {
      setStargazers(providedStargazers);
      setStarCount(providedStarCount);
      setIsLoading(false);
      return;
    }

    const fetchData = async () => {
      try {
        setIsLoading(true);
        setError(false);

        // Try to fetch from custom API endpoint first
        if (apiEndpoint) {
          const response = await fetch(
            `${apiEndpoint}?owner=${owner}&repo=${repo}`
          );
          if (response.ok) {
            const data = await response.json();
            if (data.stargazers) {
              setStargazers(data.stargazers.slice(0, maxAvatars));
            }
            if (data.stars !== undefined) {
              setStarCount(data.stars);
            }
            setIsLoading(false);
            return;
          }
        }

        // Fallback to GitHub API directly (client-side)
        // Note: This has rate limits, so using a token is recommended
        const headers: HeadersInit = {
          Accept: "application/vnd.github.v3+json",
        };

        // Parallelize independent fetches to eliminate waterfall
        const [repoResponse, stargazersResponse] = await Promise.all([
          fetch(`https://api.github.com/repos/${owner}/${repo}`, { headers }),
          fetch(
            `https://api.github.com/repos/${owner}/${repo}/stargazers?per_page=${maxAvatars}`,
            { headers }
          ),
        ]);

        // Process repo info for star count
        if (repoResponse.ok) {
          try {
            const repoData = await repoResponse.json();
            setStarCount(repoData.stargazers_count || 0);
          } catch {
            // Silently fail for star count
          }
        }

        // Process stargazers
        if (stargazersResponse.ok) {
          try {
            const stargazersData =
              (await stargazersResponse.json()) as Stargazer[];
            setStargazers(stargazersData.slice(0, maxAvatars));
          } catch {
            // Silently fail for stargazers
          }
        }
      } catch {
        setError(true);
      } finally {
        setIsLoading(false);
      }
    };

    fetchData();
  }, [
    owner,
    repo,
    apiEndpoint,
    maxAvatars,
    providedStargazers,
    providedStarCount,
  ]);

  // Animate countdown
  useEffect(() => {
    if (starCount === 0 || shouldReduceMotion) {
      if (shouldReduceMotion) {
        setDisplayCount(starCount);
        countSpring.set(starCount);
      }
      return;
    }

    const startTime = Date.now();
    const startValue = 0;
    const endValue = starCount;

    const animate = () => {
      const elapsed = Date.now() - startTime;
      const progress = Math.min(elapsed / COUNTDOWN_DURATION, 1);

      // Ease-out function
      const eased = 1 - (1 - progress) ** 3;
      const current = Math.floor(startValue + (endValue - startValue) * eased);

      setDisplayCount(current);
      countSpring.set(current);

      if (progress < 1) {
        requestAnimationFrame(animate);
      } else {
        setDisplayCount(endValue);
        countSpring.set(endValue);
      }
    };

    animate();
  }, [starCount, countSpring, shouldReduceMotion]);

  if (isLoading) {
    return (
      <div
        className={cn("flex items-center gap-3 text-foreground/60", className)}
      >
        <div className="h-10 w-10 animate-pulse rounded-full bg-foreground/20" />
        <div className="h-6 w-20 animate-pulse rounded bg-foreground/20" />
      </div>
    );
  }

  if (error && starCount === 0) {
    return null;
  }

  const visibleAvatars = stargazers.slice(0, maxAvatars);

  return (
    <div className={cn("flex items-center gap-3", className)}>
      {/* Avatars */}
      {showAvatars && visibleAvatars.length > 0 && (
        <div className="relative flex items-center">
          {visibleAvatars.map((stargazer, index) => (
            <motion.a
              animate={
                shouldReduceMotion
                  ? { opacity: 1 }
                  : {
                      opacity: 1,
                      scale: 1,
                      x: 0,
                    }
              }
              aria-label={`${stargazer.login}'s GitHub profile`}
              className={cn(
                "relative z-10 h-10 w-10 overflow-hidden rounded-full border-2 border-background bg-background transition-transform hover:z-20 hover:scale-110",
                avatarClassName
              )}
              href={stargazer.html_url}
              initial={
                shouldReduceMotion
                  ? { opacity: 1 }
                  : {
                      opacity: 0,
                      scale: 0.8,
                      x: -20,
                    }
              }
              key={stargazer.login}
              rel="noopener noreferrer"
              style={{
                marginLeft: index > 0 ? "-8px" : "0",
              }}
              target="_blank"
              transition={
                shouldReduceMotion
                  ? { duration: 0 }
                  : {
                      delay: index * STAGGER_DELAY,
                      duration: TRANSITION_DURATION,
                      ease: EASE_OUT_CUBIC,
                    }
              }
              whileHover={shouldReduceMotion ? {} : { scale: 1.1, zIndex: 20 }}
            >
              <img
                alt={`${stargazer.login}'s avatar`}
                className="h-full w-full object-cover"
                draggable={false}
                src={stargazer.avatar_url}
              />
            </motion.a>
          ))}
        </div>
      )}

      {/* Star count */}
      <motion.div
        animate={shouldReduceMotion ? { opacity: 1 } : { opacity: 1, scale: 1 }}
        className={cn("flex items-center gap-1.5 font-medium", countClassName)}
        initial={
          shouldReduceMotion ? { opacity: 1 } : { opacity: 0, scale: 0.9 }
        }
        transition={
          shouldReduceMotion
            ? { duration: 0 }
            : {
                duration: TRANSITION_DURATION,
                ease: EASE_OUT_CUBIC,
              }
        }
      >
        <Star className="h-4 w-4 fill-current" />
        <motion.span
          animate={shouldReduceMotion ? { scale: 1 } : { scale: [1, 1.1, 1] }}
          className="tabular-nums"
          transition={
            shouldReduceMotion
              ? { duration: 0 }
              : {
                  duration: 0.3,
                  ease: EASE_OUT_CUBIC,
                }
          }
        >
          {displayCount.toLocaleString()}
        </motion.span>
        <span className="text-foreground/70 text-sm">
          {displayCount === 1 ? "star" : "stars"}
        </span>
      </motion.div>
    </div>
  );
}

demo.tsx
"use client";

import GitHubStarsAnimation from "@/components/ui/github-stars-animation";

const stargazers = [
  {
    login: "educlopez",
    html_url: "https://github.com/educlopez",
    avatar_url:
      "data:image/jpeg;base64,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",
  },
  {
    login: "DimaDevelopment",
    html_url: "https://github.com/DimaDevelopment",
    avatar_url:
      "data:image/jpeg;base64,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",
  },
];

export default function GitHubStarsAnimationDemo() {
  return (
    <div className="flex min-h-[200px] flex-col items-center justify-center gap-8">
      <GitHubStarsAnimation
        maxAvatars={5}
        owner="educlopez"
        repo="smoothui"
        showAvatars
        starCount={470}
        stargazers={stargazers}
      />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install lucide-react motion
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
