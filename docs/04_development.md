# Development 

To get the development version of the magpie package, clone this repository and install the dependencies by running `npm install` in the terminal.

### Workflow

branches:

- master - Current stable version.
- development - Development version. This is where new featues or bug fixes are pushed. When the version is stable, the branch is merged into master.

#### (1) Source files

- src/
    - `magpie-canvas.js`
    - `magpie-errors.js`
    - `magpie-init.js`
    - `magpie-progress-bar.js`
    - `magpie-submit.js`
    - `magpie-utils.js`
    - `magpie-views.js`

- `magpie.css`

#### (2) Create magpie.js and magpie.full.js

##### Option 1: Build the magpie package files while developing

Use `npm run watch` command from the `magpie-ea` folder to start a process which watches for changes in the files in `src` and builds (updates) `magpie.js` and `magpie.full.js`. This commands builds both `magpie.js` and `magpie.full.js` when a file  in `src` is saved.

##### Option 2: Make changes to the files and then build the magpie files

Run `npm run concat` from the `magpie-ea` folder. This command builds both `magpie.js` and `magpie.full.js`.

#### (3) Merge into master
- include a changelog information in the README
- merge to master
- [update the version of magpie](https://docs.npmjs.com/about-semantic-versioning) in `package.json`

#### (4) Publish to npm

Run `npm publish` from the `magpie-ea` folder to publish the new version of magpie.
