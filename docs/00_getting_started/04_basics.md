# Basics

## Concepts
Experiments in _magpie follow a certain conceptual structure, as visualized in the following schema, to facilitate
good practice and aid in the design of experiments.

<img src="../../images/getting_started/experiments_schema.png" alt="Experiments schema" />

A _magpie experiment firstly consists of the **data** used to build the experiment, usually termed "independent variables" in psychological literature.
This could be textual/visual/auditory stimuli that will be presented to the participants or possible choices that the participant can make.

Every experiment is then composed of a series of **screens** that participants will go through sequentially.
Such a screen could display instructions on how to participate, or present an actual trial task to the participant.
Usually you will realize one trial per screen.

A screen may consist of one or more **slides**. Slides simplify dynamic presentation of more intricate tasks.
They are also displayed sequentially, giving experimenters the opportunity to arrange a sequence of stimuli or tasks in one screen.

Anytime during a screen, you can directly save a **trialData** row which will be sent to the _magpie server when the experiment is completed.
To make collecting measurements across slides easier, you can also gradually accumulate data in a per-screen **`measurements`** object
(think text that the participant entered, a choice between different items, or the response
time taken to accomplish the task). Once the trial is completed you can simply save all measurements in one go.

Screens also allow specifying **validations** the measured observations, for example to make sure entered text has a certain character length.

If you make measurements that are relevant for all trials, or the experiment as a whole, like screen resolution or age of the participant,
you can add those to **expData**, which will be injected into all trialData rows after completion of the experiment.

To ease the realization of experiments, there are many **built-in screen components** ready for usage,
allowing the quick assembly of simple experiments. These screens always have 4 slides, which implement

1. A **pause**
2. A **fixation** point
3. **Stimulus** presentation
4. **Response** collection

The duration of these slides and other parameters are freely configurable in these screen components.

## _magpie facilities
A _magpie experiment is [a Vue.js component tree](/00_getting_started/03_vue_js/), which is basically an HTML file with
an extended collection of HTML elements available, along with some management logic in JavaScript. The structure of the 
component tree resembles the above schema sketch.

### Experiment
The root component will always be `App.vue` in your project `src` directory. The top-most component in your `App.vue`
should be an [`<Experiment>`](https://magpie-reference.netlify.app/#experiment) element, which initializes _magpie and
makes sure you can use all _magpie functionality in your experiment.

```html
<Experiment>
    
    
</Experiment>
```

### Screens
Below the Experiment element, you define your screens. You can use the built-in screens
to build experiments with simpler trials, but you can also use the flexible [`<Screen>`](https://magpie-reference.netlify.app/#screen) component
to build your own custom screens.

```html
<Experiment>
    <Screen>
        <!-- ... -->
    </Screen>

    <Screen>
        <!-- ... -->
    </Screen>
    
    <!-- ... -->
    
</Experiment>
```

### $magpie
_magpie also exposes a magic variable called [`$magpie`](https://magpie-reference.netlify.app/#Magpie), which gives you
access to many utility functions, including methods to change slides and screens, mouse and eyetracking as well as a
messaging socket to communicate with other running sessions of the same experiment.

## Example
This is a simple example of how your experiment code could look like.

```html
<template>
  <!-- The title prop will be used for the browser tab title -->
  <Experiment title="_magpie demo">
      
      <!-- This is the welcome screen -->
      <InstructionScreen :title="'Welcome'">
        This is a sample introduction screen.
        <br />
        <br />
        This screen welcomes the participant and gives general information about
        the experiment.
        <br />
        <br />
        This mock up experiment is a showcase of the functionality of magpie.
      </InstructionScreen>

      <!-- We iterate over our experiment trials -->
      <template v-for="(rating_task, i) in sliderRating">
        <!-- and display a screen with a slider rating task
             using the built-in SliderScreen component -->
        <SliderScreen
            :key="i"
            :left="rating_task.optionLeft"
            :right="rating_task.optionRight">
            <template #stimulus>
                <!-- Here we populate the stimulus slot with an image stimulus -->
                <img :src="rating_task.picture" />
            </template>
        </SliderScreen>
      </template>

      <!-- This screen will ask some optional questions about the
           participant's background, like age, gender etc. -->
      <PostTestScreen />

      <!-- This screen is useful while testing your experiment to check
           the results immediately after taking the experiment -->
      <DebugResultsScreen />
  </Experiment>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      sliderRating,
    };
  }
};

// Here we define the independent variables for our experiment
// we could also use a separate csv file for this purpose

const sliderRating = [
  {
    picture: 'images/question_mark_02.png',
    question: 'How are you today?',
    optionLeft: 'fine',
    optionRight: 'great'
  },
  {
    picture: 'images/question_mark_01.png',
    question: "What's the weather like?",
    optionLeft: 'shiny',
    optionRight: 'rainbow'
  },
  {
    QUD: 'Here is a sentence that stays on the screen from the very beginning',
    picture: 'images/question_mark_03.jpg',
    question: "What's on the bread?",
    optionLeft: 'ham',
    optionRight: 'jam'
  }
];
</script>

```
