# Hosting on GitHub

[GitHub](https://github.com) is a code hosting and collaboration platform which lends itself naturally to
publishing your experiment code as Open Source. In addition to hosting source code, GitHub also allows
hosting websites using its [GitHub Pages](https://pages.github.com/) feature.

Magpie's project template (the one you get, when running `magpie new`) already contains a working GitHub Workflow for deploying to GitHub Pages.
This means, when you push your project to GitHub, it will automatically build the experiment and commit the output
to the `gh-pages` branch for you. Once you enable GitHub Pages for your repository, GitHub will make the latest version of the `gh-pages`
branch available publicly at `https://<username>.github.io/<repo-name>/`. As you commit new changes to your main branch,
the workflow will keep your `gh-pages` branch up-to-date.

## Multi-experiment repositories

*(available for experiments created with magpie-base v3.5.0 or newer)*

If you'd like to host multiple experiments in the same repository, you can put those experiments in separate sub-folders
and list those subfolders [in the matrix of the Github Workflow](https://github.com/magpie-ea/magpie-base/blob/develop/lib/templates/minimal/.github/workflows/deploy-to-gh-pages.yml#L15):

```yml
name: Build and Deploy
on:
  push:
    branches:
      - main
      - master

jobs:
  build-and-deploy:

    strategy:
      matrix:
        folder:
          - ./
# ...
```

By default the Github Workflow will only publish the experiment in the root folder of the repository: `./`

If you have your experiments in different folders you can simply change the list here:

```yml
    strategy:
      matrix:
        folder:
          - experiments/pilot-01/
          - experiments/main/
# ...
```

The experiments will then be hosted at

https://your-organisation.github.io/your-repository/experiments/pilot-01/

and 

https://your-organisation.github.io/your-repository/experiments/main/
