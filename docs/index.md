# _magpie
_magpie is a framework for building online experiments (e.g., for psychological research) that run in the participants' browser.

It is composed of these parts:

 1. **a server-side part** that allows managing and configuring experiments and stores experiment results; and
 2. **a client-side part** based on [Vue.js](https://vuejs.org/) that runs in the participant's browser and provides:
     * a technical and conceptual framework for building experiments; and
     * a catalogue of components that can be used in experiments.
   
You can use the same server instance for running multiple experiments.

Vue.js enables building a web application out of small self-contained components, that you can re-use and share with other people.

## Features
Some features of _magpie include:

 * *Declarativeness*: Define your experiments in an easily extendable, HTML-based language (the Vue templating language)
 * *Ease of use*: There are many built-in screens to use for quickly building simple and advanced experiments
 * *Composability*: Build custom behavior by picking from and combining a wide range of components, built-in and external (any Vue-compatible library is supported)
 * *Interactivity*: Allow participants to interact with each other with support for interactive experiments built in

This site serves the manual, which will guide you through the various features.

There is also [the _magpie reference](https://magpie-reference.netlify.app/), a comprehensive documentation
of _magpie's components and their interfaces.
