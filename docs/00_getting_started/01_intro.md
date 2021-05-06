# Introduction

_magpie is a framework for building psychological online experiments that run in your browser.

It is composed of a server-side part that stores your experiment results and handles interactivity, and a client-side part that runs in the participant's browser. You can use the same server instance for running multiple experiments.

For now, we will take a look at the part that runs in the browser.

## Set-up
To create an experiment you will need [Node.js](https://nodejs.org) and npm (usually comes together with your Node.js installation).
Node.js is a runtime for JavaScript, a language that predominantly runs in the browser. _magpie is therefore also written in JavaScript. npm is a package manager for JavaScript and allows you to install projects and libraries on your computer.

To get started with _magpie, install magpie globally using npm, by running the following on the command line:

```bash
npm install -g magpie-base
```

This will install the `magpie` command on your system. You can now enter

```
magpie
```

in your command line to run the magpie command.

### Creating a project
To create a new project, go to a directory of your choice and run

```bash
magpie new "project-name"
```

_magpie will create a new directory for you with a ready-to-run sample project.

Now, you will need to install the dependencies for your project using

```bash
npm install
```

This will install all kinds of small libraries that _magpie needs to run.

### Running a development instance
To try out your project on your local machine, run

```bash
npm run serve
```

This runs a development instance of your project for testing.
You can click on the link it provides, to open the project in your browser.
Every time you make a change to your code, the browser view will automatically update to reflect your changes.


### Building your project
To create a final build of your project that you can upload to a hoster like netlify, run

```bash
npm run build
```

This will put all necessary files for publishing into a folder called `build`.
