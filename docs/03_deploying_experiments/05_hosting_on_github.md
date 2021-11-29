# Hosting on GitHub

[GitHub](https://github.com) is a code hosting and collaboration platform which lends itself naturally to
publishing your experiment code as Open Source. In addition to hosting source code, GitHub also allows
hosting websites using its [Github Pages](https://pages.github.com/) feature.

Magpie's project template (the one you get, when running `magpie new`) already contains a working GitHub Workflow for deploying to GitHub Pages.
This means, when you push your project to GitHub, it will automatically build the experiment and commit the output
to the `gh-pages` branch for you. Once you enable GitHub Pages for your repository, GitHub will make the latest version of the `gh-pages`
branch available publicly at `https://<username>.github.io/<repo-name>/`. As you commit new changes to your main branch,
the workflow will keep your `gh-pages` branch up-to-date.

Note: For this to work, the project name in your `package.json` file needs to match your repository name on GitHub.
By default, the name in your `package.json` is the same you entered when running `magpie new`.
