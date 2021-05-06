# File structure
By default, a _magpie project has the following file structure:

```txt
# top-level configuration files
# these are usually not interesting
.eslintrc.js
.gitignore
.prettierrc.js
package.json
package-lock.json
vue.config.js

# Files for publishing (like media)
public/
  # your image files
  img/
  # your video files
  video/
  # your audio files
  audio/

# Your code goes here
src/
  # _magpie-specific configuration
  magpie.config.js
  # set-up code for Vue.js, usually you don't need to touch this
  main.js
  # The main file for your experiment.
  App.vue

# csv files and other data neded for your experiments go here
trials/

# This is where npm installs the dependencies of your project
# You usually don't need to look in here
node_modules/
```
