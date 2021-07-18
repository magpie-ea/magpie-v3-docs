# Progress bar
Magpie can show a progress bar at the top right of the experiment, indicating the participant's progress in the experiment.

## Setting progress per screen
We can make any screen display a progress bar using the [`progress`](https://magpie-reference.netlify.app/#screen) prop,
which takes a value between 0 and 1:

```html
<template>
  <!-- The title prop will be used for the browser tab title -->
  <Experiment title="magpie demo">
      
    <!-- The contents of the #title template slot will be
         displayed in the upper left corner of the experiment -->
    <template #title>
      <div>The experiment</div>
    </template>

    <!-- The contents of the #screens template slot
         define the screens of your experiment -->
    <template #screens>
      
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
            :picture="rating_task.picture"
            :left="rating_task.optionLeft"
            :right="rating_task.optionRight" 
            :progress="i / sliderRating.length" />
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
import sliderRating from '../trials/slider_rating.csv';
export default {
  name: 'App',
  data() {
    return {
      sliderRating,
    };
  }
};
</script>
```

## Setting progress manually
You can also update the progress during the course of a single trial using [`$magpie.setProgress()`](https://magpie-reference.netlify.app/#Magpie+setProgress)
