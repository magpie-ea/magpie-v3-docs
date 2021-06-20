# Basics

## Screens & Slides

Experiments in _magpie follow a certain conceptual structure, as visualized in the following schema, the basic ingredients of which are **screens** and **slides**.

<img src="../../images/getting_started/experiments_schema.png" alt="Experiments schema" />

Every experiment is composed of a series of **screens** that participants will go through sequentially.
There is often a "welcome screen", or an "instructions screen" with information for the participants.
And there will likely be one or several kinds of screens realizing the actual experimental trials which collect the data (more on this below).
The same kind of screen can be repeated (looped), as is often the case in experiments with multiple experimental trials of the same kind.

**TODO: insert picture**

[comment]: <> (TODO: insert an example, e.g., two pictures of two trials, e.g., showing a cat and a dog picture respectively and some question or rating)

A screen may consist of one or more **slides**. 
Slides simplify dynamic presentation of more intricate tasks.
For example, in many psychological experiments a single trial may consist of different stages, often carefully timed, often presented in research papers like in the picture below.
Each stage of such a trial can be realized as a slide in _magpie.

**TODO: insert picture**

[comment]: <> (TODO: insert a picture like in Figure 1 of Todd & Marois (Nature, "Capacity limit of visual short-term memory in human posterior parietal cortex") to showcase what the slides do; maybe even insert exactly this figure from the paper with full reference to it)

To ease the realization of the most common experimental designs, there are many **built-in screen components** ready for usage, allowing the quick assembly of simple experiments. 
These screens always have 4 slides, which implement:

1. A **pause**
2. A **fixation** point
3. **Stimulus** presentation
4. **Response** collection

The duration of these slides and other parameters are freely configurable in these screen components.

## Data in, data out

A _magpie experiment reads in the **data** used to build the experiment, usually termed "independent variables" in psychological literature, in the form of a CSV file.
This could be textual/visual/auditory stimuli that will be presented to the participants or possible choices that the participants can make.

**TODO: show a minimal example; 3-4 columns and 3-4 rows of a mock CSV file**

The reason to run an experiment is, of course, to collect some data.
Data collected by a _magpie experiment is of two kinds: 

1. **trial-level data**: these are measurements made in individual trials, e.g., the reaction times for the first, second ... trial the participant saw
2. **global, experiment-level data**: this is information, like screen resolution, a participant's age or the experimental group that a given participant was allocated to, which is globally fixed across the experiment.

Corresponding to these two kinds of data, _magpie experiments have two internal data representations `trialData` and `expData`, which you can add information to at any time. 
At the end of the experiment, the data from `trialData` and `expData` will be merged, so as to give a [tidy rectangular data representation](https://r4ds.had.co.nz/tidy-data.html) (see example below).

**TODO: show a minimal example of final data with what is obviously trial data (RT, response) and global data (screen resolution, age)**

At the end of an experiment, you can display all the data that was collected on the screen, which is handy for debugging.
For a live experiment, _magpie instead sends all collected data to the backend, thereby automatically adding additional information (e.g. a unique participant and experiment ID).

Anytime during a screen, you can directly save a set of measurements to be added as a row to the **trialData**.
**(TODO: give an example code call for adding sth. like response and RT, if possible)**
To make collecting measurements across slides easier, you can also gradually accumulate data on a per-screen basis, namely by using the **`measurements`** object (think: text that the participant entered, a choice between different items, or the response time taken to accomplish the task). 
Once the trial is completed you can simply save all measurements in one go.

**TODO: clarify WHY using the measurements option is useful; maybe give a concrete example?**

Screens also allow specifying **validations** of the measured observations, for example to make sure entered text has a certain character length. (**TODO: if true, add information that a validation can be used to give feedback, e.g., 'correct' answer during training trials?**)

## Implementation

A _magpie experiment is implemented using Vue.js.
Essentially, _magpie provides ready-made screens, slides and other useful reusables to implement an experiment using Vue.js.
If you are not familiar with Vue.js, the following section serves as a basic introduction. (**TODO: insert link**) 
If you are fammiliar with Vue.js, you can skip ahead to [the example section]() (**TODO: fix link**).

