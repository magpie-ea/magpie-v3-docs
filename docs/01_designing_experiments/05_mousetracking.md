# Canvas

Each magpie trial type view can make use of magpie's mouse tracking functionality. This can happen in two ways:

1. The trial type view integrates mousetracking on its own
2. Mouse tracking is manually added to a trial type view

## How to add mouse tracking to a trial

To record all mouse movement after the stimulus has been presented, regardless of which kind of view, simply add the `autostart` option to your view configuration.

For example:

```js
const main_block = magpieViews.view_generator("forced_choice", {
  trials: main_trials.length,
  name: 'main_trials',
  data: main_trials,
  mousetracking: {
    autostart: true,
  }
});
```

When implementing custom views, you can start mouse tracking by calling `config.data[CT].mousetracking.start()`.

### mousetracking.start([origin: {x: int, y: int}])
The start function optionally takes an origin argument that defines the coordinate origin relative to which mouse coordinates will be recorded.

## Result fields
Whenever mouse tracking is enabled, you will find four new properties in your result data:

    * `mousetrackingTime: int[]` - The time coordinates of the mouse path in miliseconds since the start
    * `mousetrackingX: int[]` - The x coordinates of the mouse path
    * `mousetrackingY: int[]` - The y coordinates of the mouse path
    * `mousetrackingDuration: int` - The total time the mouse was tracked for
    * `mousetrackingStartTime: int` - The start time of mouse tracking in miliseconds since the UNIX epoch (1970-01-01)
