# Custom views 

When the template views are not enough for your purposes, you can define your own custom views. There are two ways of doing this. The first possibility is to [customize the template views](#customizing-template-views). You would then keep the basic structure and design of a template view and change selected parts of it. Since all predefined template views consist of modular [predefined view elements](#predefined-view-elements), you can often modularly assemble and tweak your desired view functionality based on slight changes of the template views. The other, more radical method is to [define a completely new view from scratch](#defining-a-custom-view-from-scratch).

## Customizing template views

Each template view consists of three elements, which jointly define its appearance and functionality:

1. **stimulus container**: defines the top part of the screen, containing stuff like title, the QUD, the stimulus to be shown etc.
2. **answer container**: defines the lower part of the screen, containing the answer options, 'next' buttons, etc.
3. **handle response function**: function that is called to handle the response

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

Here, we supply a third object with custom generator functions, one for each of the three elements. Since the second code box just uses the defaults which are implicit in the first, the two calls are equivalent. The description of [template views](../01_template_views/) lists which view elements each template view uses. All predefined view elements are listed and shown [below](#predefined-view-elements).

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

    // create the answer container
    $(".babe-view").append(answer_container_generator(config, CT));
    
    // attaches an event listener to the radio button input
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

When this function is called (automatically by \_babe) it first creates the answer container. It also adds an event listener. Here, it reacts when a button is pressed, but this behavior needs to be coordinated with whatever you define in the `answer_container_generator` part. It then records the trial time and assembles the data to be recorded in the variable `trial_data`. By default, \_babe automatically adds all the data for the current trial to this object and then pushes this to its internal representation of the accumulated data so far which is in `babe.trial_data`. The function `babe.findNextView` is then called and takes you to the next trial or view.

When you write your own custom `handle_response_function` you should supply the same arguments as the function above. You should also make sure to record the data and call `babe.findNextView` eventually. Other than this, you could implement a more dynamic display of different pictures and response options, using the interplay of `answer_container_generator` and `handle_response_function`, for example.

If you write longer custom functions for customization, it is good practice to put these into the file `02_custom_functions.js`. An example is presented in the [showroom](https://github.com/babe-project/showroom).

## Predefined view elements

### Stimulus container generators

The following predefined stimulus container generators are available. The template [wrapping views](../01_template_views/#wrapping-views) and [trial views](../01_template_views/#trial-views) defined in \_babe all use one of these, as listed [here](../01_template_views/).

```javascript
// The view template dict contains a generator function for every view type we support
// The generator gets the config dict and CT as input and will generate the babe-view HTML code
// (Some view templates are the same, e.g. forced_choice and sliderRating)
const stimulus_container_generators = {
    basic_stimulus: function (config, CT) {
        return `<div class='babe-view'>
                    <h1 class='babe-view-title'>${config.title}</h1>
                    <p class='babe-view-question babe-view-qud'>${config.data[CT].QUD}</p>
                    <div class='babe-view-stimulus-container'>
                        <div class='babe-view-stimulus babe-nodisplay'></div>
                    </div>
                </div>`;
    },
    key_press: function(config, CT) {
        return `<div class="babe-view">
                    <h1 class='babe-view-title'>${config.title}</h1>
                    <p class='babe-response-keypress-header'>
                    <strong>${config.data[CT].key1}</strong> = ${config.data[CT][config.data[CT].key1]}, 
                    <strong>${config.data[CT].key2}</strong> = ${config.data[CT][config.data[CT].key2]}</p>
                    <div class='babe-view-stimulus-container'>
                        <div class='babe-view-stimulus babe-nodisplay'></div>
                    </div>
                </div>`;
    },
    fixed_text: function(config, CT) {
        return `<div class='babe-view'>
                    <h1 class='babe-view-title'>${config.title}</h1>
                    <section class="babe-text-container">
                        <p class="babe-view-text">${config.text}</p>
                    </section>
                </div>`;
    },
    post_test: function(config, CT) {
        return `<div class='babe-view babe-post-test-view'>
                    <h1 class='babe-view-title'>${config.title}</h1>
                    <section class="babe-text-container">
                        <p class="babe-view-text">${config.text}</p>
                    </section>
                </div>`;
    },
    empty: function(config, CT) {
        return ``;
    },
    self_paced_reading: function(config, CT) {
        const helpText = config.data[CT].help_text !== undefined ?
            config.data[CT].help_text : "Press the SPACE bar to reveal the words";
        return `<div class='babe-view'>
                    <h1 class='babe-view-title'>${config.title}</h1>
                    <p class='babe-view-question babe-view-qud'>${config.data[CT].QUD}</p>
                    <div class='babe-view-stimulus-container'>
                        <div class='babe-view-stimulus babe-nodisplay'></div>
                    </div>
                    <p class='babe-help-text babe-nodisplay'>${helpText}</p>
                    <p class='babe-spr-sentence'></p>
                </div>`;
    }
};
```

### Answer container generators

The following predefined answer container generators are available. The template [wrapping views](../01_template_views/#wrapping-views) and [trial views](../01_template_views/#trial-views) defined in \_babe all use one of these, as listed [here](../01_template_views/).

```javascript
// The answer container dict contains a generator function for every view type we support
// The generator gets the config dict and CT as input and will generate the babe-view-answer-container HTML code
// (Some answer container elements should be the same, e.g. slider rating and SPR-slider rating)
const answer_container_generators = {
    button_choice: function (config, CT) {
        return `<div class='babe-view-answer-container'>
                    <p class='babe-view-question'>${config.data[CT].question}</p>
                    <label for='o1' class='babe-response-buttons'>${config.data[CT].option1}</label>
                    <input type='radio' name='answer' id='o1' value=${config.data[CT].option1} />
                    <input type='radio' name='answer' id='o2' value=${config.data[CT].option2} />
                    <label for='o2' class='babe-response-buttons'>${config.data[CT].option2}</label>
                </div>`;
    },
    question: function(config, CT) {
        return `<div class='babe-view-answer-container'>
                        <p class='babe-view-question'>${config.data[CT].question}</p>`;
    },
    one_button: function (config, CT) {
        return `<button id="next" class='babe-view-button' class="babe-nodisplay">${
            config.button
            }</button>`
    },
    post_test: function(config, CT) {
        const quest = babeUtils.view.fill_defaults_post_test(config);
        return `<form>
                    <p class='babe-view-text'>
                        <label for="age">${quest.age.title}:</label>
                        <input type="number" name="age" min="18" max="110" id="age" />
                    </p>
                    <p class='babe-view-text'>
                        <label for="gender">${quest.gender.title}:</label>
                        <select id="gender" name="gender">
                            <option></option>
                            <option value="${quest.gender.male}">${quest.gender.male}</option>
                            <option value="${quest.gender.female}">${quest.gender.female}</option>
                            <option value="${quest.gender.other}">${quest.gender.other}</option>
                        </select>
                    </p>
                    <p class='babe-view-text'>
                        <label for="education">${quest.edu.title}:</label>
                        <select id="education" name="education">
                            <option></option>
                            <option value="${quest.edu.graduated_high_school}">${quest.edu.graduated_high_school}</option>
                            <option value="${quest.edu.graduated_college}">${quest.edu.graduated_college}</option>
                            <option value="${quest.edu.higher_degree}">${quest.edu.higher_degree}</option>
                        </select>
                    </p>
                    <p class='babe-view-text'>
                        <label for="languages" name="languages">${quest.langs.title}:<br /><span>${quest.langs.text}</</span></label>
                        <input type="text" id="languages"/>
                    </p>
                    <p class="babe-view-text">
                        <label for="comments">${quest.comments.title}</label>
                        <textarea name="comments" id="comments" rows="6" cols="40"></textarea>
                    </p>
                    <button id="next" class='babe-view-button'>${config.button}</button>
            </form>`
    },
    empty: function(config, CT) {
        return ``;
    },
    slider_rating: function(config, CT) {
        const option1 = config.data[CT].optionLeft;
        const option2 = config.data[CT].optionRight;
        return `<p class='babe-view-question'>${config.data[CT].question}</p>
                <div class='babe-view-answer-container'>
                    <span class='babe-response-slider-option'>${option1}</span>
                    <input type='range' id='response' class='babe-response-slider' min='0' max='100' value='50'/>
                    <span class='babe-response-slider-option'>${option2}</span>
                </div>
                <button id="next" class='babe-view-button babe-nodisplay'>Next</button>`;
    },
    textbox_input: function(config, CT) {
        return `<p class='babe-view-question'>${config.data[CT].question}</p>
                    <div class='babe-view-answer-container'>
                        <textarea name='textbox-input' rows=10 cols=50 class='babe-response-text' />
                    </div>
                    <button id='next' class='babe-view-button babe-nodisplay'>next</button>`;
    },
    dropdown_choice: function(config, CT) {
        const question_left_part =
            config.data[CT].question_left_part === undefined ? "" : config.data[CT].question_left_part;
        const question_right_part =
            config.data[CT].question_right_part === undefined ? "" : config.data[CT].question_right_part;
        const option1 = config.data[CT].option1;
        const option2 = config.data[CT].option2;
        return `<div class='babe-view-answer-container babe-response-dropdown'>
                    ${question_left_part}
                    <select id='response' name='answer'>
                        <option disabled selected></option>
                        <option value=${option1}>${option1}</option>
                        <option value=${option2}>${option2}</option>
                    </select>
                    ${question_right_part}
                    </p>
                    <button id='next' class='babe-view-button babe-nodisplay'>Next</button>
                </div>`;

    },
    rating_scale: function(config, CT) {
        return `<p class='babe-view-question'>${config.data[CT].question}</p>
                <div class='babe-view-answer-container'>
                    <strong class='babe-response-rating-option babe-view-text'>${config.data[CT].optionLeft}</strong>
                    <label for="1" class='babe-response-rating'>1</label>
                    <input type="radio" name="answer" id="1" value="1" />
                    <label for="2" class='babe-response-rating'>2</label>
                    <input type="radio" name="answer" id="2" value="2" />
                    <label for="3" class='babe-response-rating'>3</label>
                    <input type="radio" name="answer" id="3" value="3" />
                    <label for="4" class='babe-response-rating'>4</label>
                    <input type="radio" name="answer" id="4" value="4" />
                    <label for="5" class='babe-response-rating'>5</label>
                    <input type="radio" name="answer" id="5" value="5" />
                    <label for="6" class='babe-response-rating'>6</label>
                    <input type="radio" name="answer" id="6" value="6" />
                    <label for="7" class='babe-response-rating'>7</label>
                    <input type="radio" name="answer" id="7" value="7" />
                    <strong class='babe-response-rating-option babe-view-text'>${config.data[CT].optionRight}</strong>
                </div>`;
    },
    sentence_choice: function(config, CT) {
        return `<div class='babe-view-answer-container'>
                    <p class='babe-view-question'>${config.data[CT].question}</p>
                    <label for='s1' class='babe-response-sentence'>${config.data[CT].option1}</label>
                    <input type='radio' name='answer' id='s1' value="${config.data[CT].option1}" />
                    <label for='s2' class='babe-response-sentence'>${config.data[CT].option2}</label>
                    <input type='radio' name='answer' id='s2' value="${config.data[CT].option2}" />
                </div>`;
    },
    image_selection: function(config, CT) {
        $(".babe-view-stimulus-container").addClass("babe-nodisplay");
        return    `<div class='babe-view-answer-container'>
                        <p class='babe-view-question'>${config.data[CT].question}</p>
                        <label for="img1" class='babe-view-picture babe-response-picture'><img src=${config.data[CT].picture1}></label>
                        <input type="radio" name="answer" id="img1" value="${config.data[CT].option1}" />
                        <input type="radio" name="answer" id="img2" value="${config.data[CT].option2}" />
                        <label for="img2" class='babe-view-picture babe-response-picture'><img src=${config.data[CT].picture2}></label>
                    </div>`;
    }
};
```

### Handle response functions

The following predefined handle response functions are available. The template [wrapping views](../01_template_views/#wrapping-views) and [trial views](../01_template_views/#trial-views) defined in \_babe all use one of these, as listed [here](../01_template_views/).

```javascript
// The enable response dict contains a generator function for every view type we support
// The generator gets the config dict, CT, the answer_container_generator and the startingTime as input
const handle_response_functions = {
    button_choice: function(config, CT, babe, answer_container_generator, startingTime) {
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
    },
    key_press: function (config, CT, babe, answer_container_generator, startingTime) {

        $(".babe-view").append(answer_container_generator(config, CT));

        const handleKeyPress = function(e) {
            const keyPressed = String.fromCharCode(
                e.which
            ).toLowerCase();

            if (keyPressed === config.data[CT].key1 || keyPressed === config.data[CT].key2) {
                let correctness;
                const RT = Date.now() - startingTime; // measure RT before anything else

                if (
                    config.data[CT].expected ===
                    config.data[CT][keyPressed.toLowerCase()]
                ) {
                    correctness = "correct";
                } else {
                    correctness = "incorrect";
                }

                let trial_data = {
                    trial_name: config.name,
                    trial_number: CT + 1,
                    key_pressed: keyPressed,
                    correctness: correctness,
                    RT: RT
                };

                trial_data[config.data[CT].key1] =
                    config.data[CT][config.data[CT].key1];
                trial_data[config.data[CT].key2] =
                    config.data[CT][config.data[CT].key2];

                trial_data = babeUtils.view.save_config_trial_data(config.data[CT], trial_data);

                babe.trial_data.push(trial_data);
                $("body").off("keydown", handleKeyPress);
                babe.findNextView();
            }
        };

        $("body").on("keydown", handleKeyPress);
    },
    intro: function(config, CT, babe, answer_container_generator, startingTime) {

        $(".babe-view").append(answer_container_generator(config, CT));

        let prolificId;
        const prolificForm = `<p id="prolific-id-form">
                    <label for="prolific-id">Please, enter your Prolific ID</label>
                    <input type="text" id="prolific-id" />
                </p>`;

        const next = $("#next");

        function showNextBtn() {
            if (prolificId.val().trim() !== "") {
                next.removeClass("babe-nodisplay");
            } else {
                next.addClass("babe-nodisplay");
            }
        }

        if (babe.deploy.deployMethod === "Prolific") {
            $(".babe-text-container").append(prolificForm);
            next.addClass("babe-nodisplay");
            prolificId = $("#prolific-id");

            prolificId.on("keyup", function() {
                showNextBtn();
            });

            prolificId.on("focus", function() {
                showNextBtn();
            });
        }

        // moves to the next view
        next.on("click", function() {
            if (babe.deploy.deployMethod === "Prolific") {
                babe.global_data.prolific_id = prolificId.val().trim();
            }

            babe.findNextView();
        });
    },
    one_click: function(config, CT, babe, answer_container_generator, startingTime) {

        $(".babe-view").append(answer_container_generator(config, CT));

        $("#next").on("click", function() {
            babe.findNextView();
        });
    },
    post_test: function(config, CT, babe, answer_container_generator, startingTime) {
        $(".babe-view").append(answer_container_generator(config, CT));

        $("#next").on("click", function(e) {
            // prevents the form from submitting
            e.preventDefault();

            // records the post test info
            babe.global_data.age = $("#age").val();
            babe.global_data.gender = $("#gender").val();
            babe.global_data.education = $("#education").val();
            babe.global_data.languages = $("#languages").val();
            babe.global_data.comments = $("#comments")
            .val()
            .trim();
            babe.global_data.endTime = Date.now();
            babe.global_data.timeSpent =
                (babe.global_data.endTime -
                    babe.global_data.startTime) /
                60000;

            // moves to the next view
            babe.findNextView();
        });
    },
    thanks: function(config, CT, babe, answer_container_generator, startingTime) {
        const prolificConfirmText = babeUtils.view.setter.prolificConfirmText(config.prolificConfirmText,
            "Please press the button below to confirm that you completed the experiment with Prolific");
        if (
            babe.deploy.is_MTurk ||
            babe.deploy.deployMethod === "directLink" ||
            babe.deploy.deployMethod === "localServer"
        ) {
            // updates the fields in the hidden form with info for the MTurk's server
            $("#main").html(
                `<div class='babe-view babe-thanks-view'>
                            <h2 id='warning-message' class='babe-warning'>Submitting the data
                                <p class='babe-view-text'>please do not close the tab</p>
                                <div class='babe-loader'></div>
                            </h2>
                            <h1 id='thanks-message' class='babe-thanks babe-nodisplay'>${
                    config.title
                    }</h1>
                        </div>`
            );
        } else if (babe.deploy.deployMethod === "Prolific") {
            $("#main").html(
                `<div class='babe-view babe-thanks-view'>
                            <h2 id='warning-message' class='babe-warning'>Submitting the data
                                <p class='babe-view-text'>please do not close the tab</p>
                                <div class='babe-loader'></div>
                            </h2>
                            <h1 id='thanks-message' class='babe-thanks babe-nodisplay'>${
                    config.title
                    }</h1>
                            <p id='extra-message' class='babe-view-text babe-nodisplay'>
                                ${prolificConfirmText}
                                <a href="${
                    babe.deploy.prolificURL
                    }" class="babe-view-button prolific-url">Confirm</a>
                            </p>
                        </div>`
            );
        } else if (babe.deploy.deployMethod === "debug") {
            $("main").html(
                `<div id='babe-debug-table-container' class='babe-view babe-thanks-view'>
                            <h1 class='babe-view-title'>Debug Mode</h1>
                        </div>`
            );
        } else {
            console.error("No such babe.deploy.deployMethod");
        }

        babe.submission.submit(babe);
    },
    slider_rating: function(config, CT, babe, answer_container_generator, startingTime){
        let response;

        $(".babe-view").append(answer_container_generator(config, CT));

        response = $("#response");
        // checks if the slider has been changed
        response.on("change", function() {
            $("#next").removeClass("babe-nodisplay");
        });
        response.on("click", function() {
            $("#next").removeClass("babe-nodisplay");
        });

        $("#next").on("click", function() {
            const RT = Date.now() - startingTime; // measure RT before anything else
            let trial_data = {
                trial_name: config.name,
                trial_number: CT + 1,
                response: response.val(),
                RT: RT
            };

            trial_data = babeUtils.view.save_config_trial_data(config.data[CT], trial_data);

            babe.trial_data.push(trial_data);
            babe.findNextView();
        });
    },
    textbox_input: function(config, CT, babe, answer_container_generator, startingTime) {
        let next;
        let textInput;
        const minChars = config.data[CT].min_chars === undefined ? 10 : config.data[CT].min_chars;

        $(".babe-view").append(answer_container_generator(config, CT));

        next = $("#next");
        textInput = $("textarea");

        // attaches an event listener to the textbox input
        textInput.on("keyup", function() {
            // if the text is longer than (in this case) 10 characters without the spaces
            // the 'next' button appears
            if (textInput.val().trim().length > minChars) {
                next.removeClass("babe-nodisplay");
            } else {
                next.addClass("babe-nodisplay");
            }
        });

        // the trial data gets added to the trial object
        next.on("click", function() {
            const RT = Date.now() - startingTime; // measure RT before anything else
            let trial_data = {
                trial_name: config.name,
                trial_number: CT + 1,
                response: textInput.val().trim(),
                RT: RT
            };

            trial_data = babeUtils.view.save_config_trial_data(config.data[CT], trial_data);

            babe.trial_data.push(trial_data);
            babe.findNextView();
        });
    },
    dropdown_choice: function(config, CT, babe, answer_container_generator, startingTime){
        let response;

        const question_left_part =
            config.data[CT].question_left_part === undefined ? "" : config.data[CT].question_left_part;
        const question_right_part =
            config.data[CT].question_right_part === undefined ? "" : config.data[CT].question_right_part;

        $(".babe-view").append(answer_container_generator(config, CT));

        response = $("#response");

        response.on("change", function() {
            $("#next").removeClass("babe-nodisplay");
        });


        $("#next").on("click", function() {
            const RT = Date.now() - startingTime; // measure RT before anything else
            let trial_data = {
                trial_name: config.name,
                trial_number: CT + 1,
                question: question_left_part.concat("...answer here...").concat(question_right_part),
                response: response.val(),
                RT: RT
            };

            trial_data = babeUtils.view.save_config_trial_data(config.data[CT], trial_data);

            babe.trial_data.push(trial_data);
            babe.findNextView();
        });
    },
    self_paced_reading: function(config, CT, babe, answer_container_generator, startingTime){

        const sentenceList = config.data[CT].sentence.trim().split(" | ");
        let spaceCounter = 0;
        let wordList;
        let readingTimes = [];
        // wordPos "next" or "same", if "next" words appear next to each other, if "same" all words appear at the same place
        // default: "next"
        let wordPos = config.data[CT].wordPos === undefined ? "next" : config.data[CT].wordPos;
        let showNeighbor = wordPos === "next";
        // underline "words", "sentence" or "none", if "words" every word gets underlined, if "sentence" the sentence gets
        // underlined, if "none" there is no underline
        // default: "words"
        let underline = config.data[CT].underline === undefined ? "words" : config.data[CT].underline;
        let not_underline = underline === "none";
        let one_line = underline === "sentence";

        // shows the sentence word by word on SPACE press
        const handle_key_press = function(e) {

            if (e.which === 32 && spaceCounter < sentenceList.length) {
                if (showNeighbor) {
                    wordList[spaceCounter].classList.remove("spr-word-hidden");
                } else {
                    $(".babe-spr-sentence").html(`<span class='spr-word'>${sentenceList[spaceCounter]}</span>`);
                    if (not_underline){
                        $('.babe-spr-sentence .spr-word').addClass('no-line');
                    }
                }

                if (spaceCounter === 0) {
                    $(".babe-help-text").addClass("babe-invisible");
                }

                if (spaceCounter > 0 && showNeighbor) {
                    wordList[spaceCounter - 1].classList.add("spr-word-hidden");
                }

                readingTimes.push(Date.now());
                spaceCounter++;
            } else if (e.which === 32 && spaceCounter === sentenceList.length) {
                if (showNeighbor) {
                    wordList[spaceCounter - 1].classList.add("spr-word-hidden");
                } else {
                    $(".babe-spr-sentence").html("");
                }

                $(".babe-view").append(answer_container_generator(config, CT));

                $("input[name=answer]").on("change", function() {
                    const RT = Date.now() - startingTime;
                    let reactionTimes = readingTimes
                    .reduce((result, current, idx) => {
                        return result.concat(
                            readingTimes[idx + 1] - readingTimes[idx]
                        );
                    }, [])
                    .filter((item) => isNaN(item) === false);
                    let trial_data = {
                        trial_name: config.name,
                        trial_number: CT + 1,
                        response: $("input[name=answer]:checked").val(),
                        reaction_times: reactionTimes,
                        time_spent: RT
                    };

                    trial_data = babeUtils.view.save_config_trial_data(config.data[CT], trial_data);

                    babe.trial_data.push(trial_data);
                    babe.findNextView();
                });
                readingTimes.push(Date.now());
                spaceCounter++;
            }
        };
        // shows the help text
        $(".babe-help-text").removeClass("babe-nodisplay");

        if (showNeighbor) {
            // creates the sentence
            sentenceList.map((word) => {
                $(".babe-spr-sentence").append(
                    `<span class='spr-word spr-word-hidden'>${word}</span>`
                );
            });

            // creates an array of spr word elements
            wordList = $(".spr-word").toArray();
        }

        if (not_underline){
            $('.babe-spr-sentence .spr-word').addClass('no-line');
        }
        if (one_line){
            $('.babe-spr-sentence .spr-word').addClass('one-line');
        }

        // attaches an eventListener to the body for space
        $("body").on("keydown", handle_key_press);

    }
};
```

## Defining a custom view from scratch

You can also define a custom view from scratch. To do this, create a view template, ideally in file `03_custom_views_templates.js`. A view template gets a `config` object information as input (with relevant information, e.g., URLs for pictures, text to show on each trial etc.). The view template function then returns a view.

Here's a basic scaffolding of a view template:

```javascript
const custom_view_template = function(config) {
    const view = {
        name: config.name,
        CT: 0,
        trials: config.trials,
        // The render functions gets the babe object as well as the current trial in view counter as input
        render: function (CT, babe) {
            // Here, you can do whatever you want, eventually you should call babe.findNextView()
            // to proceed to the next view and if it is an trial type view,
            // you should save the trial information with babe.trial_data.push(trial_data)
        }
    };
    // We have to return the view, so that it can be used in 05_views.js
    return view;
};
```

A view is an object, that obligatorily has the properties:

* `name`: the view's name
* `CT`: the view's current trial (the counter of how many times this view occurred in the experiment, initialized to 0)
* `trials`: the maximum number of times this view is repeated
* `render`: a function that is called to create each trial of the view

The most important part is the `render` function. It gets `CT` and the babe-object itself as input.  has to call `babe.findNextView()` eventually to proceed to the next view (or the next trial in this view). If data is to be saved from any given trial, you would need to collect the data as an object, e.g., named `trial_data` and store it for later output by calling `babe.trial_data.push(trial_data)`.

Here's a full example from the [showroom](https://github.com/babe-project/showroom):

```javascript
// In this view the user can click on one of two buttons
const custom_press_a_button = function(config) {
    const view = {
        name: config.name,
        CT: 0,
        trials: config.trials,
        // The render functions gets the babe object as well as the current trial in view counter as input
        render: function (CT, babe) {
            // Here, you can do whatever you want, eventually you should call babe.findNextView()
            // to proceed to the next view and if it is an trial type view,
            // you should save the trial information with babe.trial_data.push(trial_data)

            // Normally, you want to display some kind of html, to do this you append your html to the main element
            // You could use one of our predefined html-templates, with (babe.)stimulus_container_generators["<view_name>"](config, CT)
            $("main").html(`<div class='babe-view'>
                <h1 class='babe-view-title'>Click on one of the buttons!</h1>
                <button id="first" class='babe-view-button'>First Button</button>
                <button id="second" class='babe-view-button'>Second Button</button>
                </div>`);

            // This function will handle  the response
            const handle_click = function(e) {
                // We will just save the response and continue to the next view
                let trial_data = {
                    trial_name: config.name,
                    trial_number: CT + 1,
                    response: e.target.id
                };
                // Often it makes sense to also save the config information
                // trial_data = babeUtils.view.save_config_trial_data(config.data[CT], trial_data);

                // Here, we save the trial_data
                babe.trial_data.push(trial_data);

                // Now, we will continue with the next view
                babe.findNextView();
            };

            // We will add the handle_click functions to both buttons
            $('#first').on("click", handle_click);
            $('#second').on("click", handle_click);

            // That's everything for this view
        }
    };
    // We have to return the view, so that it can be used in 05_views.js
    return view;
};
```



