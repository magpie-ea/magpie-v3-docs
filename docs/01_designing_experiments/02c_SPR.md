# Self-paced reading

There are templates for realizing self-paced reading tasks too.

## Self-paced reading with forced choice response

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
    },
    {
        question: "Should you bring Johnny this box or not?",
        sentence: "some | of the | black marbles | are | inside | the case.'",
        option1: "Bring it",
        option2: "Leave it"
    }
];
```

## Self-paced reading task with rating scale response



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
