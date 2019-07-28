# Progress bars

\_magpie provides the option to include progress bars in selected views. For example, when you have defined views for variables `practice` and `main`, you can include progress bars for these views by specifying this information during the creation of the \_magpie object, like so:

```
$("document").ready(function() {
    magpieInit({
        ...
        progress_bar: {
            in: [
                practice.name,
                main.name
            ],                  // only the practice and the main view will have progress bars in this experiment
            style: "chunks",    // there will be two chunks - one for the practice and one for the main view
            width: 100          // each one of the two chunks will be 100 pixels long
        }
    });
});
```

Notice that the array `progress.in` requires not the variables, but the view's names (as strings). It is therefore important that each view receives its own unique name.

You can use one of the following 3 styles (see pictues below):

* `separate` - shows a single bar which tracks progress within the current view
* `default` - shows a single bar which tracks progress within the whole experiment (all views designated for progress tracking)
* `chunks` - shows multiple bars, one for each progress-tracked view

Use `progress_bar.width` to set the width (in pixels** of the progress bars.


**Examples**

```
progress_bar: {
    in: [
        forced_choice.name,    // 6 trials
        dropdown_choice.name   // 10 trials
        slider_rating.name,    // 4 trials
    ],
    style: "default",
    width: 120          // 120 pixels
}


// 20 trials overall, each trial fills 10 pixels (120/20) part of the progress bar
```

<img src='../../images/progress_samples/pb_default.png' alt='progress bar sample' height='auto' width='600' />


```
progress_bar: {
    in: [
        forced_choice.name,    // 6 trials
        dropdown_choice.name   // 10 trials
        slider_rating.name,    // 4 trials
    ],
    style: "separate",
    width: 120          // 120 pixels
}


// 20 trials overall, each trial fills (120/total trials) pixels part of the progress bar. Each type of view has a separate progress bar.
```

<img src='../../images/progress_samples/pb_separate.png' alt='progress bar sample' height='auto' width='600' />

```
progress_bar: {
    in: [
        forced_choice.name,    // 6 trials
        dropdown_choice.name   // 10 trials
        slider_rating.name,    // 4 trials
    ],
    style: "chunks",
    width: 60           // 60 pixels
}


// 20 trials overall, each trial fills part of its corresponding chunk. Each type of view has a separate progress bar and all progress bars are displyed.
```

<img src='../../images/progress_samples/pb_chunks.png' alt='progress bar sample' height='auto' width='600' />



