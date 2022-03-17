# Built-in screens
There are two basic categories of screens: Trial screens display the actual trial tasks, while wrapper screens wrap your trial block
with useful information like instructions or ask for demographic information about participants.

## Wrapper screens
Wrapper screens allow you to display information about the experiment or ask for additional information
about the participants.

### Instruction screen
The [InstructionScreen](https://magpie-reference.netlify.app/#instructionscreen) component displays informational
text or HTML along with a simple "Next" button below it.

```html
<InstructionScreen>
    <p>Thank you for participating in this experiment.</p>
    <p>On the next screen you will find more information about how your data will be used.</p>
</InstructionScreen>
```

## Trial screens
Trial screens are the parts of your experiment which are (usually) instantiated several times (realizing different
trials of your main experimental task, for example). They usually collect the data and often rely on
additional information (e.g. pictures to be displayed, or questions and answer options).

Magpie has some ready-made trial screens built-in for simple experimental trials, all [listed in the reference](https://magpie-reference.netlify.app/#trial-screens).

### Forced choice
The [ForcedChoiceScreen](https://magpie-reference.netlify.app/#forcedchoicescreen) component displays a context,
and a question to be answered in a forced choice task, with a variable number of options.
Choices are made by clicking on one of the buttons.

```html
<ForcedChoiceScreen
    :options="['Yes', 'No', 'Maybe']"
    question="Do you understand this question?"
    qud="Always do the opposite of what you are asked."
/>
```

### Image selection
The [ImageSelectionScreen](https://magpie-reference.netlify.app/#imageselectionscreen) realizes another forced choice task, by presenting multiple pictures (arranged horizontally) and
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
The [TextareaScreen](https://magpie-reference.netlify.app/#textareascreen) requires users to type in text freely in a text box.
```html
<TextareaScreen
    question="What do you usually eat?"
    qud="Eating healthy is good for you."
/>
```


## Life cycle phases
Built-in trial screens usually have 5 phases in the following order:

1. A pause phase of variable duration, displaying nothing (optional)
2. A fixation phase of variable duration, displaying a fixation cross or similar (optional)
3. A stimulus phase of variable duration, presenting the stimulus (optional)
4. A response phase with an optional timeout, allowing the participant to respond
5. A feedback phase where you can give feedback to the participant (optional)

(If you'd like to implement a custom view with these phases, you can use [LifecycleScreen](https://magpie-reference.netlify.app/#lifecyclescreen).)

Let's have a look at this using the ForcedChoiceScreen as an example.

### Only response phase
```html
<ForcedChoiceScreen
    :options="['Yes', 'No']"
    question="Do you understand this question?"
    qud="Always do the opposite of what you are asked."
/>
```

Here we only have a response phase, presenting a question as well as the response choices "Yes" and "No".

### Stimulus and response phase together
You can provide a stimulus by overriding the `stimulus` slot of the relevant screen as follows:

```html
<ForcedChoiceScreen
    :options="['Yes', 'No']"
    question="Do you understand this question?"
    qud="Always do the opposite of what you are asked.">
    <template #stimulus>
        <img src="img/confusion.jpg" />
    </template>
</ForcedChoiceScreen>
```

Here we pass an image to the stimulus slot. This will display the stimulus on the same slide as the response inputs.

### Separate stimulus and response phase
If you want to present the stimulus separately from the response phase, you need to define a stimulus time.

```html
<ForcedChoiceScreen
    :options="['Yes', 'No']"
    question="Do you understand this question?"
    qud="Always do the opposite of what you are asked."
    :stimulusTime="1500">
    <template #stimulus>
        <img src="img/confusion.jpg" />
    </template>
</ForcedChoiceScreen>
```

This will present the stimulus for 1.5 seconds and then present the response slide instead.

### Separate stimulus and response phase (manual transition)
For things like audio or video stimuli, it can also be useful to go to the next slide manually:

```html
<ForcedChoiceScreen
    :options="['Yes', 'No']"
    question="Do you understand this question?"
    qud="Always do the opposite of what you are asked."
    :stimulusTime="-1">
    <template #stimulus>
        <audio src="audio/sealion.ogg" autoplay @ended="$magpie.nextSlide()" /> 
    </template>
</ForcedChoiceScreen>
```

Here we set `stimulusTime` to `-1` to indicate that we want to control stimulus presentation ourselves and use
`$magpie.nextSlide()` to go to the next slide once the audio playback has ended.

### Pause and stimulus
To add a pause phase, simply add a `pauseTime` prop, defining the pause duration.
```html
<ForcedChoiceScreen
    :options="['Yes', 'No']"
    question="Do you understand this question?"
    qud="Always do the opposite of what you are asked."
    :pauseTime="500">
    <template #stimulus>
        <img src="img/confusion.jpg" />
    </template>
</ForcedChoiceScreen>
```

In this example we pause for 0.5s before presenting the stimulus and the response interface.

### Pause, Fixation and stimulus phase
```html
<ForcedChoiceScreen
    :options="['Yes', 'No']"
    question="Do you understand this question?"
    qud="Always do the opposite of what you are asked."
    :pauseTime="500"
    :fixationTime="1000">
    <template #stimulus>
        <img src="img/confusion.jpg" />
    </template>
</ForcedChoiceScreen>
```

Here, we display a fixation cross for 1s after a 0.5s pause. Finally, we display the stimulus together with the response interface.

### Custom fixation phase
If you would like to use your own fixation material, you can use the `fixation` slot.
```html
<ForcedChoiceScreen
    :options="['Yes', 'No']"
    question="Do you understand this question?"
    qud="Always do the opposite of what you are asked."
    :fixationTime="1000">
    <template #fixation>
        <img src="img/fixation.jpg" />
    </template>
    <template #stimulus>
        <img src="img/confusion.jpg" />
    </template>
</ForcedChoiceScreen>
```

Here, we present a custom image for 1s in the fixation phase. Afterwards we present the stimulus and the response interface together in the same phase.

### Feedback phase
If you would like to provide feedback, after the participant has responded, you can use the `feedback` slot.

```html
<ForcedChoiceScreen
    :options="['Yes', 'No']"
    question="Do you understand this question?"
    qud="Always do the opposite of what you are asked."
    :feedbackTime="1500">
    <template #stimulus>
        <img src="img/confusion.jpg" />
    </template>
    <template #feedback>
        <p v-if="$magpie.measurements.response === 'No'">Correct</p>
        <p v-else>Incorrect</p>
    </template>
</ForcedChoiceScreen>
```

Here, we check the response the participant gave and display an indicator of whether it is right or wrong for 1.5s.

### Feedback phase (manual transition)
If you would like to provide feedback, after the participant has responded, you can use the `feedback` slot.

```html
<ForcedChoiceScreen
    :options="['Yes', 'No']"
    question="Do you understand this question?"
    qud="Always do the opposite of what you are asked."
    :feedbackTime="-1">
    <template #stimulus>
        <img src="img/confusion.jpg" />
    </template>
    <template #feedback>
        <p v-if="$magpie.measurements.response === 'No'">Correct</p>
        <p v-else>Incorrect</p>
        <button @click="$magpie.nextScreen()">Ok</button>
    </template>
</ForcedChoiceScreen>
```

Here, we check the response the participant gave and display an indicator of whether it is right or wrong.
The user then has the possibility to go to the next screen with the click of a button.

For more details on the available parameters, [refer to the reference on the LifecycleScreen](https://magpie-reference.netlify.app/#lifecyclescreen).


## Special screens
All special screens are [listed in the reference](https://magpie-reference.netlify.app/#screens).

### Post Test Screen
The [PostTestScreen](https://magpie-reference.netlify.app/#posttestscreen) component asks for additional information
about the participant. By default, it will ask for age, gender, level of education, native languages and further comments.

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

### SubmitResultsScreen
The [SubmitResultsScreen](https://magpie-reference.netlify.app/#submitresultsscreen) is usually placed at the end
of an experiment to bundle up the recorded data and send it to the server. Once that is done, it will thank the participant for you.
If you put your experiment into debug mode in the magpie configuration, this screen will behave like
[DebugResultsScreen](https://magpie-reference.netlify.app/#debugresultsscreen) and will not submit any data.

```html
<SubmitResultsScreen />
```

### DebugResultsScreen
The [DebugResultsScreen](https://magpie-reference.netlify.app/#debugresultsscreen) can be placed at the end of an
experiment instead of the SubmitResultsScreen. Instead of submitting the recorded data to the server, it will display the data
and offer a button for download.

```html
<DebugResultsScreen />
```
