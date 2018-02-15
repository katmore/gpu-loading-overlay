# GPU Loading Overlay
A pure css/gpu "loading spinner" overlay bundled into a javascript module to conveniently control state.

## Installation
Install using Bower or a CDN as described below.

### Install using Bower
#### Bower Step 1: execute bower shell command:
```Shell
$ bower install gpu-loading-overlay --save
```
#### Bower Step 2: include script tag pointing your project's bower components path
```html
<!-- change './bower_components' to point to your project's bower components path as appropriate-->
<script src="./bower_components/gpu-loading-overlay/src/loadingOverlay.js"></script>
```

### Install using CDN
**Include script tag from rawgit CDN:**
```html
<!-- thanks to Ryan Grove for operating https://rawgit.com-->
<script src="https://cdn.rawgit.com/katmore/gpu-loading-overlay/master/src/loadingOverlay.js"></script>
```
(See the [rawgit CDN FAQ](https://github.com/rgrove/rawgit/wiki/Frequently-Asked-Questions) for more information)

## Usage
### Methods
  * {string} loadingOverlay.**activate**() 
  
     Active the spinner overlay.
      
     **Returns:**

      * {string} spin handle
      

  * {void} loadingOverlay.**cancel**({string} spinHandle)
  
       Cancel the specified spinner.
     
     **Parameters:**

      * {string} **spinHandle**
      
        *Required* spin handle
        
  * {void} loadingOverlay.**cancelAll**({string} spinHandle)
  
       Cancels all active spinners.
     

### Examples

#### Example 1) Simple usage:

```javascript
var spinHandle = loadingOverlay.activate();
//
// do something that takes 5 seconds...
//
setTimeout(function() {
   loadingOverlay.cancel(spinHandle);
},5000);
```

#### Example 2) Why we use 'handles':

**loadingOverlay** provides a 'spin handle' each time the loading overlay is activated so that asyncronous processes
will not 'clobber' a subsequently started loading overlay.

Meaning, if the script fires off multiple processes subsequently with corresponding
*loadingOverlay* "spin handles", any of the processes can go ahead and "cancel" the overlay without worrying that it
will prematurely turn off the spinner meant for another process.

In the example below, there are two processes. The first process takes longer than the second process, yet the second
process can run the "cancel" method without concern of the first process's state.

```javascript
//
// do something that takes 5 seconds...
//
var spinHandle_firstProcess = loadingOverlay.activate();
setTimeout(function() {
   loadingOverlay.cancel(spinHandle_firstProcess);
},5000);

//
// do something that takes 2 seconds...
//
var spinHandle_secondProcess = loadingOverlay().activate();
setTimeout(function() {
   loadingOverlay.cancel(spinHandle_secondProcess);
},2000);
```

#### Example 3) Cancel all handles:

There may come a time when all active loading spinners should be cancelled regardless of if they have been individually cancelled.

An example use-case would be upon an unhandled exception:

```javascript
//
// prepare an event listener that will "cancel all spinners" upon encountering any unhandled javascript error
//
window.addEventListener('error', function (evt) {
   loadingOverlay.cancelAll();
});

//
// activate a spinner
//
var spinHandle = loadingOverlay.activate();

//
// do something that takes 5 seconds...
//
setTimeout(function() {
   loadingOverlay.cancel(spinHandle_secondProcess);
},5000);

//
// a wild error appears
//
throw new Error('Some error.');
```

### Screenshots
![Demo Screenshot](https://raw.githubusercontent.com/katmore/gpu-loading-overlay/master/demo-screenshot.jpg)
## Legal
loadingOverlay is distributed under the terms of the MIT license or the GPLv3 license.

Copyright (c) 2006-2018 Doug Bird.
All rights reserved.
