### Magpie is a framework for building psychological online experiments that run in the participants' browser.

<video src="/images/getting_started/intro.webm" style="width: 100%" controls></video>

Magpie is composed of these parts:

 1. [**a server-side part**](https://github.com/magpie-ea/magpie-backend) that allows managing and configuring experiments and stores experiment results; and
 2. [**a client-side part**](https://github.com/magpie-ea/magpie-base) based on [Vue.js](https://vuejs.org/) that runs in the participant's browser and provides:
     * a technical and conceptual framework for building experiments; and
     * a catalogue of components that can be used in experiments.
   
You can use the same server instance for running multiple experiments.

Vue.js enables building a web application out of small self-contained components, that you can re-use and share with other people.

## Features
Some features of magpie include:

 * *Declarative*: Define your experiments in an easily extendable, HTML-based language (the Vue.js templating language)
 * *Easy to use*: There are many built-in screens to use for quickly building simple and advanced experiments
 * *Composable*: Build custom behavior by picking from and combining a wide range of components, built-in and external (any Vue-compatible library is supported)
 * *Interactive*: Allow participants to interact with each other with support for interactive experiments built in

## This site
This site serves the manual, which will guide you through the various features.

There is also [the magpie reference](https://reference.magpie-experiments.org/), a comprehensive documentation
of magpie's components and their interfaces. It includes code examples that can be edited live to try out the components.
