# Trial views

Trial views are the parts of your experiment which are (usually) instantiated several times (realizing different trials of your, say, main experimental task). They usually collect the data and often rely on additional information (e.g., the picture to be displayed in trial 27, or the question and answer options for trial 13).

All trial views have three **obligatory fields**:

1. `trials: int` - the number of trials this view will appear
2. `name: string` - the name of the view
3. `data: array` - list of objects, each with information for each consecutive trial

Different types of wrapping views have more optional fields, as documented below. 

## Forced-choice task

Instantiate with `babeView.forcedChoice`. Displays a context, a picture and a question to be answered in a two-alternative forced choice task. Choices are made by clicking on one of two buttons.

<img src='../../images/views_samples/view_fc.png' alt='sample' height='auto' width='auto' style="border:2px solid black" />

* **Obligatory Fields**
    * `option1: string` - text on button for option 1
    * `option2: string` - text on button for option 2

* **Optional Fields**
    * `question: string` - question to be asked
    * `QUD: string` - text that is always present on the slide
    * `canvas: object` - [more about babe canvas](05_canvas.md)
    * `picture: string (link)` - picture to be displayed

* **Sample data**

```
const forced_choice_trials = [
    {
        picture: "path/to/picture_of_questionmark.jpg",
        question: "What's the weather like? like?",
        option1: "shiny",
        option2: "rainbow"
    }
];
```
## Sentence Choice task

Instantiate with `babeViews.sentenceChoice`. Similar to the `forcedChoice` view, this view presents two text-based options to click on. This view, however, realizes options on longer buttons arranged vertically. This is better for choice between several longer expressions, like whole sentences.

<img src='../../images/views_samples/view_ss.png' alt='sample' height='auto' width='auto' style="border:2px solid black" />

* **Obligatory Fields**
    * `option1: string`
    * `option2: string`

* **Optional Fields**
    * `QUD: string` - text that is always present on the slide
    * `canvas: object` - [more about babe canvas](05_canvas.md)
    * `picture: string (link)`
    * `question: string`

* **Sample data**

```
const sentence_choice_trials = [
    {
        picture: 'path/to/picture_of_bread.jpg',
        question: "What's on the bread?",
        option1: 'jam',
        option2: 'ham'
    },
    {
        picture: 'path/to/picture_of_bread.jpg',
        option1: 'jam',
        option2: 'ham'
    },
    {
        question: "What's the weather like?",
        option1: 'shiny',
        option2: 'rainbow'
    }
];
```

## Image Selection task

Instantiate with `babeViews.imageSelection`. Realizes another 2-alternative forced choice task, by presenting two pictures (arranged horizontally) and requiring a click on one of the pictures. 

<img src='../../images/views_samples/view_is.png' alt='sample' height='auto' width='auto' style="border:2px solid black" />

* **Obligatory Fields**
    * `option1: string` - label for choice of picture 1 (stored in `response` variable)
    * `option2: string` - label for choice of picture 2 (stored in `response` variable)
    * `picture1: string (link)` - refers to `option1`
    * `picture2: string (link)` - refers to `option2`

* **Optional Fields**
    * `QUD: string` - text that is always present on the slide
    * `question: string`
    * `canvas: object` - [more about babe canvas](05_canvas.md)

* **Sample data**

```
const image_selection_trials = [
    {
        picture1: 'path/to/picture1.jpg',
        picture2: 'path/to/picture2.jpg',
        option1: 'yes',
        option2: 'no'
    },
    {
        picture1: 'path/to/picture_of_bread1.jpg',
        picture2: 'path/to/picture_of_bread2.jpg',
        question: "What's on the bread?",
        option1: 'jam',
        option2: 'ham'
    }
];
```

## Textbox Input task

Instantiate with `babeViews.textboxInput`. Requires users to type in text freely in a textbox. Allows to specify a minimum number of characters before the `next` button appears. 

<img src='../../images/views_samples/view_ti.png' alt='sample' height='auto' width='auto' style="border:2px solid black" />

* **Obligatory Fields**
    * `question: string` - question to answer

* **Optional Fields**
    * `QUD: string` - text that is always present on the slide
    * `canvas: object` - [more about babe canvas](05_canvas.md)
    * `picture: string (link)` - picture to be displayed
    * `min_chars: number`
        * the minumum number of characters in the textarea field before the `next` button appears
        * default - *10*

* **Sample data**

