# Custom views 

When the template views are not enough for your purposes, you can define your own custom views. There are two ways of doing this. The first possibility is to customize the template views. You would then keep the basic structure and design of a template view but to change some parts of it. The other is to define a completely new view from scratch.

## Customizing template views

Each template view consists of three elements, which jointly define its appearance and functionality:

1. **stimulus container**: defines the top part of the screen, containing stuff like title, the QUD, the stimulus to be shown etc.
2. **answer container**: defines the lower part of the screen, containing the answer options, 'next' buttons, etc.
3. **handle response function**: function that is called when a response is given

Each one of these elements is independently customizable. Indeed, when a template view is created by \_babe "behind the scenes", it simply uses defaults for these three elements which are appropriate for the template view to be created. For example, we would normally create a template `forced_choice` view like so:

```javascript
const forced_choice_instance = babeViews.view_generator(
    'forced_choice', 
    // config information
    {
        trials: part_one_trial_info.forced_choice.length,
        name: 'rebuilt_FC',
        data: part_one_trial_info.forced_choice
    }
    );
```

Equivalently, we could write this: 

```javascript
const forced_choice_instance = babeViews.view_generator(
   'forced_choice', 
   // config information
    {
        trials: part_one_trial_info.forced_choice.length,
        name: 'rebuilt_FC',
        data: part_one_trial_info.forced_choice
    },
    // custom generator functions
    {
        stimulus_container_generator: stimulus_container_generators.basic_stimulus,
        answer_container_generator: answer_container_generators.button_choice,
        handle_response_function: handle_response_functions.button_choice
    }
   );
```

Here, we supply a third object with custom generator functions, one for each of the three elements. Since the second code box just uses the defaults which are implicit in the first, the two calls are equivalent.

The stimulus generator function used by the template `forced_choice` view is this:

```javascript
function (config, CT) {
        return `<div class='babe-view'>
                    <h1 class='babe-view-title'>${config.title}</h1>
                    <p class='babe-view-question babe-view-qud'>${config.data[CT].QUD}</p>
                    <div class='babe-view-stimulus-container'>
                        <div class='babe-view-stimulus babe-nodisplay'></div>
                    </div>
                </div>`;
```

Generally, the function supplied to `stimulus_container_generator` must take the `config` object and a `CT` argument. The latter is the current trial, and it is supplied by \_babe internally. The function should then return HTML code as a string, which governs the appearance of the upper part of the view screen. You can use information from `config` and `CT`, so that when we write `${config.data[CT].QUD}` in the HTML string to be returned this is replaced by the QUD information stored in `config` for the current trial. Notice that the picture element is inserted automatically by \_babe in the `babe-view-stimulus` container. 

We can specify whatever we'd like to appear in the stimulus container. For example, let's insert the same picture twice. This can be done by instantiating a customized template view like so:

```javascript
const forced_choice_custonmized = babeViews.view_generator(
    "forced_choice",
    // config information
    {
        trials: part_one_trial_info.forced_choice.length,
        name: 'rebuilt_FC',
        data: part_one_trial_info.forced_choice
    },
    // custom generator functions
    {
        stimulus_container_generator: function (config, CT) {
            return `<div class='babe-view'>
                      <div class='babe-view-stimulus-container'>
                        <img src="${config.data[CT].picture}" height="42" width="42">
                        <img src="${config.data[CT].picture}" height="42" width="42">
                      </div>
                    </div>`;}
    }
);
```

We can also change the design and behavior of the part of the view that deals with the response. Per default, the `forced_choice` view uses a predefined function `button_choice` to have the user select one of two buttons. Suppose we wanted to have a third button. We could then use code like the following (which only works, if we define `option3` in `04_trials.js` as well, of course):


```javascript
const forced_choice_customized = babeViews.view_generator(
    "forced_choice",
    // config information
    {
        trials: part_one_trial_info.forced_choice.length,
        name: 'rebuilt_FC',
        data: part_one_trial_info.forced_choice
    },
    // custom generator functions
    {
      answer_container_generator: function (config, CT) {
       return `<div class='babe-view-answer-container'>
               <p class='babe-view-question'>${config.data[CT].question}</p>
               <label for='o1' class='babe-response-buttons'>${config.data[CT].option1}</label>
               <input type='radio' name='answer' id='o1' value=${config.data[CT].option1} />
               <label for='o2' class='babe-response-buttons'>${config.data[CT].option2}</label>
               <input type='radio' name='answer' id='o2' value=${config.data[CT].option2} />
               <label for='o2' class='babe-response-buttons'>${config.data[CT].option3}</label>
               <input type='radio' name='answer' id='o3' value=${config.data[CT].option3} />
               </div>`;
  }
    }
);
```

It is possible to supply a completely new type of response variable via the function specified for `answer_container_generator`. However, you might need to make sure that the data is handled correctly. This is the job of the final third element you can use to customize a template view, namely `handle_response_function`. In other words, often the two design elements `answer_container_generator` and `handle_response_function` might need to be adjusted to each other.

The `handle_response_function` used per default in the `forced_choice` view is this:

```javascript
function(config, CT, babe, answer_container_generator, startingTime) {
        $(".babe-view").append(answer_container_generator(config, CT));

        // attaches an event listener to the yes / no radio inputs
        // when an input is selected a response property with a value equal
        // to the answer is added to the trial object
        // as well as a readingTimes property with value
        $("input[name=answer]").on("change", function() {
            const RT = Date.now() - startingTime;
            let trial_data = {
                trial_name: config.name,
                trial_number: CT + 1,
                response: $("input[name=answer]:checked").val(),
                RT: RT
            };

            trial_data = babeUtils.view.save_config_trial_data(config.data[CT], trial_data);

            babe.trial_data.push(trial_data);
            babe.findNextView();
        });
}    
```


## Defining a custom view from scratch


... to be filled soon ... 
