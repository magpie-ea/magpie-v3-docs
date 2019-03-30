All trial views have a fixed life cycle that you can use to manipulate the timing of events during a trial. You can insert custom functions, so-called 'hooks' to massage template views into style.

## Life cycle

All the trial views go through 4 steps.

1. pause step - blank screen

    * enable by passing `pause: number (in miliseconds)` (will show a pause for after number amount of time)
    * shows nothing but a blank screen and the `QUD` if there is such

2. fixation point step - a cross in the middle where the stimulus appears

    * passed to the trial view as `fix_duration: number (in miliseconds)`
    * shows a cross in the middle of the stimulus and the `QUD` if there is such

3. stimulus shown step - stimulus appears

3.5 (optional) stimulus hidden step - hides the stimulus from the screen
    * hide the stimulus after certain amount of time by passing `stim_duration: number (in miliseconds)` to the view creation
    * hide the stimulus when SPACE gets pressed with `stim_duration: 'space'`
    * skip this step by not defining `stim_duration`

4. interactions are enabled - the participant can interact with the view (respond, read the sentence etc.)

The views you created do not need to use these timeouts, however, each trial view still goes through these steps on the background and you can still [hook](#trial-view-hooks) and call locally defined functions

## Hooks

You can create functions in your local js files and hook these functions to the trial view. To understand how hooks work, first learn about babe's [trial views lifecycle](#trial-views-lifecycle)

**Hooks**

* after the pause is finished
    enable with `hook.after_pause: _function_`

* after the fixation point hides
    enable with `hook.after_fin_point: _function_`

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

**Real example**

Imagine you want to tell the participants whether their repsonse was correct while they are getting familiar with the experiment, for example in the practice trial view. To do that you need to get the answer they chose and check it for correctness. You can define a funciton that gets their response and hook to the trial view after the response is enabled. In the example below, assume that `option1` is always the correct answer.


```
// compares the chosen answer to the value of `option1`
function checkResponse(data, next) {
    $('input[name=answer]').on('change', function(e) {
        if (e.target.value === data.option1) {
            alert('Your answer is correct! Yey!');
        } else {
            alert('Sorry, this answer is incorrect :(');
        }
        next();
    })
}
```

and add a `after_response_enabled` hook to the view:

```
const practice = babeViews.forcedChoice({
    trials: 20,
    name: 'practice',
    trial_type: 'practice',
    data: practice_trials.forcedChoice,
    fix_duration: 500,
    pause: 500,
    hook: {
        after_response_enabled: checkResponse
    }
});
```
