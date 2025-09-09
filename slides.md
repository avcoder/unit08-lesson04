---
# You can also start simply with 'default'
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /assets/intro.jpg
# some information about your slides (markdown enabled)
title: Software Development | Foundations
info: |
  ## Software Development | Foundations
# apply unocss classes to the current slide
class: text-left
drawings:
  persist: false
transition: slide-left
mdc: true
---

# React Native
Mobile Development: Unit 08 - Lesson 04

- [ ] Timer, Countdowns
- [ ] Confetti, R.N. "fun" packages 
- [ ] Guest Speaker, R.N. UI frameworks

<div class="abs-br m-6 text-xl">
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
-->

---
transition: slide-left
---

# Recap

- We will have a guest speaker tonight. When Bill arrives around 8pm EST, please turn on your cameras
- The reason our notification didn't work last time was we forgot [one thing](https://unit08-lesson03.netlify.app/22):
   ```tsx
   if (result === "granted") {
         Notifications.setNotificationHandler({
         handleNotification: async () => ({
            shouldShowBanner: true,
            shouldShowList: true,
            shouldPlaySound: true,
            shouldSetBadge: false,
         }),
      });
   ```
- see updated repo: https://github.com/avcoder/mobile-react-native-01
   - I changed folder structure due to anything you put in app folder will show up in tabs
   - I changed tsconfig so that the `@` symbol can be used in import paths where `@` = `src/`

---
transition: slide-left
---

# Countdown Timer

- in `counter/index.tsx`
   ```tsx
   export default function CounterScreen() {
      const [secondsElapsed, setSecondsElapsed] = useState(0);

      useEffect(() => {
         const intervalId = setInterval(() => {
            setSecondsElapsed((val) => val + 1);
         }, 1000);
         
         return () => {
            clearInterval(intervalId);
         };
      }, []);
   ...
   <Text>{secondsElapsed}</Text>
   ```
- Try it, does the timer work so far?
- fyi: `npx expo install <package-name>` installs the correct version of the package that is compatible with your Expo SDK version

---
transition: slide-left
---

# Common ways to calculate time in milliseconds

```js
const oneSecond = 1 * 1000;
const oneMinute = 60 * 1000;
const oneHour = 60 * 60 * 1000;
const oneDay = 24 * 60 * 60 * 1000;
```

- Measure elapsed time:
```ts
const start = Date.now(); // get current timestamp

// Do something...
setTimeout(() => {
  const end = Date.now();
  const elapsed = end - start; // in milliseconds
  console.log(`Elapsed time: ${elapsed} ms`);
}, 500);
```

- Human readable times
```ts
const date = new Date();       // current date/time
console.log(date.toISOString());     // "2025-09-08T15:00:00.000Z"
console.log(date.toLocaleString());  // '9/8/2025, 9:12:41 PM'
```
---
transition: slide-left
---

# Intl.DateTimeFormat
lets you format a Date object based on locale and options like time zone, date style, hour format, etc.

```ts
// Basic Usage
const date = new Date(); 
const formatter = new Intl.DateTimeFormat('en-US'); 
console.log(formatter.format(date));  // e.g. "9/8/2025"

// Formatting with Locale and Time Zone
const formatter = new Intl.DateTimeFormat('en-GB', { 
  timeZone: 'Europe/London',
  dateStyle: 'full',
  timeStyle: 'long'
});
console.log(formatter.format(new Date()));

// Show only time
const timeFormatter = new Intl.DateTimeFormat('en-US', {
  hour: 'numeric',
  minute: 'numeric',
  second: 'numeric',
  hour12: false
});
console.log(timeFormatter.format(new Date()));  // ex "16:45:22"

```

---
transition: slide-left
---

# Install date-fns

- `npx expo install date-fns`
   ```tsx
   import { Text, View, StyleSheet, TextStyle } from "react-native";
                              // create this in new file: `components/TimeSegment.tsx`
   type Props = {
      number: number;
      unit: string;
      textStyle?: TextStyle; // ensures we're not using invalid styles
   };

   export function TimeSegment({ number, unit, textStyle }: Props) {
      return (
         <View style={styles.segmentContainer}>
            <Text style={[styles.number, textStyle]}>{number}</Text>
            <Text style={textStyle}>{unit}</Text>
         </View>
      );
   }

   const styles = StyleSheet.create({
      segmentContainer: {}, // fill in whatever styles you want
      number: {},
   });
   ```