```
const textbox_input_trials = [
    {
        picture: "path/to/picture.jpg",
        question: "How are you today?",
        min_chars: 100
    },
    {
        question: "What's the weather like?",
        min_chars: 50
    }
];
```

## Slider Rating task

Instantiate with `babeViews.sliderRating`. Gives you a single (horizontally oriented) slider, with endpoints whose labels can be specified. The `next` button only appears when the slider is clicked on or moved at least once. Internally slider values are represented as ranging from 0 to 100 in steps of 1.

<img src='../../images/views_samples/view_sr.png' alt='sample' height='auto' width='auto' style="border:2px solid black" />

* **Obligatory Fields**
    * `optionLeft: string`
    * `optionRight: string`

* **Optional Fields**
    * `QUD: string` - text that is always present on the slide
    * `canvas: object` - [more about babe canvas](05_canvas.md)
    * `picture: string (link)`
    * `question: string`

* **Sample data**

```
const slider_rating_trials = [
    {
        picture: 'path/to/picture_of_bread.jpg',
        question: "What's on the bread?",
        optionLeft: 'jam',
        optionRight: 'ham'
    },
    {
        question: "What's the weather like?",
        optionLeft: 'shiny',
        optionRight: 'rainbow'
    }
];
```

## Dropdown Choice task

Instantiate with `babeViews.dropdownChoice`. Prompts the user to select one option from a drop-down menu, which can be embedded into a sentence, e.g., to fill in a word or phrase in a fixed sentence frame.

<img src='../../images/views_samples/view_dc.png' alt='sample' height='auto' width='auto' style="border:2px solid black" />


* **Obligatory Fields**
    * `option1: string`
    * `option2: string`

* **Optional Fields**
    * `QUD: string` - text that is always present on the slide
    * `canvas: object` - [more about babe canvas](05_canvas.md)
    * `picture: string (link)`
    * `question_left_part: string`
    * `question_right_part: string`

* **Sample data**

```
const dropdown_choice_trials = [
    {
        picture: 'path/to/picture_of_bread.jpg',
        question: "What's on the bread?",
        option1: 'jam',
        option2: 'ham'
    },
    {
        question: "What's the weather like?",
        option1: 'shiny',
        option2: 'rainbow'
    }
];
```

## Rating Scale task

Instantiate with `babeViews.ratingScale`. Realizes a Likert-scale (ordinal) rating task, with button labeled with consecutive numbers. Participants click on these numbered buttons to proceed. Allows labels for endpoints on the scale.

<img src='../../images/views_samples/view_rc.png' alt='sample' height='auto' width='auto' style="border:2px solid black" />



* **Obligatory Fields**
    * `optionLeft: string`
    * `optionRight: string`

* **Optional Fields**
    * `QUD: string` - text that is always present on the slide
    * `canvas: object` - [more about babe canvas](05_canvas.md)
    * `picture: string (link)`
    * `question: string`

* **Sample data**

```
const rating_scale_trials = [
    {
        picture: 'path/to/picture_of_bread.jpg',
        question: "What's on the bread?",
        option1: 'jam',
        option2: 'ham'
    },
    {
        question: "What's the weather like?",
        option1: 'shiny',
        option2: 'rainbow'
    }
];
```

## Key Press task

Instantiate with `babeViews.keyPress`. Offers a 2-alternative forced choice task where choice options are given by pressing keys on the keyboard. Ideal for more accurate reaction time measurements.

<img src='../../images/views_samples/view_kp.png' alt='sample' height='auto' width='auto' style="border:2px solid black" />

* **Obligatory Fields**
    * `key1: string` - single character string specifying which key to use for option 1
    * `key2: string` - single character string specifying which key to use for option 2
    * `<key-specified in key1, e.g. f>: string` - option 1 corresponding to first key
    * `<key specified in key2, e.g. j>: string` - option 2 corresponding to second key
    * `expected: string` - which option is the correct or expected one

* **Optional Fields**
    * `question: string`
    * `picture: string (link)`
    * `canvas: object` - [more about babe canvas](05_canvas.md)

* **Sample data**

```
const key_press_trials = [
    {
        question: "What's the weather like?",
        key1: 'f',
        key2: 'j',
        f: 'shiny',
        j: 'rainbow',
        expected: 'shiny'
    },
    {
        question: "What's on the bread?",
        picture: 'path/to/picture.jpg',
        key1: 'f',
        key2: 'j',
        f: 'ham',
        j: 'jam',
        expected: 'jam'
    }
];
```

