## Recruitment via Prolific

The following assumes that you are familiar with how to
operate experiments with [Prolific](https://www.prolific.ac). 


Go to the Prolific website and create a new experiment. Make a note of the **completion URL** that is provided by Prolific.

<img src="https://raw.githubusercontent.com/magpie-ea/ProlificDeployTemplate/master/images/readme/prolific_url.png" width="100%" >

Enter the completion URL in the field `completionUrl` when creating your experiment, as follows:

```javascript
export default {
    // Either 'debug', 'directLink' or 'prolific'
    mode: 'Prolific',

    experimentId: 'INSERT_A_NUMBER',
    serverUrl: 'https://magpie-demo.herokuapp.com/',
    socketUrl: 'wss://magpie-demo.herokuapp.com/socket',
    contactEmail: 'test@example.com',

    // this will be used in prolific mode
    completionUrl: 'https://app.prolific.ac/submissions/complete?cc=SAMPLE1234',
};
```

Use deploy method `Prolific` will take participants back to the Prolific website where they are supplied with their completion code.

The data from your experiment will _not_ be stored by Prolific, but recorded by the magpie server app.
Before launching the study on Prolific, double-check that the database on the back end is set up and the necessary information (`experimentId`, server URL) are set.
Also do not forget to update a web-site version of your experiment so that it includes the correct information in `completionUrl` before running the study.
