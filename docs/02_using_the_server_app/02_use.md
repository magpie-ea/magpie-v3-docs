# Use 

After installation you can visit the server app in a browser, with the username and password you previously specified. For Heroku deployment, you may do so by running `heroku open` in the command line or find the app URL in your Heroku account. For local deployment, the URL is by default http://localhost:4000.

There is an example demo of the server app available at [https://babe-demo.herokuapp.com](https://babe-demo.herokuapp.com).

## Managing experiments

The server app shows a list of experiments whose data may be stored in a database. It
shows the experiments ID, its name, its author, the number of submissions retrieved so far,
date information, as well as whether the experiment is currently active or not. If an
experiment is set to be active, it allows further submissions to be recorded in the
database.

<img src="../../images/server_app_screen.png" width="100%">

The server app allows you to retrieve the data for an experiment from the database. Simply
click on the button "Retrieve CSV" to download a CSV-file with the data collected so far.

To delete an experiment, click the "Delete" button. Always make sure that you have recovered
all necessary data from that experiment; otherwise your data collected so far might be
irrevocably lost.

You can also edit an experiment with the "Edit" button. You can change information about the
experiment on the edit-screen. You can also toggle whether the experiment is active or not. You
can set a maximum number of submissions after which the experiment automatically toggles its
activity status off. Any submission made by a participant to a non-active experiment is just
lost and will not be recorded. This is to protect your database from pollution or attacks, but
if used unwisely could also cause you loss of relevant data.

## Creating new experiments

Click on the "New" button under "Manage Experiments" to set up a new data base for your
experiment. The interface for creating a new experiment is very similar to editing an existing
experiment. Importantly, you need to give some required information about a new experiment
(name and author). If you want to use dynamic retrieval of experiment data ([documentation
pending](fill_me)), you must specify which fields should be available to be retrieved by your
API calls. This allows you to expose only the relevant fields, since the dynamic retrieval API
is not password protected by default.

After you have created your experiment, there are two pieces of information that are important for later use. When you [specify the deploy information](/03_deploying_experiments/01_configuration/#changing-deploy-information), you must supply the ID which the server app allocates to your experiment and you must specify the URL (possibly local) of the server app itself. You find these pieces of information when you click on "Edit" for your newly created experiment.

<img src="../../images/edit_exp_screenshot.png" width="80%">

You need to specify the ID in field `experimentID` and the server URL for submissions **without the ID** in the field `serverAppURL`. So, in the case above, you would initialize your experiment with the following information:

```javascript
 babeInit({
        ...
        deploy: {
            experimentID: "1",
            serverAppURL: "https://babe-demo.herokuapp.com/api/submit_experiment/",
            ...
        }
        ...
    });
```
