# vue-rating-stars
## A Highly Customizable, easy-to-use elegant stars rating component (similar to Google Play)

Feedback would be much appreciated, questions, suggestions, issues are more than welcome.

###### Demo

![alt text](https://i.imgur.com/pciVDo0.png "4.6 Rating Stars")

[![Edit Vue Template](https://codesandbox.io/static/img/play-codesandbox.svg)](https://codesandbox.io/s/9846q4oz4r)

# Usage
Install via NPM ```npm i vue-dynamic-star-rating```

Then require in your project:
```
var StarRating = require('vue-dynamic-star-rating');
```
or ES6 syntax:
```
import StarRating from 'vue-dynamic-star-rating'
```
Then you can register the component globally:
```
Vue.component('star-rating', StarRating);
```
Or in your Vue component:
```
components: {
  StarRating
}
```
You can then use the following selector anywhere in your project:
```
<star-rating></star-rating>
```

# Docs
```config: {...}``` is a configuration object that is to be binded to vue-star-rating, api properties are:

## Basics

| Property | Type  | Description | Default
| --- | ---  | --- | --- |
| **rating** | number  | A number between 0.0-5.0 that will determine the fullness of the 5-stars rating polygons | 1 |

## Customized Styling

| Property | Type  | Description | Default |
| --- | ---  | --- | --- |
| **fullStarColor** | string | Set the full or partially-full star color | ```#ed8a19``` |
| **emptyStarColor** | string | Set the empty or partially-empty star color | ```#737373``` |
| **starWidth** | number | Set the star width | 20 |
| **starHeight** | number | Set the star height | 20 |

## Implementation Example
Define your **config** options object in the component importing VueStarsRating e.g
