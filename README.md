# vue-dynamic-star-rating
## A Highly Customizable, easy-to-use elegant stars rating component (similar to Google Play)

![MIT License](https://badgen.net/badge/license/MIT/blue "MIT License")
[![view on npm](http://img.shields.io/npm/v/vue-dynamic-star-rating.svg?colorB=red)](https://www.npmjs.org/package/vue-dynamic-star-rating)

For a walkthrough blogpost about how I implemented this component you can head to my *[medium post](https://medium.com/@yonatandoron/star-rating-make-svg-great-again-d4ce4731347e)*

###### Demo

![4.8 star rating](https://github.com/JonathanDn/vue-stars-rating/blob/master/demo_indicator.png "4.8 star rating")

[![Edit Vue Template](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/morqm41968)

# Usage
Install via NPM ```npm i vue-dynamic-star-rating```

Then require in your project:
```js
var StarRating = require('vue-dynamic-star-rating');
```
or ES6 syntax:
```js
import StarRating from 'vue-dynamic-star-rating'
```
Then you can register the component globally:
```js
Vue.component('star-rating', StarRating);
```
Or in your Vue component:
```js
components: {
  StarRating
}
```
You can then use the following selector anywhere in your project:
* To get up and running quick the package now supports rendering just the selector with default values
```html
<star-rating></star-rating>
```

# Props

## Basics

| Property | Type  | Description | Default
| --- | ---  | --- | --- |
| **rating** | Number  | A number between 0.0-5.0 that will determine the fullness of the 5-stars rating polygons | 0 |
| **indicator** | Boolean | A property that deteremines weather a rating indicator would show to the right | false |

## Customized Styling

| Property | Type  | Description | Default |
| --- | ---  | --- | --- |
| **color.full** | string | Set the full or partially-full star color | ```#ed8a19``` |
| **color.empty** | string | Set the empty or partially-empty star color | ```#737373``` |
| **size** | number | Set the star width and height | 24 |

## Implementation Example
Define your **config** options object in the component importing StarRating e.g
```js
data() {
  return {
    rating: 4.7,
    color: {
      full: '#FFDE2B',
      empty: '#3c455c',
    },
  }
}
```
And bind it to the selector like so
```html
<star-rating :rating="rating" :color="color"></star-rating>
```

Feedback would be much appreciated, questions, suggestions, issues are more than welcome.

---
👨‍💻 Follow me on [Twitter](https://twitter.com/jodoron).
