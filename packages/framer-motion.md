# Framer Motion

## Install

```
npm i framer-motion
```

## Import to File

```ts
import { motion } from "framer-motion";
```

## Create a Motion Element

- Turn an element into a motion element by adding `motion.` to the element name.
  For example:

```html
<motion.div></div>
<motion.section></div>
```

- This is not possible to do with components in this way, so we must create a motion component.
- Do this by passing in the component as an argument to `motion()`. This returns the motion component.

```ts
const MotionLink = motion(Link);
```

- In NextJS this does mean that the component we're using this motion component in must be turned into a `client component` as motion uses Reacts `context hook` in the background - and we cannot use react hooks in server components.

## Framer Motion useScroll hook

- The useScroll hook gives us an indicator of how much we have scrolled according to a specified reference point.
- Used to create scroll based animations.
- Imports from framer-motion
- Rturns 4 motion values:

  - Absolute scroll positions in pixels for: `scrollX` and `scrollY`
  - Position between defined offsets as `scrollXProgress` and `scrollYProgress`

- We typically use a `ref` for the reference point.
- When we call `useScroll()` we pass in an object of options:
  - the `target` - or refrence point.
  - `offset` to change the starting point

```ts
const ref = useRef(null);
const { scrollYProgress } = useScroll({
    target: ref,
    offset: ['start end', 'end end'],
})

<MotionComponent ref={ref} />
```

**Offsets**

- offset is an array of at least 2 intersections.
- An intersection is a point when the target and container (or viewport with no container) meet.
- So `['start end]` above means when the start of the target meets the end of the container, and `['end end']` is where the end of the target meets the end of the container.
- Numbers can also be provided where `0` is the start and `1` is the end. So `['0 1']` means where the start of the target meets the end of the container.
- Can also use numbers outside of this range, so `['0 1', '1.5 1']` means the animation starts when the start of the target meets the end of the container and the animation should finish when 1.5 times the height of the target (from the start of the target) meets the end of the container (when the 1.5 times the height of the target has scrolled past the start point).
- Other accepted values are `percentages` and `vh` and `vw` values.

We can also offset the starting point by passing in an offset option.

```ts
const { scrollYProgress } = useScroll({
  target: ref,
  offset: ["0 1", "1.5 1"],
});
```

## useTransform()

- Taking our above offsets, we could then use `useTransform` to mape some values and alter how dramatically certain things are done.
- For example, if our `scrollYProgress` will be used to affect the scale and opacity of an element, we dont want to scale to be 0 when Y is 0.
- We do this by providing a `value`, an `input` and an `output`.
  - `useTransform(value, input, output, {options});`
- In this case:
  - the Value is `scrollYProgress`
  - input is the initial state of `[0,1]`
  - the output is whatever we want it to be.
- The mapping is done as follows:
  - `0` is the starting point and `1` is the end point in our input (`[0,1]`).
  - for our output we pass another array where the first index is the scale we want our element to start at, and the 2nd index is the value we want our scale to finish at.
- If we want our scale to start at 0.5, and end at 1, then our input would be `[0,1]` and our output `[0.5,1]`.

```ts
// when Y is at 0, scale should be 0.8, when Y is at 1, scale should be 1.
const scaleProgress = useTransform(scrollYProgress, [0, 1], [0.8, 1]);
// when Y is at 0, opacity should be 0.3, when Y is at 1, opacity should be 1.
const opacityProgress = useTransform(scrollYProgress, [0, 1], [0.3, 1]);
```
