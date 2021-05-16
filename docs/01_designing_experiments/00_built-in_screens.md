# Built-in screens

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
<PostTestScreen #default="{ measurements }">
    <label>Name<input type="text" v-model="measurements.name"></label>
</PostTestScreen>
```

## Trial screens
Trial screens are the parts of your experiment which are (usually) instantiated several times (realizing different
trials of your, say, main experimental task). They usually collect the data and often rely on
additional information (e.g., the picture to be displayed in trial 27, or the question and answer
options for trial 13).

_magpie has some ready-made trial screens built-in for simple experimental trials.

### Forced choice
The [`<ForcedChoiceScreen>`](https://magpie-reference.netlify.app/#forcedchoicescreen) component displays a context, a picture and a question to be answered in a two-alternative forced choice task.
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
