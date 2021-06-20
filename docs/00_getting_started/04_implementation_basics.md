# Implementation basih Vue.js, first learn about the basics in [Section Vue.js]() (**TODO: insert link**).
The structure of the component tree resembles the above conceptual schema of an experiment, using screens and slides, discussed in Section [Screens & Slides]() (**TODO: fix link**).

### Project file structure

By default, a _magpie project has the following file structure:

```gitignore
# top-level configuration files
# these are usually not interesting
.eslintrc.js
.gitignore
.prettierrc.js
package.json
package-lock.json
vue.config.js

# This is where npm installs the dependencies of your project
# You usually don't need to look in here
node_modules/

# Files for including in your experiment 
public/
  # your image files
  images/
  # your video files
  video/
  # your audio files
  audio/
 
# Data (e.g.,csv files), with information about how to
# realize individual trials, e.g., which pictures to show etc.
trials/

# The source code that realizes your experiment
# The main work happens here
src/
  # _magpie-specific configuration
  magpie.config.js
  # set-up code for Vue.js, usually you don't need to touch this
  main.js
  # The main file for your experiment.
  App.vue

```

### The `Experiment` element

The root component of the component tree will always be the one in file `App.vue` in your project `src` directory. 
The top-most component in your `App.vue` should be an [`<Experiment>`](https://magpie-reference.netlify.app/#experiment) element, which initializes _magpie and makes sure you can use all _magpie functionality in your experiment.

**TODO: please check: example created by 'magpie new' has 'template' wrapped around 'experiment'; if that's always necessary, maybe best to also show it in the HTML below and comment on why that is **

**TODO: I wonder if there is a difference between 'element' and 'component'; if so, let's make sure the terminology is always used correctly (no need to explicitly introduce the difference if there is one, I think)**

```html
<Experiment>
    
    
</Experiment>
```

### The `Screen` elements
Below the `Experiment` element, you define your screens. You can use ready-made screens, which _magpie supplies for convenience, such as the `InstructionsScreen`, but you can also use the flexible [`<Screen>`](https://magpie-reference.netlify.app/#screen) component to build your own custom screens.
For a list of available ready-made screens see [here]() (**TODO: insert link**).

```html
<Experiment>
  
    <InstructionScreen :title="'Welcome'">
      <!-- ... -->
    </InstructionScreen>
    
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
_magpie also supplies a magic variable called [`$magpie`](https://magpie-reference.netlify.app/#Magpie), which gives you
access to many utility functions, including methods to change slides and screens, mouse and eyetracking as well as a
messaging socket to communicate with other running sessions of the same experiment.

## A minimal example

This is a simple example of how your experiment code could look like.

**TODO: maybe explain what's going on here? maybe introduce bit by bit? maybe embedd a video here, developing this minimal example?**

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
