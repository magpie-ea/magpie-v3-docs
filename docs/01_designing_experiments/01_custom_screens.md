# Custom screens
When the built-in screens are not enough for your purposes, you can define your own custom screens.

For this purpose there is an abstract `<Screen>` component available with _magpie.

## Slides

As mentioned in the "Basics" section,
a screen consists of one or more slides, which are displayed one after the other. The code for a basic screen would then
look like this:

```html
<Screen>
    <template #0>
    Hello World
    </template>
</Screen>
```

This screen has exactly one slide, which displays "Hello world". Notice that we use numbered slots to define our slides.
If you would like to have another slide, you would need to use slot 1:

```html
<Screen>
    <template #0>
    Hello World
    </template>

    <template #1>
        Anybody here?
    </template>
</Screen>
```

The problem now is that there is no indication when to switch to the second slide. So, we will stay on slide 0 forever.

## nextSlide

However, _magpie lends us a hand here, and provides a function to go to the next slide:

```html
<Screen>
    <template #0="{ nextSlide }">
    Hello World
    </template>

    <template #1>
        Anybody here?
    </template>
</Screen>
```

Here, we obtain the `nextSlide` function from the Screen component, so we can use it in our first slide. We can now use it in an event handler for example.

```html
<Screen>
    <template #0="{ nextSlide }">
        Hello World
        <button @click="nextSlide">Next slide</button>
    </template>

    <template #1>
        Anybody here?
    </template>
</Screen>
```

Now our participants can click on the button to go to the next slide. We can also use a helper component to trigger
this automatically after some time:

```html
<Screen>
    <template #0="{ nextSlide }">
        Hello World
        <Wait :time="1000" @done="nextSlide" />
    </template>

    <template #1>
        Anybody here?
    </template>
</Screen>
```

This invisible component will wait one second and then call nextSlide for us, so effectively the first slide is only shown for one second.

Now, we're stuck on the second slide, however. How do we jump to the next screen?

## nextScreen
It turns out, there is a function for this as well:

```html
<Screen>
    <template #0="{ nextSlide }">
        Hello World
        <Wait :time="1000" @done="nextSlide" />
    </template>

    <template #1="{ nextScreen }">
        Anybody here?
        <button @click="nextScreen">Yes</button>
    </template>
</Screen>
```

After 1 second of displaying "Hello world", we display "Anybody here?" and when the participant clicks the "Yes" button,
we go to the next screen. Notice, that you have to inject the slot parameters you need (like `nextScreen`) in every slide
separately.

## Collecting data
Experiments are of course about collecting data, so we need some inputs to collect responses from our participants.

_magpie has some built-in input components ready for you. For example, you could use the RatingInput component to realize
a rating task:

```html
<Screen>
    <template #0>
        <RatingInput
            :right="very cute"
            :left="very appalling"/>
    </template>
</Screen>
```

This will display a 7-step rating going from "very appalling" to "very cute". To store the response somewhere, we can
inject the `measurements` object from the Screen component:

```html
<Screen>
    <template #0="{ measurements }">
        <RatingInput
            :right="very cute"
            :left="very appalling"
            :response.sync="measurements.rating" />
    </template>
</Screen>
```

Once, injected, we can a property of the measurements object to the `response` prop of the RatingInput. The magic here
is in the `.sync` suffix. This will make sure, that the assignment is two-way: If the participant changes their response,
`measurements.rating` will be changed automatically to always reflect the latest value.

To save all our measurements made in the current screen, we then use `save`:

```html
<Screen>
    <template #0="{ measurements, save, nextScreen }">
        <RatingInput
            :right="very cute"
            :left="very appalling"
            :response.sync="measurements.rating" />
        <button @click="save(); nextScreen()">Submit</button>
    </template>
</Screen>
```

However, as writing and injecting both functions all the time is cumbersome, there's a combined version of the two:

```html
<Screen>
    <template #0="{ measurements, saveAndNextScreen }">
        <RatingInput
            :right="very cute"
            :left="very appalling"
            :response.sync="measurements.rating" />
        <button @click="saveAndNextScreen">Submit</button>
    </template>
</Screen>
```

Once, the participant clicks "Submit", their rating will be stored in the data set for the current screen.

This architecture makes it possible to collect multiple responses per screen and even per slide:

```html
<Screen>
    <template #0="{ measurements, saveAndNextScreen }">
        <RatingInput
            :right="very cute"
            :left="very appalling"
            :response.sync="measurements.cuteness" />
        <RatingInput
            :right="very large"
            :left="very small"
            :response.sync="measurements.size" />
        <RatingInput
            :right="very interesting"
            :left="very boring"
            :response.sync="measurements.interest" />
    
        <button @click="saveAndNextScreen">Submit</button>
    </template>
</Screen>
```