---
transition: slide-left
---

# Counter (pg.1)

- in `/counter/index.tsx`
- remove any `secondsElapsed` code
```tsx
import { type Duration } from "date-fns";
...
// 5 seconds from now
const timestamp = Date.now() + (5 * 1000);

type CountdownStatus = {
  isOverdue: boolean;
  distance: Duration; 
};
...
const [status, setStatus] = useState<CountdownStatus>({
    isOverdue: false,
    distance: {},
});
```
- take a peak at the TS definition for `Duration`

---
transition: slide-left
---

# Counter (pg.2)

```tsx
import { ... intervalToDuration, isBefore, } from "date-fns";
...
useEffect(() => {
     const intervalId = setInterval(() => {
      // is the timestamp before right now?  trying to determine if we are dueTime occurred
      const isOverdue = isBefore(timestamp, Date.now());

      // calculate number of seconds to dueTime or from dueTime
      const distance = intervalToDuration(
        isOverdue
          ? { start: timestamp, end: Date.now() }
          : { start: Date.now(), end: timestamp },
      );

      setStatus({ isOverdue, distance });
     }, 1000);
```

- after our useState, run `console.log(status)` - does our countdown timer work?

---
transition: slide-left
---

# Exercise: Counter (pg.3)

- Now that we have the logic built for our countdown timer, Build a UI for it so it uses our `TimeSegment` component to display the Seconds left via `status.distance?.seconds`
- change font-color and background-color styles if overdue
- Re-use the TimeSegment component to additionally display the:
   - minutes left
   - hours left

```tsx
{!status.isOverdue ? (
   <Text style={[styles.heading]}>Due in</Text>
) : (
   <Text style={[styles.heading, styles.whiteText]}>Overdue by</Text>
)}
...
   <TimeSegment
     unit="Seconds"
     number={status.distance?.seconds ?? 0}
     textStyle={status.isOverdue ? styles.whiteText : undefined}
   />
```

---
transition: slide-left
---

# Confetti


---
layout: image-right
transition: slide-left
image: /assets/dan.png
backgroundSize: 444px 300px
class: text-left
---

# 10 minute break

🍦 Cool Tips, Trends and Resources:
- 🪏 [Why we ditched Next.js](https://northflank.com/blog/why-we-ditched-next-js-and-never-looked-back)
- ⚡ [content-visibility: auto](https://cekrem.github.io/posts/content-visibility-auto-performance/)
- 🎭 [React for Two Computers](https://www.youtube.com/watch?v=ozI4V_29fj4)
- ⚛️ [JSX Over the Wire = RSC](https://overreacted.io/jsx-over-the-wire/)
- 🍎 [new MAC setup](https://www.swyx.io/new-mac-setup)


<br>
<hr>
<br>

- 🧪 [Enter anonymous lab questions](https://docs.google.com/forms/d/e/1FAIpQLSevvGARdHQikso-uLqFCO481MABKE5HofuSrlzEPMNQ2ZLykw/viewform?usp=dialog)
- ℹ️ [Course feedback survey](https://circuitstream.typeform.com/to/ZoyYk7px#course_id=SoftwareAN&instructor=9514)

---
transition: slide-left
---

# Guest Speaker: Bill Mitchell

- When Bill arrives around 8pm EST, please turn on your cameras
- check out his work experience: https://www.linkedin.com/in/wkmitch/
- He has graciously given us 20 to 30 minutes of Q&A
- Ask him anything 

---
transition: slide-left
---

# Exercise: UI frameworks for React-Native

- Create a new React-Native project but try installing and playing with:
   - https://reactnativepaper.com/
   - https://reactnativeelements.com/

---
transition: slide-left
---

# Homework

- Start working on your Mobile Assignment 
- Start working on your Capstone planning project