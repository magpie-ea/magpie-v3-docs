# Hosting on GitHub

[GitHub](https://github.com) is a code hosting and collaboration platform which lends itself naturally to
publishing your experiment code as Open Source. In addition to hosting source code, GitHub also allows
hosting websites using its [GitHub Pages](https://pages.github.com/) feature.

Assuming that the experiment exists locally, but has not been added to
GitHub:

1. Create a repository on GitHub
2. Set read/write permissions for GitHub workflows
   - GitHub.com -> in the repo -> settings -> actions -> general ->
   workflow permissions -> check R/W persmissions -> save
   - otherwise the GH-actions scripts halts with an error
3. initialize the local repo with `git init`
4. Commit everything that’s relevant (including `.github` folder) and push to GitHub
5. Wait for the GitHub Action to finish (this will create a `gh-pages`
   branch)
6. On GitHub, go to settings -> pages and select to host GitHub Pages from
   the branch `gh-pages`
   - you must do this only *after* the Action has created
   the `gh-pages` branch, otherwise GitHub Pages keeps serving a page
   from your `main` branch (showing only your `README.md` file)
7. After a while your page will appear under URL
   `https://<username>.github.io/<repo-name>/`
   - this URL will show on GitHub under settings -> pages only after one
   more build (after one more commit to the repo, which will trigger a rerun
   of the GitHub Action), but the site should be there even without a
   second built

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
