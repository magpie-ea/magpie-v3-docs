# Template views

## Wrapping views

Wrapping views are short, usually one-trial views that structure your experiment. They can provide a welcome message, instructions or collect post-survey data. The most important wrapping view is the *thanks* view. **The _thanks_ view must always be included in your experiment because it wraps up and processes the data collected during the experiment.**

All wrapping views have two **obligatory fields**:

* `trials: int`
    * the number of trials this view will be cycled through
* `name: string`
    * the unique name of this view instance

### Intro view

Instantiate with `magpieViews.view_generator('intro', ...)`. Optional fields:

* `buttonText: string`
    * the text of the button that takes the participant to the next view
    * default: 'Next'
* `title: string`
    * the title of the view
    * default: 'Welcome!'
* `text: string`
    * the text of the view
    * default: *there is no default*

The intro view uses the following [predefined view elements](../03_custom_views/#predefined-view-elements):

* stimulus_container_generator: `fixed text`
* answer_container_generator: `one_button`
* handle_response_function: `intro`

### Instructions view

Instantiate with `magpieViews.view_generator('instructions', ...)`. Optional fields:

* `buttonText: string`
    * the text of the button that takes the participant to the next view
    * default: 'Next'
* `title: string`
    * the title of the view
    * default: 'Instructions'
* `text: string`
    * the text of the view
    * default: *there is no default*

The intro view uses the following [predefined view elements](../03_custom_views/#predefined-view-elements):

* stimulus_container_generator: `fixed text`
* answer_container_generator: `one_button`
* handle_response_function: `one_click`


### Begin view

Instantiate with `magpieViews.view_generator('begin, ...)`. Optional fields:

* `buttonText: string`
    * the text of the button that takes the participant to the next view
    * default: 'Next'
* `title: string`
    * the title of the view
    * default: 'Begin'
* `text: string`
    * the text of the view
    * default: *there is no default*
    
The begin view uses the following [predefined view elements](../03_custom_views/#predefined-view-elements):

* stimulus_container_generator: `fixed text`
* answer_container_generator: `one_button`
* handle_response_function: `one_click`

### Post_test view

Instantiate with `magpieViews.view_generator('post_test', ...)`. Optional fields:

* `buttonText: string`
    * the text of the button that takes the participant to the next view
    * default: 'Next'
* `title: string`
    * the title of the view
    * default: 'Additional Information'
* `text: string`
    * the text of the view
    * default: *there is no default*
* `age_question: string`
    * question about participant's age
    * default: 'Age',
* `gender_question: string`
    * question about participant's gender
    * default: 'Gender'
* `gender_male: string`
    * answer option for the gender question
    * default: 'male'
* `gender_female: string`
    * answer option for the gender question
    * default: 'female'
* `gender_other: string`
    * answer option for the gender question
    * default: 'other'
* `edu_question: string`
    * question about participant's level of education
    * default: 'Level of Education'
* `edu_graduated_high_school: string`
    * answer option for the education question
    * default: 'Graduated High School'
* `edu_graduated_college: string`
    * answer option for the education question
    * default: 'Graduated College'
* `edu_higher_degree: string`
    * answer option for the education question
    * default: 'Higher Degree'
* `languages_question: string`
    * question about participant's native languages
    * default: 'Native Languages'
* `languages_more: string`
    * more info about what native languages are
    * default: '(i.e. the language(s) spoken at home when you were a child)'

The post test view uses the following [predefined view elements](../03_custom_views/#predefined-view-elements):

* stimulus_container_generator: `post_test`
* answer_container_generator: `post_test`
* handle_response_function: `post_test`

### Thanks view

Instantiate with `magpieViews.view_generator('thanks', ...)`. **The _thanks_ view must always be included in your experiment because it wraps up and processes the data collected during the experiment.** Optional fields:

* `title: string`
    * the title of the view
    * default: 'Thank you for taking part in this experiment!'
* `prolificConfirmText: string`
    * text asking the participant to press the 'confirm' button
    * default: 'Please press the button below to confirm that you completed the experiment with Prolific'

The thanks view uses the following [predefined view elements](../03_custom_views/#predefined-view-elements):

* stimulus_container_generator: `empty`
* answer_container_generator: `empty`
* handle_response_function: `thanks`

## Trial views

Trial views are the parts of your experiment which are (usually) instantiated several times (realizing different trials of your, say, main experimental task). They usually collect the data and often rely on additional information (e.g., the picture to be displayed in trial 27, or the question and answer options for trial 13).

All trial views have three **obligatory fields**:

* `trials: int` 
     * the number of trials this view will appear
* `name: string`
     * the name of the view
* `data: array`
     * list of objects, each with information for each consecutive trial

Different types of wrapping views have more optional fields, as documented below. 

### Forced choice (2 alternatives)

Instantiate with `magpieViews.view_generator('forced_choice', ...)` Displays a context, a picture and a question to be answered in a two-alternative forced choice task. Choices are made by clicking on one of two buttons.

<img src='../../images/views_samples/view_fc.png' alt='sample' height='auto' width='auto' style="border:2px solid black" />

* **Obligatory Fields**
    * `option1: string` - text on button for option 1
    * `option2: string` - text on button for option 2

* **Optional Fields**
    * `question: string` - question to be asked
    * `QUD: string` - text that is always present on the slide
    * `canvas: object` - [more about magpie canvas](05_canvas.md)
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

The forced\_choice view uses the following [predefined view elements](../03_custom_views/#predefined-view-elements):

* stimulus_container_generator: `basic_stimulus`
* answer_container_generator: `button_choice`
* handle_response_function: `button_choice`

### Sentence choice 

Instantiate with `magpieViews.view_generator('sentence_choice', ...)`. Similar to the `forcedChoice` view, this view presents two text-based options to click on. This view, however, realizes options on longer buttons arranged vertically. This is better for choice between several longer expressions, like whole sentences.

<img src='../../images/views_samples/view_ss.png' alt='sample' height='auto' width='auto' style="border:2px solid black" />

* **Obligatory Fields**
    * `option1: string`
    * `option2: string`

* **Optional Fields**
    * `QUD: string` - text that is always present on the slide
    * `canvas: object` - [more about magpie canvas](05_canvas.md)
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

The sentence\_choice view uses the following [predefined view elements](../03_custom_views/#predefined-view-elements):

* stimulus_container_generator: `basic_stimulus`
* answer_container_generator: `sentence_choice`
* handle_response_function: `button_choice`

### Image selection

Instantiate with `magpieViews.view_generator('image_seletion', ...)`. Realizes another 2-alternative forced choice task, by presenting two pictures (arranged horizontally) and requiring a click on one of the pictures. 

<img src='../../images/views_samples/view_is.png' alt='sample' height='auto' width='auto' style="border:2px solid black" />

* **Obligatory Fields**
    * `option1: string` - label for choice of picture 1 (stored in `response` variable)
    * `option2: string` - label for choice of picture 2 (stored in `response` variable)
    * `picture1: string (link)` - refers to `option1`
    * `picture2: string (link)` - refers to `option2`

* **Optional Fields**
    * `QUD: string` - text that is always present on the slide
    * `question: string`
    * `canvas: object` - [more about magpie canvas](05_canvas.md)

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

The image\_selection view uses the following [predefined view elements](../03_custom_views/#predefined-view-elements):

* stimulus_container_generator: `basic_stimulus`
* answer_container_generator: `image_selection`
* handle_response_function: `button_choice`

### Textbox Input task

Instantiate with `magpieViews.view_generator('textbox_input', ...)`. Requires users to type in text freely in a textbox. Allows to specify a minimum number of characters before the `next` button appears. 

<img src='../../images/views_samples/view_ti.png' alt='sample' height='auto' width='auto' style="border:2px solid black" />

* **Obligatory Fields**
    * `question: string` - question to answer

* **Optional Fields**
    * `QUD: string` - text that is always present on the slide
    * `canvas: object` - [more about magpie canvas](05_canvas.md)
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
        question: "What's the weather like? like?",
        min_chars: 50
    }
];
```

The forced\_choice view uses the following [predefined view elements](../03_custom_views/#predefined-view-elements):

* stimulus_container_generator: `basic_stimulus`
* answer_container_generator: `textbox_input`
* handle_response_function: `textbox_input`


### Slider rating

Instantiate with `magpieViews.view_generator('slider_rating', ...)`. Gives you a single (horizontally oriented) slider, with endpoints whose labels can be specified. The `next` button only appears when the slider is clicked on or moved at least once. Internally slider values are represented as ranging from 0 to 100 in steps of 1.

<img src='../../images/views_samples/view_sr.png' alt='sample' height='auto' width='auto' style="border:2px solid black" />

* **Obligatory Fields**
    * `optionLeft: string`
    * `optionRight: string`

* **Optional Fields**
    * `QUD: string` - text that is always present on the slide
    * `canvas: object` - [more about magpie canvas](05_canvas.md)
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
        question: "What's the weather like? like?",
        optionLeft: 'shiny',
        optionRight: 'rainbow'
    }
];
```

The slider\_rating view uses the following [predefined view elements](../03_custom_views/#predefined-view-elements):

* stimulus_container_generator: `basic_stimulus`
* answer_container_generator: `slider_rating`
* handle_response_function: `slider_rating`

### Dropdown choice

Instantiate with `magpieViews.view_generator(drowdown_choice', ...)`. Prompts the user to select one option from a drop-down menu, which can be embedded into a sentence, e.g., to fill in a word or phrase in a fixed sentence frame.

<img src='../../images/views_samples/view_dc.png' alt='sample' height='auto' width='auto' style="border:2px solid black" />


* **Obligatory Fields**
    * `option1: string`
    * `option2: string`

* **Optional Fields**
    * `QUD: string` - text that is always present on the slide
    * `canvas: object` - [more about magpie canvas](05_canvas.md)
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

The dropdown\_choice view uses the following [predefined view elements](../03_custom_views/#predefined-view-elements):

* stimulus_container_generator: `basic_stimulus`
* answer_container_generator: `dropdown_choice`
* handle_response_function: `dropdown_choice`

### Rating scale

Instantiate with `magpieViews.view_generator('rating_scale', ...)`. Realizes a Likert-scale (ordinal) rating task, with button labeled with consecutive numbers. Participants click on these numbered buttons to proceed. Allows labels for endpoints on the scale.

<img src='../../images/views_samples/view_rc.png' alt='sample' height='auto' width='auto' style="border:2px solid black" />



* **Obligatory Fields**
    * `optionLeft: string`
    * `optionRight: string`

* **Optional Fields**
    * `QUD: string` - text that is always present on the slide
    * `canvas: object` - [more about magpie canvas](05_canvas.md)
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

The rating\_scale view uses the following [predefined view elements](../03_custom_views/#predefined-view-elements):

* stimulus_container_generator: `basic_stimulus`
* answer_container_generator: `rating_scale`
* handle_response_function: `button_choice`



### Key press

Instantiate with `magpieViews.view_generator('key_press', ...)`. Offers a 2-alternative forced choice task where choice options are given by pressing keys on the keyboard. Ideal for more accurate reaction time measurements.

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
    * `canvas: object` - [more about magpie canvas](05_canvas.md)

* **Sample data**

```
const key_press_trials = [
    {
        question: "What's the weather like? like?",
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

The key\_press view uses the following [predefined view elements](../03_custom_views/#predefined-view-elements):

* stimulus_container_generator: `key_press`
* answer_container_generator: `question`
* handle_response_function: `key_press`



## Self-paced reading

There are templates for realizing self-paced reading tasks too.

### Self-paced reading with forced choice response

Instantiate with `magpieViews.view_generator('self_paced_reading', ...)`.

* **Obligatory Fields**
    * `sentence: string`
        * the spr parts are separated by ' | '
    * `option1: string`
    * `option2: string`

* **Optional Fields**
    * `QUD: string` - text that is always present on the slide
    * `help_text: string`
        * instructions to press SPACE above the spr sentence lines
        * default - *Press the SPACE bar to reveal the words*
    * `picture: string`
    * `canvas: object` - [more about magpie canvas](05_canvas.md)
    * `question: string`
    * `wordPos: "next" or "same"`
        * option how to display the spr parts, if "next" every spr part is displayed next to each other, as in a sentence, if "same" every spr part is displayed at the same place
        * default: "next"
    * `underline: "words", "sentence" or "none"`
        * option how to underline the spr parts, if "words" every part is underlined separately,  if "sentence" the complete sentence is underlined, if "none" there is no underline 
        * default: "words"

* **Sample data**

```
const spr_trials = [
    {
        QUD: "Johnny says: 'I want you to bring me the box where ...",
        picture: "images/all-false3.png"
        help_text: 'just press SPACE',
        question: "Should you bring Johnny this box or not?",
        sentence: "all | of | the | yellow | marbles | are | inside | the | case.'",
        option1: "Bring it",
        option2: "Leave it",
        wordPos: "same"
    },
    {
        question: "Should you bring Johnny this box or not?",
        sentence: "some | of the | black marbles | are | inside | the case.'",
        option1: "Bring it",
        option2: "Leave it",
        underline: "none"
    }
];
```

The self\_paced\_reading view uses the following [predefined view elements](../03_custom_views/#predefined-view-elements):

* stimulus_container_generator: `self_paced_reading`
* answer_container_generator: `button_choice`
* handle_response_function: `self_paced_reading`

### Self-paced reading task with rating scale response

Instantiate with `magpieViews.view_generator('self_paced_reading_rating_scale', ...)`.


* **Obligatory Fields**
    * `sentence: string`
        * the spr parts are separated by ' | '
    * `optionLeft: string`
    * `optionRight: string`

* **Optional Fields**
    * `QUD: string` - text that is always present on the slide
    * `help_text: string` - SPACE press text above the spr sentence
    * `picture: string`
    * `canvas: object` - [more about magpie canvas](05_canvas.md)
    * `question: string`
    * `wordPos: "next" or "same"`
        * option how to display the spr parts, if "next" every spr part is displayed next to each other, as in a sentence, if "same" every spr part is displayed at the same place
        * default: "next"
    * `underline: "words", "sentence" or "none"`
        * option how to underline the spr parts, if "words" every part is underlined separately,  if "sentence" the complete sentence is underlined, if "none" there is no underline
        * default: "words"



```
const spr_rc_trials = [
    {
        QUD: "Johnny says: 'I want you to bring me the box where ...",
        picture: "images/all-false3.png"
        help_text: 'SPACEEEE',
        sentence: "all | of the | yellow marbles | are | inside | the case.'",
        question: "Should you bring Johnny this box or not?",
        optionLeft: "Bring it",
        optionRight: "Leave it",
    },
    {
        question: "Should you bring Johnny this box or not?",
        sentence: "some | of the | black marbles | are | inside | the case.'",
        optionLeft: "Bring it",
        optionRight: "Leave it"
    }
];
```

The self\_paced\_reading\_rating\_scale view uses the following [predefined view elements](../03_custom_views/#predefined-view-elements):

* stimulus_container_generator: `self_paced_reading`
* answer_container_generator: `rating_choice`
* handle_response_function: `self_paced_reading`
