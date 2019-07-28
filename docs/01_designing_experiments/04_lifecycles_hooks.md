# Life cycles and hooks

All trial views have a fixed life cycle that you can use to manipulate the timing of events during a trial. You can insert custom functions, so-called 'hooks' to massage template views into style.

## Life cycles

All the trial views go through the following steps.

1. **pause step** :: show a blank screen for specified time

    * enable by passing `pause: number (in miliseconds)`
    * defaults to `pause: 0`
    * shows nothing but a blank screen and the `QUD` if there is such

2. **fixation point step** :: show fixation cross in the middle where the stimulus appears for specified time

    * enabled by setting `fix_duration: number (in miliseconds)`
    * defaults to `fix_duration: 0`
    * shows a cross in the middle of the stimulus and the `QUD` if there is such

3. **stimulus shown step** :: stimulus appears

4. **stimulus hidden step** :: hides the stimulus from the screen
    * set by passing `stim_duration: number (in miliseconds)`
    * defaults to `stim_duration: Infinity`
    * hide the stimulus when SPACE gets pressed with `stim_duration: 'space'`
    * skip this step by not defining `stim_duration`

5. **response enabled step** :: the participant can interact with the view (respond, read the sentence etc.**


**Example**

Suppose you want to add a 1000 ms inter-stimulus break between two trials and show a fixation cross for 250 ms. You could realize this when instantiating your view like this:

~~~
const forced_choice_2A = magpieViews.view_generator("key_press", {
    trials: trial_info.forced_choice.length,
    name: 'forced_choice_2A',
    data: trial_info.forced_choice,
    pause: 1000,
    fix_duration: 250
});
~~~

## Hooks

For minor customization of the template views, you can create functions, ideally in file `02_custom_functions.js` and hook these functions to specific steps in the life cycle of a trial view. The following **hooks** exist where your custom functions could attach:

* after the pause is finished
    enable with `hook.after_pause: _function_`

* after the fixation point hides
    enable with `hook.after_fix_point: _function_`

* after the stimulus is shown
    enable with `hook.after_stim_shown: _function_`

* after the stimulus hides
    enable with `hook.after_stim_hidden: _function_`

* after the interactions are enabled
    enable with `hook.after_response_enabled: _function_`


Your custom functions get the trial `data` for each trial view and `next` as arguments. You can use the `data` if you need to. To proceed to the next step of the lifecycle, you have to call `next()`

**Full lifecycle - hook sample**

1. pause shows
2. pause finishes
3. after_pause function called
4. fixation point shows
5. fixation point disappears
6. after_fix_point function called
7. stimulus shows
8. after_stim_shown function called
9. stimlus hides
10. after_stim_hidden function called
11. response is enabled
12. after_response_enabled function called

**Example: response check**

Imagine you want to tell the participants whether their choice was right or wrong, e.g., during practice trials. We could realize this with a function  which checks the answer for correctness which is hooked to the "response-enabled step". Of course, your data for each trial needs to include the information which answer is correct. For the Departure-Point example, we could add this information to `04_trials.js` like so:

```javascript
    forced_choice: [
        {
            question: "What's on the bread?",
            picture: "images/question_mark_02.png",
            option1: 'jam',
            option2: 'ham',
            correct: 'jam' // which option is correct?
        },
        {
            question: "What's the weather like?",
            picture: "images/weather.jpg",
            option1: "shiny",
            option2: "rainbow",
            correct: "shiny" // which option is correct?
        }
    ]

```

We then define the function to be hooked, ideally in file `02_custom_functions.js`:

```
// compares the chosen answer to the value of `option1`
check_response = function(data, next) {
    $('input[name=answer]').on('change', function(e) {
        if (e.target.value === data.correct) {
            alert('Your answer is correct! Yey!');
        } else {
            alert('Sorry, this answer is incorrect :( The correct answer was ' + data.correct);
        }
        next();
    })
}
```

We then add this function to be called after the relevant step when creating the relevant view:

```
const forced_choice_2A = magpieViews.view_generator("key_press", {
    trials: trial_info.forced_choice.length,
    name: 'forced_choice_2A',
    data: trial_info.forced_choice,
    hook: {
        after_response_enabled: check_response
    }

});
```
