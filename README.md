<h3 align="center">
    <img alt="Logo" src="https://user-images.githubusercontent.com/30767528/103286800-5a83fa00-49e1-11eb-8091-ef895c6f8241.png" width="400"/>
</h3>

<h3 align="center">
    Visual dom-selection library 
</h3>

<p align="center">
    <a href="https://choosealicense.com/licenses/mit/"><img
        alt="License MIT"
        src="https://img.shields.io/badge/licence-MIT-ae15cc.svg"></a>
    <img alt="No dependencies"
        src="https://img.shields.io/badge/dependencies-none-8115cc.svg">
    <a href="https://www.patreon.com/simonwep"><img
        alt="Support me"
        src="https://img.shields.io/badge/patreon-support-6a15cc.svg"></a>
    <a href="https://www.jsdelivr.com/package/npm/@simonwep/selection-js"><img
        alt="jsdelivr hits"
        src="https://img.shields.io/jsdelivr/npm/hm/@simonwep/selection-js"></a>
    <a href="https://github.com/Simonwep/selection/actions?query=workflow%3ACI"><img
        alt="Build Status"
        src="https://github.com/Simonwep/selection/workflows/CI/badge.svg"></a>
    <a href="https://www.npmjs.com/package/@simonwep/selection-js"><img
        alt="downloads per week"
        src="https://img.shields.io/badge/downloads-1k%2Fweek-brightgreen.svg"></a>
    <img alt="gzip size" src="https://img.badgesize.io/https://raw.githubusercontent.com/Simonwep/selection-js/master/dist/selection.min.js?compression=gzip">
    <img alt="brotli size" src="https://img.badgesize.io/https://raw.githubusercontent.com/Simonwep/selection-js/master/dist/selection.min.js?compression=brotli">
    <img alt="Current version"
        src="https://img.shields.io/github/tag/Simonwep/selection.svg?color=23AD62&label=version">
</p>

<p align="center">
    <a href="https://www.buymeacoffee.com/aVc3krbXQ" target="_blank">
        <img src="https://user-images.githubusercontent.com/30767528/63641973-9d301680-c6b7-11e9-9d29-2ad1da50fdce.png"></a>
    </a>
</p>

### Features
* Tiny (only ~4kb)
* Simple usage
* Highly optimized
* Zero dependencies
* Mobile / touch support
* Vertical and horizontal scroll support

### Install

Via npm:
```
$ npm install @simonwep/selection-js
```

Via yarn:
```
$ yarn add @simonwep/selection-js
```

Include via [jsdelivr.net](https://www.jsdelivr.com/)
```html
<script src="https://cdn.jsdelivr.net/npm/@simonwep/selection-js/dist/selection.min.js"></script>
```

Or using ES6 modules:
```js
import {SelectionArea} from "https://cdn.jsdelivr.net/npm/@simonwep/selection-js/dist/selection.min.mjs"
```

## Usage
```javascript
const selection = new SelectionArea({

    // Class for the selection-area-element
    class: 'selection-area',

    // document object - if you want to use it within an embed document (or iframe)
    frame: document,

    // px, how many pixels the point should move before starting the selection (combined distance).
    // Or specifiy the threshold for each axis by passing an object like {x: <number>, y: <number>}.
    startThreshold: 10,

    // Enable / disable touch support
    touch: true,

    // On which point an element should be selected.
    // Available modes are cover (cover the entire element), center (touch the center) or
    // the default mode is touch (just touching it).
    mode: 'touch',

    // Behaviour on single-click
    // Available modes are 'native' (element was mouse-event target) or 'touch' (element got touched)
    tapMode: 'native',

    // Enable single-click selection (Also disables range-selection via shift + ctrl)
    singleClick: true,

    // Query selectors from elements which can be selected
    selectables: [],

    // Query selectors for elements from where a selection can be start
    startareas: ['html'],

    // Query selectors for elements which will be used as boundaries for the selection
    boundaries: ['html'],

    // Query selector or dom node to set up container for selection-area-element
    container: 'body',

    // Scroll configuration
    scrolling: {

        // On scrollable areas the number on px per frame is devided by this amount.
        // Default is 10 to provide a enjoyable scroll experience.
        speedDivider: 10,

        // Browsers handle mouse-wheel events differently, this number will be used as 
        // numerator to calculate the mount of px while scrolling manually: manualScrollSpeed / scrollSpeedDivider
        manualSpeed: 750
    }
});

```

## Events
Use the `on(event, cb)` and `off(event, cb)` functions to bind / unbind event-listener.

> You may want to checkout the [source](https://gist.github.com/Simonwep/b0c25725a4715c1c5aab501d6a782a71) used in the demo-page, it's easier than reading through the manual.

| Event      | Description
| -------------- | ----------- | 
| `beforestart`  | The `mousedown` / `touchstart` got called inside a valid boundary. Return `false` to cancel selection immediatly.  |
| `start`        | User started the selection, the `startThreshold` got fulfilled. | 
| `move`         | The user is selecting elements / moving the mouse around. |
| `stop`         | The user stopped the selection, called on `mouseup` and `touchend` / `touchcancel` after a selection. |

Check out [types.ts](https://github.com/Simonwep/selection/blob/v2/src/types.ts) to see what kind of data is passed to each event.

> Example:

```js
selection.on('beforestart', evt => {
    
    // Use this event to decide whether a selection should take place or not.
    // For example if the user should be able to normally interact with input-elements you 
    // may want to prevent a selection if the user clicks such a element:
    // selection.on('beforestart', ({event}) => {
    //   return event.target.tagName !== 'INPUT'; // Returning false prevents a selection
    // });
    
    console.log('beforestart', evt);
}).on('start', evt => {

    // A selection got initiated, you could now clear the previous selection or
    // keep it if in case of multi-selection.
    console.log('start', evt);
}).on('move', evt => {

    // Here you can update elements based on their state.
    console.log('move', evt);
}).on('stop', evt => {

    // The last event can be used to call functions like keepSelection() in case the user wants
    // to select multiple elements.
    console.log('stop', evt);
});
```

> You can find event-related examples [here](EXAMPLES.md).

## Methods
* selection.on(event`:String`, cb`:Function`) _- Adds an event listener to the given corresponding event-name (see section Events), returns the current instance so it can be chained._
* selection.off(event`:String`, cb`:Function`) _- Removes an event listener from the given corresponding event-name (see section Events), returns the current instance so it can be chained._
* selection.disable() _- Disable the functionality to make selections._
* selection.enable() _- Enable the functionality to make selections._
* selection.destroy() _- Unbinds all events and removes the area-element._
* selection.cancel() _- Cancels the current selection process._

* selection.trigger(evt`:MouseEvent|TouchEvent`, silent`:boolean`) _- Manually triggers the start of a selection. `silent = true` means that no `beforestart` event will get fired (default)._
* selection.keepSelection() _- Will save the current selected elements and will append those to the next selection. Can be used to allow multiple selections._
* selection.clearSelection() _- Clear the previous selection (elements which were saved by `keepSelection()`)._
* selection.getSelection() _- Returns currently selected elements as an Array._
* selection.resolveSelectables() _- Need to be called if during a selection elements have been added / removed._
* selection.select(query`:[String]|String`) _- Manually appends elements to the selection, can be a / an array of queries / elements. Returns actual selected elements as array._
* selection.deselect(el`:HTMLElement`) _- Removes a particular element from the current selection._

