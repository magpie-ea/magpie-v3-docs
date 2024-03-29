# Installation

## Pre-requisites
To create an experiment you will need [Node.js](https://nodejs.org) and npm (usually comes together with your Node.js installation).
Node.js is a runtime for JavaScript, a language that predominantly runs in the browser. Magpie is therefore also written in JavaScript. npm is a package manager for JavaScript and allows you to install projects and libraries on your computer.

Required are

* Node.js v18.x
* npm v8 or later

## Creating a project
To create a new project, go to a directory of your choice and run

```bash
npx magpie-base@latest new "project-name"
```

magpie will create a new directory for you with a ready-to-run sample project.

Now, in that newly created directory you will need to install the dependencies for your project using

```bash
npm install
```

This will install all kinds of small libraries that magpie needs to run.

## Running a development instance
To try out your project on your local machine, run

```bash
npm run serve
```

This runs a development instance of your project for testing.
You can click on the link it provides, to open the project in your browser.
Every time you make a change to your code, the browser view will automatically update to reflect your changes.


## Building your project
To create a final build of your project that you can upload to a hoster like netlify, run

```bash
npm run build
```

This will put all necessary files for publishing into a folder called `build`.
