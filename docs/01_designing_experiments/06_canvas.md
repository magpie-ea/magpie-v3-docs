# Canvas
Apart from using static files as stimuli, like image, audio or video files,
you can also dynamically draw visual stimuli using the [Canvas](http://magpie-reference.netlify.app/#canvas) components.

## CanvasStage and CanvasLayer
In order to draw anything you first need a canvas stage, which lays out the canvas on the web page.

Using the `config` prop, you can specify the dimensions of the new canvas:
```html
<CanvasStage :config="{width: 700, height: 500}">
```

Inside a stage, the shapes you draw are organized in layers that come one after the other, similar
to photo editing software. So, the children in your CanvasStage are going to be `CanvasLayer` components.

### Drawing shapes
Inside your layers, you can use the following built-in shapes to draw on your canvas. `x` and `y` are either the top left
coordinates of the shape (in case of rectangular shapes), or the center coordinates of the shape (in case of radial shapes).

All canvas components are reactive, meaning, whenever you change the config values, the drawing will update in real-time.

#### CanvasCircle
```html
<CanvasCircle :config="{
        x: 100,
        y: 100,
        radius: 70,
        fill: 'red',
        stroke: 'black',
        strokeWidth: 2 }" />
```

#### CanvasRect
```html
<CanvasRect :config="{
    x: 230,
    y: 30,
    width: 140,
    height: 140,
    fill: 'green',
    stroke: 'black',
    strokeWidth: 2 }"/>
```

#### CanvasStar
```html
<CanvasStar :config="{
        x: 500,
        y: 100,
        numPoints: 7,
        outerRadius: 70,
        innerRadius: 40,
        fill: 'yellow',
        stroke: 'black',
        strokeWidth: 2 }"/>
```

### CanvasLine
* `points`: Define the points of your line as an array of consecutive x,y pairs, i.e. `[x1, y1, x2, y2, x3, y3]`.
* `lineJoin`: miter, round, or bevel
* `lineCap`: butt, round, or square
* `tension`: Value between 0 and 1, where 0 means zig-zag and 1 means completely curved 

```html
<CanvasLine :config="{
        points: [5, 70, 140, 23, 250, 60, 300, 20],
        stroke: 'red',
        strokeWidth: 15,
        lineCap: 'round',
        lineJoin: 'round'}" />
```

### Canvas events
You can add all normal event listeners to all shapes, as follows:

```html
<CanvasStar :config="{
        x: 500,
        y: 100,
        numPoints: 7,
        outerRadius: 70,
        innerRadius: 40,
        fill: 'yellow',
        stroke: 'black',
        strokeWidth: 2 }"
        @click="onStarClicked" />
```
