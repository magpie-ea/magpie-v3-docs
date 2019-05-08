# Wrapping views

Wrapping views are short, usually one-trial views that structure your experiment. They can provide a welcome message, instructions or collect post-survey data. The most important wrapping view is the *thanks* view. **The _thanks_ view must always be included in your experiment because it wraps up and processes the data collected during the experiment.**

## Intro view

Instantiate with `babeViews.intro`. Optional fields:

* `buttonText: string`
    * the text of the button that takes the participant to the next view
    * default: 'Next'
* `title: string`
    * the title of the view
    * default: 'Welcome!'
* `text: string`
    * the text of the view
    * default: *there is no default*


## Instructions view

Instantiate with `babeViews.instructions`. Optional fields:

* `buttonText: string`
    * the text of the button that takes the participant to the next view
    * default: 'Next'
* `title: string`
    * the title of the view
    * default: 'Instructions'
* `text: string`
    * the text of the view
    * default: *there is no default*

## Begin view

Instantiate with `babeViews.begin`. Optional fields:

* `buttonText: string`
    * the text of the button that takes the participant to the next view
    * default: 'Next'
* `title: string`
    * the title of the view
    * default: 'Begin'
* `text: string`
    * the text of the view
    * default: *there is no default*

## PostTest view

Instantiate with `babeViews.postTest`. Optional fields:

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

## Thanks view

Instantiate with `babeViews.thanks`. **The _thanks_ view must always be included in your experiment because it wraps up and processes the data collected during the experiment.** Optional fields:

* `title: string`
    * the title of the view
    * default: 'Thank you for taking part in this experiment!'
* `prolificConfirmText: string`
    * text asking the participant to press the 'confirm' button
    * default: 'Please press the button below to confirm that you completed the experiment with Prolific'


