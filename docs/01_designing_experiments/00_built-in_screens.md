# Built-in screens

## Trial screens
Trial screens are the parts of your experiment which are (usually) instantiated several times (realizing different
trials of your, say, main experimental task). They usually collect the data and often rely on
additional information (e.g., the picture to be displayed in trial 27, or the question and answer
options for trial 13).

_magpie has some ready-made trial screens built-in for simple experimental trials.

### Forced choice
Displays a context, a picture and a question to be answered in a two-alternative forced choice task.
Choices are made by clicking on one of the buttons.

```html
<ForcedChoiceScreen
    :options="['Yes', 'No']"
    question="Do you understand this question?"
    qud="Always do the opposite of what you are asked."
/>
```

### Image selection
Realizes another forced choice task, by presenting multiple pictures (arranged horizontally) and
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
Requires users to type in text freely in a text box.
```html
<TextareaScreen
    question="What do you usually eat?"
    qud="Eating healthy is good for you."
/>
```

### More built-in screens
See the API docs for more built-in screens.
