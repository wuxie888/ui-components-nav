<!-- Event Calendar · @arihantcodes_1f7b8c4d · https://21st.dev/@arihantcodes_1f7b8c4d/components/event-calendar
     license: unspecified · category: calendar
     Here is Event Calendar component -->

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
components/ui/demoevent.tsx
"use client";
import React from "react";
import { motion, AnimatePresence } from "framer-motion";
import { ChevronLeft, ChevronRight, Plus, X } from "lucide-react";
import { Button } from "@/components/ui/button";
import {
  Dialog,
  DialogContent,
  DialogHeader,
  DialogTitle,
} from "@/components/ui/dialog";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";
import Image from "next/image";
import { cn } from "@/lib/utils";
import {
  add,
  eachDayOfInterval,
  endOfMonth,
  format,
  isEqual,
  isSameMonth,
  isToday,
  parse,
  startOfMonth,
  startOfWeek,
  endOfWeek,
} from "date-fns";

interface Subscription {
  id: string;
  name: string;
  date: number;
  icon: string;
  color: string;
}
interface SubscriptionDay {
  date: Date;
  subscriptions: Subscription[];
  isCurrentMonth: boolean;
}
function EventCalendar() {
  const [subscriptions, setSubscriptions] = React.useState<Subscription[]>([]);
  const [currentMonth, setCurrentMonth] = React.useState(
    format(new Date(), "MMM-yyyy"),
  );
  const [isAddModalOpen, setIsAddModalOpen] = React.useState(false);
  const firstDayCurrentMonth = parse(currentMonth, "MMM-yyyy", new Date());
  const days = React.useMemo(() => {
    const start = startOfWeek(startOfMonth(firstDayCurrentMonth));
    const end = endOfWeek(endOfMonth(firstDayCurrentMonth));
    return eachDayOfInterval({ start, end }).map(
      (day): SubscriptionDay => ({
        date: day,
        subscriptions: subscriptions.filter(
          (subscription) => subscription.date === day.getDate(),
        ),
        isCurrentMonth: isSameMonth(day, firstDayCurrentMonth),
      }),
    );
  }, [firstDayCurrentMonth, subscriptions]);
  function previousMonth() {
    const firstDayNextMonth = add(firstDayCurrentMonth, { months: -1 });
    setCurrentMonth(format(firstDayNextMonth, "MMM-yyyy"));
  }
  function nextMonth() {
    const firstDayNextMonth = add(firstDayCurrentMonth, { months: 1 });
    setCurrentMonth(format(firstDayNextMonth, "MMM-yyyy"));
  }
  const handleAddSubscription = (newSubscription: Omit<Subscription, "id">) => {
    const subscription = { ...newSubscription, id: Date.now().toString() };
    setSubscriptions([...subscriptions, subscription]);
  };
  const handleRemoveSubscription = (id: string) => {
    setSubscriptions(subscriptions.filter((sub) => sub.id !== id));
  };
  return (
    <div className="p-4 mx-auto max-w-3xl">
      <div className="flex items-center justify-between mb-4">
        <div className="flex items-center gap-4">
          <Button
            variant="outline"
            className="p-2 opacity-75 hover:opacity-100"
            onClick={previousMonth}
          >
            <ChevronLeft className="w-4 h-4" />
          </Button>
          <Button
            variant="outline"
            className="p-2 opacity-75 hover:opacity-100"
            onClick={nextMonth}
          >
            <ChevronRight className="w-4 h-4" />
          </Button>
          <motion.h2
            key={currentMonth}
            initial={{ opacity: 0, y: -20 }}
            animate={{ opacity: 1, y: 0 }}
            className="text-xl font-semibold"
          >
            {format(firstDayCurrentMonth, "MMMM yyyy")}
          </motion.h2>
        </div>
        <Button onClick={() => setIsAddModalOpen(true)}>
          <Plus className="w-4 h-4 mr-2" />
          Add Event
        </Button>
      </div>
      <div className="grid grid-cols-7 gap-px bg-muted rounded-lg overflow-hidden">
        <AnimatePresence mode="wait">
          {["SUN", "MON", "TUE", "WED", "THU", "FRI", "SAT"].map((day) => (
            <motion.div
              key={day}
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              className="p-2 text-center text-sm font-medium bg-background"
            >
              {day}
            </motion.div>
          ))}
          {days.map((day, dayIdx) => (
            <motion.div
              key={format(day.date, "yyyy-MM-dd")}
              initial={{ opacity: 0, scale: 0.95 }}
              animate={{ opacity: 1, scale: 1 }}
              transition={{ delay: dayIdx * 0.02 }}
              className={cn(
                "relative p-2 bg-background min-h-[100px]",
                !day.isCurrentMonth && "bg-muted/50",
                isEqual(day.date, new Date()) && "bg-accent",
              )}
            >
              <time
                dateTime={format(day.date, "yyyy-MM-dd")}
                className={cn(
                  "text-sm",
                  isToday(day.date) && "font-semibold text-primary",
                  !day.isCurrentMonth && "text-muted-foreground",
                )}
              >
                {format(day.date, "d")}
              </time>
              <div className="space-y-1 mt-1">
                {day.subscriptions.map((subscription) => (
                  <motion.div
                    key={subscription.id}
                    whileHover={{ scale: 1.05 }}
                    className="flex items-center gap-1 p-1 rounded bg-background border text-sm group"
                    style={{ borderColor: subscription.color }}
                  >
                    <div className="relative w-4 h-4">
                      <Image
                        src={subscription.icon}
                        alt={subscription.name}
                        className="rounded-sm object-cover"
                        fill
                      />
                    </div>
                    <span className="text-xs truncate flex-1">
                      {subscription.name}
                    </span>
                    <button
                      onClick={() => handleRemoveSubscription(subscription.id)}
                      className="opacity-0 group-hover:opacity-100 transition-opacity"
                    >
                      <X className="w-3 h-3" />
                    </button>
                  </motion.div>
                ))}
              </div>
            </motion.div>
          ))}
        </AnimatePresence>
      </div>
      <Dialog open={isAddModalOpen} onOpenChange={setIsAddModalOpen}>
        <DialogContent>
          <DialogHeader>
            <DialogTitle>Add New Event</DialogTitle>
          </DialogHeader>
          <form
            onSubmit={(e) => {
              e.preventDefault();
              const formData = new FormData(e.currentTarget);
              const name = formData.get("name") as string;
              const date = parseInt(formData.get("date") as string);
              const icon = formData.get("icon") as string;
              const color = formData.get("color") as string;
              handleAddSubscription({ name, date, icon, color });
              setIsAddModalOpen(false);
            }}
            className="space-y-4"
          >
            <div>
              <Label htmlFor="name">Event Name</Label>
              <Input placeholder="Event Name" id="name" name="name" required />
            </div>
            <div>
              <Label htmlFor="date">Enter Only Date</Label>
              <Input
                id="date"
                name="date"
                placeholder="Ex - 12"
                type="number"
                min={1}
                max={31}
                required
              />
            </div>
            <div>
              <Label htmlFor="icon">Icon URL</Label>
              <Input placeholder="Icon URL" id="icon" name="icon" required />
            </div>
            <div>
              <Label htmlFor="color">Color</Label>
              <Input id="color" name="color" type="color" required />
            </div>
            <Button type="submit">Add Event</Button>
          </form>
        </DialogContent>
      </Dialog>
    </div>
  );
}

export default EventCalendar;
```

Install NPM dependencies:
```bash
npm install date-fns framer-motion lucide-react
```

Install shadcn/ui registry dependencies:
```bash
npx shadcn@latest add button dialog input label
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
