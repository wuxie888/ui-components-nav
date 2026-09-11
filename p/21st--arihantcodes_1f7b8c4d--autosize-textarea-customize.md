<!-- Autosize Textarea Customize · @arihantcodes_1f7b8c4d · https://21st.dev/@arihantcodes_1f7b8c4d/components/autosize-textarea-customize
     license: MIT · category: form
     A bio form field with an auto-resizing textarea, Zod validation, and a loading submit button. -->

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
components/ui/autosize-textarea-customize.tsx
"use client";

import { zodResolver } from "@hookform/resolvers/zod";
import { useForm } from "react-hook-form";
import * as z from "zod";
import * as React from "react";
import {
  Form,
  FormControl,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from "@/components/ui/form";
import { Textarea } from "@/components/ui/textarea";
import { toast } from "@/components/ui/use-toast";
import { useAutosizeTextArea } from "@/components/ui/autosize-textarea";
import { LoadingButton } from "@/components/ui/loading-button";

const FormSchema = z.object({
  bio: z
    .string()
    .min(10, {
      message: "Bio must be at least 10 characters.",
    })
    .max(160, {
      message: "Bio must not be longer than 30 characters.",
    }),
});

const AutosizeTextareaCustomize = () => {
  const form = useForm<z.infer<typeof FormSchema>>({
    resolver: zodResolver(FormSchema),
  });

  const [loading, setLoading] = React.useState(false);

  const textAreaRef = React.useRef<HTMLTextAreaElement>(null);
  const [triggerAutoSize, setTriggerAutoSize] = React.useState("");
  useAutosizeTextArea({
    textAreaRef: textAreaRef?.current,
    triggerAutoSize: triggerAutoSize,
    minHeight: 52,
    maxHeight: 200,
  });

  /** You can use `form.watch` to trigger auto sizing. */
  const bio = form.watch("bio");
  React.useEffect(() => {
    if (textAreaRef.current) {
      setTriggerAutoSize(bio);
    }
  }, [bio]);

  function onSubmit(data: z.infer<typeof FormSchema>) {
    setLoading(true);

    setTimeout(() => {
      setLoading(false);
      toast({
        title: "Your submitted data",
        description: (
          <pre className="mt-2 w-[340px] rounded-md bg-slate-950 p-4">
            <code className="text-white">{JSON.stringify(data, null, 2)}</code>
          </pre>
        ),
      });
    }, 500);
  }

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)} className="w-2/3 space-y-6">
        <FormField
          control={form.control}
          name="bio"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Bio</FormLabel>
              <FormControl>
                <Textarea
                  placeholder="Tell us a little bit about yourself"
                  {...field}
                  ref={textAreaRef}
                />
              </FormControl>
              <FormMessage />
            </FormItem>
          )}
        />
        <LoadingButton loading={loading} type="submit">
          Submit
        </LoadingButton>
      </form>
    </Form>
  );
};
export default AutosizeTextareaCustomize;

demo.tsx
import AutosizeTextareaCustomize from "@/components/ui/autosize-textarea-customize";

export default function Default() {
  return (
    <div className="flex w-full justify-center p-6">
      <AutosizeTextareaCustomize />
    </div>
  );
}
```

Install NPM dependencies:
```bash
npm install @hookform/resolvers react-hook-form sonner zod
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add autosize-textarea-dependecies form loading-button loading-button?api_key=21st_sk_5356a638ce7b27c9e1337a2db58cf9cf8bd3e395819fe545408f2a17d3ef2068&publisher_install_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwdXJwb3NlIjoicHVibGlzaGVyLXJlZ2lzdHJ5LWluc3RhbGwiLCJpYXQiOjE3ODg0NjE0MjksImV4cCI6MTc4ODQ2MjAyOX0.W5DbfKjgkrlz2JzgtKID83Y9ka6f6hs9uU6dsA0A_5A textarea
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
