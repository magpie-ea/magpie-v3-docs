# Built-in screens
There are two basic categories of screens: Trial screens display the actual trial tasks, while wrapper screens wrap your trial block
with useful information like instructions or ask for demographic information about participants.

## Wrapper screens
Wrapper screens allow you to display information about the experiment or ask for additional information
about the participants.

### Instruction screen
The [`<InstructionScreen>`](https://magpie-reference.netlify.app/#instructionscreen) component displays informational
text or HTML along with a simple "Next" button below it.

```html
<InstructionScreen>
    <p>Thank you for participating in this experiment.</p>
    <p>On the next screen you will find more information about how your data will be used.</p>
</InstructionScreen>
```

### Post Test Screen
The [`<PostTestScreen>`](https://magpie-reference.netlify.app/#posttestscreen) component asks for additional information
about the participant. By default, it will ask for age, gender, level of education, native langauges and further comments.

```html
<PostTestScreen />
```

You can disable any of the questions using the associated prop:

```html
<PostTestScreen :age="false" />
```

If you would like to ask for additional information, you can use the component's default slot as follows:

```html
<PostTestScreen>
    <label>Name<input type="text" v-model="$magpie.measurements.name"></label>
</PostTestScreen>
```

## Trial screens
Trial screens are the parts of your experiment which are (usually) instantiated several times (realizing different
trials of your main experimental task, for example). They usually collect the data and often rely on
additional information (e.g. pictures to be displayed, or questions and answer options).

Magpie has some ready-made trial screens built-in for simple experimental trials.

### Life cycle phases
The built-in trial screens are usually based on the [`<LifecycleScreen>`](https://magpie-reference.netlify.app/#lifecyclescreen) component and thus
have 4 slides or phases:

 * A pause phase of variable duration, where nothing is displayed
 * A fixation phase of variable duration, where a fixation cross or similar is displayed
 * A stimulus phase of variable duration, where the stimulus is presented
 * A response phase with an optional timeout, where the participant can respond

The first two can be skipped and the stimulus and response phase can be merged into one phase.

You can provide a stimulus by overriding the `stimulus` slot of the relevant screen as follows:

```html
<ForcedChoiceScreen
    :options="['Yes', 'No']"
    question="Do you understand this question?"
    qud="Always do the opposite of what you are asked."
>
    <template #stimulus>
        <img src="img/confusion.jpg" />
    </template>
</ForcedChoiceScreen>
```

This will display the stimulus on the same slide as the response inputs.

If you want to present the stimulus only for a certain amount of time, you can use the `stimulusTime` prop:

```html
<ForcedChoiceScreen
    :options="['Yes', 'No']"
    question="Do you understand this question?"
    qud="Always do the opposite of what you are asked."
    :stimulusTime="1500"
>
    <template #stimulus>
        <img src="img/confusion.jpg" />
    </template>
</ForcedChoiceScreen>
```

For things like audio or video, it can also be useful to go to the next slide dynamically:

```html
<ForcedChoiceScreen
    :options="['Yes', 'No']"
    question="Do you understand this question?"
    qud="Always do the opposite of what you are asked."
    :stimulusTime="-1"
>
    <template #stimulus>
        <audio src="audio/sealion.ogg" autoplay @ended="$magpie.nextSlide()" /> 
    </template>
</ForcedChoiceScreen>
```

Here we set `stimulusTime` to `-1` to indicate that we want to control stimulus presentation ourselves.

For more details on the available parameters, [refer to the reference on the LifecycleScreen](https://magpie-reference.netlify.app/#lifecyclescreen).

### Forced choice
The [`<ForcedChoiceScreen>`](https://magpie-reference.netlify.app/#forcedchoicescreen) component displays a context,
and a question to be answered in a forced choice task, with a variable number of options.
Choices are made by clicking on one of the buttons.

```html
<ForcedChoiceScreen
    :options="['Yes', 'No']"
    question="Do you understand this question?"
    qud="Always do the opposite of what you are asked."
/>
```

### Image selection
The [`<ImageSelectionScreen>`](https://magpie-reference.netlify.app/#imageselectionscreen) realizes another forced choice task, by presenting multiple pictures (arranged horizontally) and
requiring a click on one of the pictures.

```html
<ImageSelectionScreen
    :options="[
            {src: 'img/fries.jpg', label: 'fries'},
            {src: 'img/soup.jpg', label: 'soup' }
            ]"
    question="What do you eat?"
    qud="Eating healthy is good for you."
/>
```

### Text box input
The [`<TextareaScreen>`](https://magpie-reference.netlify.app/#textareascreen) requires users to type in text freely in a text box.
```html
<TextareaScreen
    question="What do you usually eat?"
    qud="Eating healthy is good for you."
/>
```

### More built-in screens
Have a look at [the API reference for more built-in screens](https://magpie-reference.netlify.app/#screens).
