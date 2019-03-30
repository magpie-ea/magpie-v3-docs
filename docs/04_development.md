[TOC]

To get the development version of the \_babe package, clone this repository and install the dependencies by running `npm install` in the terminal.

### Workflow

branches:

- master - Current stable version.
- development - Development version. This is where new featues or bug fixes are pushed. When the version is stable, the branch is merged into master.

#### (1) Source files

- src/
    - `babe-canvas.js`
    - `babe-errors.js`
    - `babe-init.js`
    - `babe-progress-bar.js`
    - `babe-submit.js`
    - `babe-utils.js`
    - `babe-views.js`

- `babe.css`

#### (2) Create babe.js and babe.full.js

##### Option 1: Build the \_babe package files while developing

Use `npm run watch` command from the `babe-project` folder to start a process which watches for changes in the files in `src` and builds (updates) `babe.js` and `babe.full.js`. This commands builds both `babe.js` and `babe.full.js` when a file  in `src` is saved.

##### Option 2: Make changes to the files and then build the \_babe files

Run `npm run concat` from the `babe-project` folder. This command builds both `babe.js` and `babe.full.js`.

#### (3) Merge into master
- include a changelog information in the README
- merge to master
- [update the version of \_babe](https://docs.npmjs.com/about-semantic-versioning) in `package.json`

#### (4) Publish to npm

Run `npm publish` from the `babe-project` folder to publish the new version of \_babe.
