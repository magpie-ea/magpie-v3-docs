# Basics

## Concepts
Experiments in _magpie follow a certain conceptual structure, as visualized in the following schema.

![Experiments schema](../../images/getting_started/experiments_schema.png)

A _magpie experiment consists of data used to build the experiment, usually termed "independent variables" in psychological literature.
Each experiment defines a series of screens that participants will go through sequentially.

Trial screens will optionally obtain a set of measurements that will be stored for submission to the server, which can
be anything from a text that the participant entered, a choice between different items, to the response time taken to accomplish the task.

A screen also allows validating the measured observations, e.g. to make sure entered text has a certain character length.

To allow for more dynamic tasks, Screens may consist of multiple slides, which are displayed sequentially, giving experimentors
the opportunity to display fixation points, or enforce pauses between stimulus and response.

## _magpie facilities

### Experiment
The root component will always be `App.vue` in your project `src` directory. The top-most component in your `App.vue`
should be an [`<Experiment>`](https://magpie-reference.netlify.app/#experiment) element, provided by _magpie, which initializes _magpie and makes sure you can use all _magpie functionality in your experiment.

### Screens
Below the Experiment element, you define your screens. You can use the built-in screens
to build experiments with simpler trials, but you can also use the flexible [`<Screen>`](https://magpie-reference.netlify.app/#screen) component
to build your own custom screens.

### $magpie
_magpie also exposes a magic variable called [`$magpie`](#https://magpie-reference.netlify.app/#Magpie), which gives you access to more advanced functionality for your experiment.

## Example
This is a simple example of how your experiment code could look like.

```html
<template>
  <!-- The title prop will be used for the browser tab title -->
  <Experiment title="_magpie demo">
      
    <!-- The contents of the #title template slot will be
         displayed in the upper left corner of the experiment -->
    <template #title>
      <div>The experiment</div>
    </template>

    <!-- The contents of the #screens template slot
         define the screens of your experiment -->
    <template #screens>
      
      <!-- This is the welcome screen -->
      <Screen :title="'Welcome'">
        This is a sample introduction screen.
        <br />
        <br />
        This screen welcomes the participant and gives general information about
        the experiment.
        <br />
        <br />
        This mock up experiment is a showcase of the functionality of magpie.
        <!-- The $magpie variable gives you access to magpie-specific functionality -->
        <button @click="$magpie.nextScreen()">Begin the experiment</button>
      </Screen>

      <!-- We iterate over our experiment trials -->
      <template v-for="(rating_task, i) in sliderRating">
        <!-- and display a screen with a slider rating task
             using the built-in SliderScreen component -->
        <SliderScreen
            :key="i"
            :picture="rating_task.picture"
            :left="rating_task.optionLeft"
            :right="rating_task.optionRight" />
      </template>

      <!-- This screen will ask some optional questions about the
           participant's background, like age, gender etc. -->
      <PostTestScreen />

      <!-- This screen is useful while testing your experiment to check
           the results immediately after taking the experiment -->
      <DebugResultsScreen />
    </template>
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
