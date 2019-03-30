Each babe trial type view can use the babe canvas api to create a picture of elements in the trial view.

babe provides three types of element plcement:

1. **random placement**

<img src='../../images/canvas_samples/random.png' alt='random placement example' height='auto' width='600' />

2. **grid placement**

<img src='../../images/canvas_samples/grid.png' alt='grid placement example' height='auto' width='600' />

3. **split grid placement**

<img src='../../images/canvas_samples/split_grid.png' alt='split grid placement example' height='auto' width='600' />


## How to use babe canvas

To generate a picture of shapes, all you need is to have `canvas` object with some properties in the data your views use.

For example:

```
let trials = [
    ...,
    {
        question: "Are there any blue squares on the screen?",
        option1: 'yes',
        option2: 'no',
        canvas: {
            focalColor: 'black',
            focalShape: 'circle',
            focalNumber: 23,
            otherShape: 'square',
            otherColor: 'red',
            sort: 'random',
            elemSize: 30,
            total: 40
        }
    },
    ...
];
```
