# Development 

To get the development version of the magpie package, clone this repository and install the dependencies by running `npm install` in the terminal.

### Workflow

branches:

- master - Current stable version.
- development - Development version. This is where new features or bug fixes are pushed. When the version is stable, the branch is merged into master.

#### (1) Source files

- `lib` contains the Command Line interface as well as the scaffolding files for new magpie projects
- `src/` contains the main source code
    - `docs.md` Each component folder contains a docs.md which will be prepended to the reference for that section
    - `API.md` This file is created automatically from the JavaScript source files
- `styleguidist/` contains the initialization code for the reference documentation
- `styleguidist.config.js` contains the configuration for the reference documentation

#### (2) Build the docs

##### Option 1: Build the reference documentation while developing

Use `npm run docs` command from the `magpie-base` folder to start a process which watches for changes in the files in `src` and makes a fully built version of the docs available on your local machine.

##### Option 2: Make changes to the files and then build the reference documentation

Run `npm run docs:build` from the `magpie-base` folder. This command outputs the reference docs in the styleguide folder.

#### (3) Develop experiments with your local version of magpie-base

In order to install your local version of magpie-base in a magpie project without publishing it first...

1. Run `npm link` in the folder `magpie-base`
2. Go to your project folder
3. Run `npm install`
4. Run `npm link magpie-base` to install the version of magpie-base that you just ran `npm link` in.

#### (4) Merge into master
- include a changelog information in the README
- merge to master
- [update the version of magpie](https://docs.npmjs.com/about-semantic-versioning) in `package.json`

#### (5) Publish to npm

Run `npm publish` from the `magpie-base` folder to publish the new version of magpie.

No build step is required before publishing.
