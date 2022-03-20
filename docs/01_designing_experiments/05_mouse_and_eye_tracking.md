# Mouse and eye tracking

## Mouse tracking
Magpie can track mouse movements during trials.

You can start collecting mouse tracking data by calling
[$magpie.mousetracking.start()](https://magpie-reference.netlify.app/#Mousetracking+start).

After the trial you can retrieve the tracking data using [$magpie.mousetracking.getMouseTrack()](https://magpie-reference.netlify.app/#Mousetracking+getMouseTrack).

For convenience, you can also use [MousetrackingStart](https://magpie-reference.netlify.app/#mousetrackingstart) in your
custom screens to start a new mouse track once it is rendered. The component will be invisible to participants.

## Eye tracking
Magpie supports [WebGazer](https://webgazer.cs.brown.edu), a library that can track participants' eye movements
via their webcam. Due to licensing issues we cannot bundle WebGazer with magpie, so you need to install it first using
the following command in your terminal. (Note that WebGazer is licensed under the GNU Public License.)

```shell
npm install webgazer
```

Before you can record eye tracking data, WebGazer needs to be calibrated using
[EyetrackingCalibrationScreen](https://magpie-reference.netlify.app/#eyetrackingcalibrationscreen). Afterwards, you can optionally
test calibration using [EyetrackingValidationScreen](https://magpie-reference.netlify.app/#eyetrackingvlidationscreen).

In a trial screen you can start collecting eyetracking data by calling [$magpie.eyetracking.start()](https://magpie-reference.netlify.app/#Eyetracking+start).

After the trial you can retrieve the tracking data using [$magpie.eyetracking.getEyeTrack()](https://magpie-reference.netlify.app/#Eyetracking+getEyeTrack).

For convenience, you may also use [EyetrackingStart](https://magpie-reference.netlify.app/#eyetrackingstart) in your
trial screens to start a new mouse track once the component is rendered (it is invisible).

## Fullscreen
For mouse and eye tracking it can be beneficial for the experiment to take up as much screen estate as possible.
To this end you can use [FullscreenStart](https://magpie-reference.netlify.app/#fullscreenstart) to automatically trigger
fullscreen mode. Additionally, you may want to use the full screen width in your experiment, instead of the default box.
This can be changed by setting the `wide` prop of the [Experiment](https://magpie-reference.netlify.app/#experiment) component.
