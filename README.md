# React Bubble Chart

A combination React/D3 bubble chart that animates color/size/position when
updating data sets.

![gif](http://i.imgur.com/OQEdgOW.gif)

## Installation

```sh
npm install react-bubble-chart
```

## Use

NOTE: you must include the css rules provided in `src/styles.css` (also
included is a LESS file if that's your cup of tea) somewhere on your page for
this component to work properly.

ANOTHER NOTE: this component does not load in React directly (as to not
conflict with whatever version of React you're already using). As a result of
this, it will not work if you don't already have react installed in your
project.

```js
import React            from 'react';
import ReactBubbleChart from 'react-bubble-chart';
import Actions          from '../Actions';

var colorLegend = [
  //reds from dark to light
  {color: "#67000d", text: 'Negative'}, "#a50f15", "#cb181d", "#ef3b2c", "#fb6a4a", "#fc9272", "#fcbba1", "#fee0d2",
  //neutral grey
  {color: "#f0f0f0", text: 'Neutral'},
  // blues from light to dark
  "#deebf7", "#c6dbef", "#9ecae1", "#6baed6", "#4292c6", "#2171b5", "#08519c", {color: "#08306b", text: 'Positive'}
];

class BubbleChart extends React.Component {
  render () {
    var data = this.props.data.map(d => ({
      _id: d._id,
      value: d.value,
      colorValue: d.sentiment,
      selected: d.selected
    }));

    return <ReactBubbleChart
      className="my-cool-chart"
      colorLegend={colorLegend}
      data={data}
      selectedColor="#737373"
      selectedTextColor="#d9d9d9"
      fixedDomain={{min: -1, max: 1}}
      onClick={Actions.doStuff.bind(Actions)}
    />;
  }
}

module.exports = BubbleChart;
```

To update the data, simply update the parent component that passes down the
`data` prop to the bubble chart.

## Breakdown of Props

ReactBubbleChart uses the following props:

### `data` (required)

An array of data objects (defined below) used to populate the bubble chart.

```js
{
   _id: string,        // unique id (required)
   value: number,      // used to determine relative size of bubbles (required)
   displayText: string,// will use _id if undefined
   colorValue: number, // used to determine color
   selected: boolean,  // if true will use selectedColor/selectedTextColor for circle/text
}
```

### `colorLegend` (required)

An array of strings (hex values) or objects that define a color and text. If
text is defined, this value will be shown in the legend.

```js
string || {
  color: string,
  text: string used in legend
}
```

### `fixedDomain` (optional)

Used in tandum with the color legend. If defined, the minimum number corresponds
to the minimum value in the color legend array, and the maximum corresponds to
the max. The rest of the `colorValue` values will use a quantized domain to find
their spot.

If this is undefined we will use the min and max of the `colorValue`s of the
dataset.

```js
{
  min: number,
  max: number
}
```

### `selectedColor` (optional)

String hex value.

If defined, will use this to color the circle corresponding to the data object
whose `selected` property is true.

### `selectedTextColor`

String hex value.

If defined, will use this to color the text corresponding to the data object
whose `selected` property is true.

### `onClick`

Can pass a function that will be called with the data object when that bubble is
clicked on.

## Implementation Inspiration (credit where credit is due).

### React+D3

Connecting React+D3 followed the three guidelines
[presented here](http://nicolashery.com/integrating-d3js-visualizations-in-a-react-app/).
In case you don't want to read that article, they are:

  1. **One Source Of Truth**: The D3 visualization should get all of the data it
  needs to render passed down to it.
  1. **Stateless All The Things**: This is related to (1). D3 and React
  components alike should be as stateless as possible, i.e. they shouldn't
  hide/encapsulate something that makes them render differently given the same
  "input".
  1. **Don't Make Too Many Assumptions**: This is related to (1) and (2).
  Components shouldn't make too many assumptions about how they will be used.

### The Bubble Chart

Some of the animation math was inspired from the animated bubble chart in
[this article](http://www.pubnub.com/blog/fun-with-d3js-data-visualization-eye-candy-with-streaming-json/)

### Getting the Text to Wrap in the Bubble Chart

[This article](http://vallandingham.me/building_a_bubble_cloud.html) presents
the idea of having the bubbles be in their own `<svg>` tag, and having the text
be animated as `<div>`s in their own `<html>` tag that is a sibling of the
`<svg>` tag. The reason for this being, that it's tricky to get text to wrap
with svgs, but not with html.
# Contributing

Feel free to fork this repo and open a Pull Request or open an issue!

# License

This app is licensed under the Apache 2.0 License. Full license text is
available in [LICENSE](https://github.com/kauffecup/react-bubble-chart/blob/master/LICENSE).

# Contact Me

All of my contact information can be found [here](http://www.jkaufman.io/about/)
