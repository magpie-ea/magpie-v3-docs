# Independent variables
Let's have another look at our example experiment.

```html
<template>
  <!-- The title prop will be used for the browser tab title -->
  <Experiment title="magpie demo">
      
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
                <img :src="rating_task.picture" />
            </template>
        </SliderScreen>
      </template>

      <!-- This screen will ask some optional questions about the
           participant's background, like age, gender etc. -->
      <PostTestScreen />

      <!-- While setting your experiment mode to 'debug' in the magpie config
       this screen will show the results of the current experiment directly. Once you switch to directLink or prolific
       this screen will submit the results to your magpie backend -->
      <SubmitResultsScreen />
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

Here, we have our independent variables already in JavaScript form, stored in a variable called `sliderRating`, which we then inject into the experiment data.

## Loading csv data
Usually, you have trial data only available in csv. You can import csv files into your experiment as follows:

```js
import sliderRating from '../trials/slider_rating.csv'
```

If we place this at the top of the `<script>` block in our `.vue` file, we will import the data from `slider_rating.csv`
in the `trials` folder of our project.

The value of `sliderRating` now looks similar to the data we saw above as raw JavaScript: An array of objects with the
CSV columns as property names.

## Iterating over trials
As seen in the example above you can now simply iterate over your trial data using

```html
<template v-for="(rating_task, i) in sliderRating">
```

## Trial Randomization
To randomize your trial data, you can use the function [_.shuffle](https://lodash.com/docs/4.17.15#shuffle) provided by the `lodash` library.
(Lodash comes pre-installed with all projects created with the `magpie` command.)

First, we have to import lodash as follows

```js
import _ from 'lodash'
```

then we can use lodash's shuffle to randomly shuffle our trial data:

```html
<script>
import sliderRating from '../trials/slider_rating.csv'
import _ from 'lodash'
export default {
  name: 'App',
  data() {
    return {
      sliderRating: _.shuffle(sliderRating),
    };
  }
};
</script>
```

## In-situ randomization
If you would like to randomly draw a sample from a collection at certain points in your experiment, you can use
lodash's [_.sample](https://lodash.com/docs/4.17.15#sample)


```html
<script>
import sliderRating from '../trials/slider_rating.csv'
import _ from 'lodash'
export default {
  name: 'App',
  data() {
    return {
      sliderRating,
    };
  },
  methods: {
    getRandomColor() {
        return _.sample(['green', 'blue', 'yellow', 'red', 'orange'])
    }
  }
};
</script>
```

In a custom screen we could then use this method to draw a circle with a random color as follows:

```html
<Screen title="Some drawing exercises">
    
    <Slide>
      <CanvasStage :config="{width: 700, height: 500}">
        <CanvasLayer>
          <CanvasCircle :config="{
            x: 100,
            y: 100,
            radius: 70,
            fill: 'red',
            stroke: getRandomColor(),
            strokeWidth: 2 }"></CanvasCircle>
        </CanvasLayer>
      </CanvasStage>
    </Slide>
    
</Screen>
```

## Pre-loading media assets
When displaying media on the web, it is usually only loaded by the browser the second it is about to be displayed.
In an experiment that depends on response times, this is not ideal, so we often want to pre-load media assets before
they are displayed.

For this, the [Experiment](https://magpie-reference.netlify.app/#experiment) component has various `assets` props:

```html
<template>
  <Experiment title="magpie demo"
              :image-assets="pictures">
      <!--
      ...
      -->
  </Experiment>
</template>

<script>
import sliderRating from '../trials/slider_rating.csv'
export default {
  name: 'App',
  data() {
    return {
        // Take the picture path from all slider rating tasks
        pictures: sliderRating.map(task => task.picture),
        
        sliderRating,
    };
  }
};
</script>
```

Here, we extract all pictures from our trial data and pass them to the [Experiment](https://magpie-reference.netlify.app/#experiment) component via the `image-assets` prop.
This will cause all passed image paths to be loaded with the initial page load.
