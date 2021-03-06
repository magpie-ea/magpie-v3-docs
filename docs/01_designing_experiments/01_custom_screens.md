# Custom screens
When the built-in screens are not enough for your purposes, you can define your own custom screens.

For this purpose there is an abstract [Screen](https://reference.magpie-experiments.org/#screen) component available with magpie, along with a catalog of [input](https://reference.magpie-experiments.org/#inputs) and [stimulus](https://reference.magpie-experiments.org/#stimuli) components.
Magpie's `$magpie` API offers additional helpers for managing screens and slides.

## Slides

As mentioned in the "Basics" section,
a screen consists of one or more slides, which are displayed one after the other. The code for a basic screen would then
look like this:

```html
<Screen>
    <Slide>
    Hello World
    </Slide>
</Screen>
```

This screen has exactly one slide, which displays "Hello world".
If you would like to have another slide, you can simply add another `<Slide>` component next to the first. They are displayed
in the order they appear in the file.

```html
<Screen>
    <Slide>
        Hello World
    </Slide>

    <Slide>
        Anybody here?
    </Slide>
</Screen>
```

The problem now is that there is no indication when to switch to the second slide. So, we will stay on the first slide forever.

## Switching slides

However, magpie lends us a hand here, and provides a function to go to the next slide:

```html
<Screen>
    <Slide>
        Hello World
        <button @click="$magpie.nextSlide()">Next slide</button>
    </Slide>

    <Slide>
        Anybody here?
    </Slide>
</Screen>
```

Here, we call `$magpie.nextSlide()` in an event handler for the click event of our button.

Now our participants can click on the button to go to the next slide. We can also use a helper component to trigger
this automatically after some time:

```html
<Screen>
    <Slide>
        Hello World
        <Wait :time="1000" @done="$magpie.nextSlide()" />
    </Slide>

    <Slide>
        Anybody here?
    </Slide>
</Screen>
```

This invisible component will wait one second and then call `$magpie.nextSlide()` for us, so effectively the first slide is only shown for one second.

Now, we're stuck on the second slide, however. How do we jump to the next screen?

## Switching screens
It turns out, there is a function for this as well:

```html
<Screen>
    <Slide>
        Hello World
        <Wait :time="1000" @done="$magpie.nextSlide()" />
    </Slide>

    <Slide>
        Anybody here?
        <button @click="$magpie.nextScreen()">Yes</button>
    </Slide>
</Screen>
```

After 1 second of displaying "Hello world", we display "Anybody here?" and when the participant clicks the "Yes" button,
we go to the next screen.

## Collecting data
Of course, Experiments are about collecting data, so we need some inputs to collect responses from our participants.

Magpie has some built-in input components ready for you. For example, you could use the RatingInput component to realize
a rating task:

```html
<Screen>
    <Slide>
        <RatingInput
            :right="very cute"
            :left="very appalling"/>
    </Slide>
</Screen>
```

This will display a 7-step rating going from "very appalling" to "very cute". To store the response somewhere, we can
use the `$magpie.measurements` object, which holds custom variables:

```html
<Screen>
    <Slide>
        <RatingInput
            :right="very cute"
            :left="very appalling"
            :response.sync="$magpie.measurements.rating" />
    </Slide>
</Screen>
```

We can assign a custom property of the measurements object to the `response` prop of the RatingInput. The magic here
is in the `.sync` suffix. This will make sure, that the assignment is two-way: If the participant changes their response,
`$magpie.measurements.rating` will be changed automatically to always reflect the latest value.

To save all our measurements made in the current screen, we then use `$magpie.saveMeasurements`:

```html
<Screen>
    <Slide>
        <RatingInput
            :right="very cute"
            :left="very appalling"
            :response.sync="$magpie.measurements.rating" />
        <button @click="$magpie.saveMeasurements(); $magpie.nextScreen()">Submit</button>
    </Slide>
</Screen>
```

However, as writing both functions all the time is cumbersome, there's a combined version of the two:

```html
<Screen>
    <Slide>
        <RatingInput
            :right="very cute"
            :left="very appalling"
            :response.sync="$magpie.measurements.rating" />
        <button @click="$magpie.saveAndNextScreen()">Submit</button>
    </Slide>
</Screen>
```

Once, the participant clicks "Submit", their rating will be stored in the data set for the current screen.

This architecture makes it possible to collect multiple responses per screen and even per slide:

```html
<Screen>
    <Slide>
        <RatingInput
            :right="very cute"
            :left="very appalling"
            :response.sync="$magpie.measurements.cuteness" />
        <RatingInput
            :right="very large"
            :left="very small"
            :response.sync="$magpie.measurements.size" />
        <RatingInput
            :right="very interesting"
            :left="very boring"
            :response.sync="$magpie.measurements.interest" />
    
        <button @click="$magpie.saveAndNextScreen()">Submit</button>
    </Slide>
</Screen>
```

## Validating measurements
Imagine we want to record a participant-entered text in a screen. For this we can use the textarea input.

```html
<Screen>
    <Slide>
        <TextareaInput :response.sync="$magpie.measurements.text" />
    
        <button @click="$magpie.saveAndNextScreen()">Submit</button>
    </Slide>
</Screen>
```

However, we often have some specifications about how such input should look like. This is where validations come in.
We can specify validations for measurements via the `validations` prop of the `Screen` component:

```html
<Screen :validations="{
      text: {
        minLength: $magpie.v.minLength(4),
        alpha: $magpie.v.alpha
      }
    }">
    <Slide>
        <TextareaInput :response.sync="$magpie.measurements.text" />
    
        <button v-if="!$magpie.validateMeasurements.text.$invalid" @click="$magpie.saveAndNextScreen()">Submit</button>
    </Slide>
</Screen>
```

Here, we specify two validations for our `text` measurement: The text must be at least 4 characters long and may
only contain alphabetical characters.

Below, on the submit button, we added an if-statement that makes sure, we only show the button, if there are no problems
with the measurement.

You may have noticed, we use [$magpie.v](https://reference.magpie-experiments.org/#Magpie+validators) to access the validators we needed this time. `$magpie.v` provides
a selection of generally useful validators which you can find all listed in the reference. If you ever need a validator that is not in there, you can simply write a
function that returns true if the validation passed and false if it didn't.

```html
<Screen :validations="{
      text: {
        lengthRequirements: (text) => text.length >= 4 && text.length < 50
      }
    }">
    <Slide>
        <TextareaInput :response.sync="$magpie.measurements.text" />

        <button v-if="!$magpie.validateMeasurements.text.$invalid" @click="$magpie.saveAndNextScreen()">Submit</button>
    </Slide>
</Screen>
```

Here, we've added a hand-built validator that check whether the text is between 4 and 50 characters long.

Validations incidentally also work for built-in screens as they inherit from the abstract Screen component.
The variable that holds the response in these cases is always called `response`.

## Separating custom screens into files
If we want to use a custom screen multiple times in our experiment, we can separate it out into a different `.vue` file.

Suppose our experiment looks like this:

```html
<template>
    <Experiment>
        <template v-for="(task, i) in tasks">
            <Screen :key="i">
                <p>{{ task.question }}</p>
                <TextareaInput :response.sync="$magpie.measurements.text" />
                <button @click="$magpie.saveAndNextScreen()">Next</button>
            </Screen>
        </template>
    </Experiment>
</template>
<script>
    import tasks from './data/tasks.csv'
    export default {
        name: 'App',
        data() {
            return {
                tasks,
            }
        }
    }
</script>
```

Now, we're extracting the code for our screen into a new file `QuestionScreen.vue`.

```html
<!-- QuestionScreen.vue -->
<template>
    <Screen>
        <p>{{ task.question }}</p>
        <TextareaInput :response.sync="$magpie.measurements.text" />
        <button @click="$magpie.saveAndNextScreen()">Next</button>
    </Screen>
</template>

<script>
export default {
    name: 'QuestionScreen',
    props: {
        task: {
            type: Object,
            required: true
        }
    }
}    
</script>
```

Here, we define a new Vue component for the custom screen and introduce a prop called `task`. We need to pass the task
object to our custom screen explicitly as a prop, because vue components do not have access to the variables of other components.
So, much like you'd have to pass a local variable in your JavaScript to a different function as a parameter, if you want to use it
in that function, we need to explicitly pass variables across component borders, if we want other components to be able to use them.

To define a prop we always have to set two facts about it: What data type is it? (One of `Number`, `String`, `Array`, `Boolean`)
Is it required? If not, we have to set a default value using the `default` field.

Now, we can use this screen component in our main experiment file.

```html
<!-- App.vue -->
<template>
  <Experiment>
      <template v-for="(task, i) in tasks">
        <QuestionScreen
            :key="i"
            :task="task" />
      </template>
  </Experiment>
</template>
<script>
    import tasks from './data/tasks.csv'
    import QuestionScreen from './QuestionScreen'
    export default {
        name: 'App',
        components: { QuestionScreen },
        data() {
            return {
                tasks,
            }
        }
    }
</script>
```

We import the new screen component into the current file using a normal import statement and afterwards register it as a
subcomponent to be used as part of our main App component. Now, we can use it in the `<template>` part of the file.

## Creating result rows manually
Usually, you will use the `measurements` object to collect data and save it to the results as seen above:

```html
<Screen>
    <Slide>
        <RatingInput
                right="very appealing"
                left="very unappealing"
                :response.sync="$magpie.measurements.appetite" />
        <RatingInput
                right="very large"
                left="very small"
                :response.sync="$magpie.measurements.portion" />
        <RatingInput
                right="very healthy"
                left="very unhealthy"
                :response.sync="$magpie.measurements.healthy" />

        <button @click="$magpie.saveAndNextScreen()">Submit</button>
    </Slide>
</Screen>
```

This might result in the following result data set:

|appetite|portion|healthy|
|-|-|-|
|20|35|75|


However, sometimes you need to create multiple rows in the result data per screen.

You can add new result rows from within a screen as well as from within a method using [$mapgie.addTrialData()](https://reference.magpie-experiments.org/#Magpie+addTrialData)

For example, instead of using `Screen`'s `measurements` together with `saveAndNextScreen`, you can save the data manually
as follows:

```html
<Screen>
    <Slide>
        <RatingInput
                right="very appealing"
                left="very unappealing"
                :response.sync="$magpie.measurements.appetite" />
        <RatingInput
                right="very large"
                left="very small"
                :response.sync="$magpie.measurements.portion" />
        <RatingInput
                right="very healthy"
                left="very unhealthy"
                :response.sync="$magpie.measurements.healthy" />

        <button @click="$magpie.addTrialData({
          appetite: $magpie.measurements.appetite / 100,
          portion: $magpie.measurements.portion / 100,
          healthy: $magpie.measurements.healthy / 100
        })">Add row</button>
        <button @click="$magpie.nextScreen()">Next</button>
    </Slide>
</Screen>
```

Here we use a [RatingInput](https://reference.magpie-experiments.org/#ratinginput) to collect three different variables.
The participant can add new data multiple times using the "Add row" button's click handler, which
calls [$mapgie.addTrialData()](https://reference.magpie-experiments.org/#Magpie+addTrialData). Since the data is already
saved, we only need to call `nextScreen` instead of `saveAndNextScreen`.

This might result in the following result data set:

|appetite|portion|healthy|
|-|-|-|
|20|35|75|
|84|26|75|
|20|35|75|

### Passing variables directly to the results
Often, you will have independent variables that you want to be present in the result rows as well.
For this you can use the [Record](https://reference.magpie-experiments.org/#record) component.

```html
<Screen>
    <Slide>
        <img :src="imgpath" />
        
        <RatingInput
                right="very appealing"
                left="very unappealing"
                :response.sync="$magpie.measurements.appetite" />
        <RatingInput
                right="very large"
                left="very small"
                :response.sync="$magpie.measurements.portion" />
        <RatingInput
                right="very healthy"
                left="very unhealthy"
                :response.sync="$magpie.measurements.healthy" />
        
        <Record :data="{
              image: imgpath,
            }" />

        <button @click="$magpie.saveAndNextScreen()">Submit</button>
    </Slide>
</Screen>
```

Whatever object you pass to the Record component will be merged with your measurements the next time you save.

### Adding global data
We can also add data globally such that it will be present in all result rows using [$mapgie.addExpData()](https://reference.magpie-experiments.org/#Magpie+addExpData).

```html
<Screen>
    <Slide>
        <RatingInput
                right="very appealing"
                left="very unappealing"
                :response.sync="$magpie.measurements.appetite" />
        <RatingInput
                right="very large"
                left="very small"
                :response.sync="$magpie.measurements.portion" />
        <RatingInput
                right="very healthy"
                left="very unhealthy"
                :response.sync="$magpie.measurements.healthy" />

        <button @click="$magpie.addExpData({
          appetite: $magpie.measurements.appetite / 100,
          portion: $magpie.measurements.portion / 100,
          healthy: $magpie.measurements.healthy / 100
        }); $magpie.nextScreen()">Submit</button>
    </Slide>
</Screen>
```

This time, we call [$mapgie.addExpData()](https://reference.magpie-experiments.org/#Magpie+addExpData) on submit which adds the data globally, so that
it is present in every result row of our hypothetical experiment:

|response_time|response|appetite|portion|healthy|
|-|-|-|-|-|
|500|Yes|20|35|75|No
|736|Yes|20|35|75|
|365|No|20|35|75|
|615|Yes|20|35|75|

This is useful for demographic data like age or level of education.

You can use the [Record](https://reference.magpie-experiments.org/#record) component for this as well, by using the global
property:

```html
<Screen>
    <Slide>
        <RatingInput
                right="very appealing"
                left="very unappealing"
                :response.sync="$magpie.measurements.appetite" />
        <RatingInput
                right="very large"
                left="very small"
                :response.sync="$magpie.measurements.portion" />
        <RatingInput
                right="very healthy"
                left="very unhealthy"
                :response.sync="$magpie.measurements.healthy" />

        <Record
                global
                :data="{
                 maxAppetite: 100,
                 maxPortion: 100,
                 maxHealthy: 100
                }"
            />
        
        <button @click="$magpie.nextScreen()">Submit</button>
    </Slide>
</Screen>
```

### Using Record in built-in screens
You can also use the [Record](https://reference.magpie-experiments.org/#record) component in built-in screens. You only need to make sure
to declare it inside one of the named slots, e.g. `#stimulus`. 

```html
<CompletionScreen ...>
    <template #stimulus>
        <Record
                :data="{
                 maxAppetite: 100,
                 maxPortion: 100,
                 maxHealthy: 100
                }"
            />
    </template>
</CompletionScreen>
```
