## Recruitment via Prolific

The following assumes that you are familiar with how to
operate experiments with [Prolific](https://www.prolific.ac). 


Go to the Prolific website and create a new experiment. Make a note of the **completion URL** that is provided by Prolific.

<img src="https://raw.githubusercontent.com/babe-project/ProlificDeployTemplate/master/images/readme/prolific_url.png" width="100%" >

Enter the completion URL in the field `prolificURL` when creating your experiment, as follows:

```javascript
 babeInit({
        ...
        deploy: {
            deployMethod: "Prolific",
            prolificURL: "https://app.prolific.ac/submissions/complete?cc=SAMPLE1234"
        }
        ...
    });
```

Use deploy method `Prolific` has two consequences. For one, it will insert a text input field in the introduction view of your experiment. For another, it will supply a `confirm` button at the end of the experiment, which takes participants to the Prolific website where they are supplied with their completion code.

The data from your experiment will _not_ be stored by Prolific, but recorded by the _babe server app. Before launching the study on Prolific, double-check that the database on the back end is set up and the necessary information (`experimentID`, server URL) are set. Also do not forget to update a web-site version of your experiment so that it includes the correct information in `prolificURL` before running the study.
