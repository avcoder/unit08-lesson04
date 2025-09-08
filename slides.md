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
- [ ] Activity Indicator 
- [ ] History, Confetti

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

---
transition: slide-left
---

# Install date-fns

- `npx expo install date-fns`
- create new file: `components/TimeSegment.tsx`
   ```tsx
   import { Text, View, StyleSheet, TextStyle } from "react-native";

   type Props = {
      number: number;
      unit: string;
      textStyle?: TextStyle;
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

# Homework

- Start working on your Mobile Assignment 
- Start working on your Capstone planning project