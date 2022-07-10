node-dualshock-controller
=========================

This project is an updated fork of [node-dualshock-controller](https://github.com/rdepena/node-dualshock-controller) An eventing API layer over HID for the Sony DualShock 3 and DualShock 4 controllers.

# Installation:
Only a manual instalation of this package will work for right now.

Download the github repo with `git clone https://github.com/HaroldPetersInskipp/node-dualshock-controller.git`

Or from [HERE](https://github.com/HaroldPetersInskipp/node-dualshock-controller/archive/refs/heads/main.zip).

To use with Node-RED, place the downloaded files in your `.node-red/node_modules` directory (usually located in the users home folder, it may be hidden by default).

Extract the files.

Then in your prefered terminal, navigate to the directory where you extracted the files and type `npm i` or `npm install`

# Optional Instructions - Import flow.json into Node-Red
This flow is an example of the package being used to capture events from a Dualshock 4 controller for use in Node-RED.

It can be imported into the Node-Red editor using the `flow.json` file. The package must have been manually installed first.

In the Node-RED editor, select the `Import` menu option or press `ctrl-i` to bring up the import dialog.

The Import dialog can be used to import a flow by the following methods:

    pasting in the flow JSON directly,
    uploading a flow JSON file,
    browsing the local flow library,
    browsing the example flows provided by installed nodes.

After pasting the contents of `flow.json` click the `Import` button and click `Deploy` to deploy the flow.

This example may save you some time starting a project, but doesn't do much else yet.

## Using the DualShock library

`Important: THE CONTROLLER WILL NOT SEND ANY DATA IF YOU DO NOT PRESS THE PS BUTTON.`

Also, to use the touchpad, rumble and LED capabilities of the controller you
must connect the controller to your computer using a micro-USB cable.

~~~~ javascript
let dualShock = require('node-dualshock-controller');

// pass options to init the controller.
let controller = dualShock({
    //you can use a ds4 by uncommenting this line.
    config: "dualshock4-generic-driver", 
    //if the above configuration doesn't work for you,
    //try uncommenting the following line instead.
    //config: "dualshock4-alternate-driver",  
    //if using ds3 uncomment this line.
    //config: "dualShock3",  
    //smooths the output from the acelerometers (moving averages) defaults to true
    accelerometerSmoothing: true, 
    //smooths the output from the analog sticks (moving averages) defaults to false
    analogStickSmoothing: false,
});

// make sure you add an error event handler
controller.on('error', err => console.log(err));

// DualShock 4 control rumble and light settings for the controller
controller.setExtras({
  rumbleLeft:  0,   // 0-255 (Rumble left intensity)
  rumbleRight: 0,   // 0-255 (Rumble right intensity)
  red:         0,   // 0-255 (Red intensity)
  green:       75,  // 0-255 (Blue intensity)
  blue:        225, // 0-255 (Green intensity)
  flashOn:     40,  // 0-255 (Flash on time)
  flashOff:    10   // 0-255 (Flash off time)
});

// DualShock 3 control rumble and light settings for the controller
controller.setExtras({
  rumbleLeft:  0,   // 0-1 (Rumble left on/off)
  rumbleRight: 0,   // 0-255 (Rumble right intensity)
  led: 2 // 2 | 4 | 8 | 16 (Leds 1-4 on/off, bitmasked)
});

// add event handlers:
controller.on('left:move', data => console.log('left Moved: ' + data.x + ' | ' + data.y));
controller.on('right:move', data => console.log('right Moved: ' + data.x + ' | ' + data.y));
controller.on('connected', () => console.log('connected'));
controller.on('square:press', ()=> console.log('square press'));
controller.on('square:release', () => console.log('square release'));

// sixasis motion events:
// the object returned from each of the movement events is as follows:
//{
//    direction : values can be: 1 for right, forward and up. 2 for left, backwards and down.
//    value : values will be from 0 to 120 for directions right, forward and up and from 0 to -120 for left, backwards and down.
//}

// DualShock 4 TouchPad
// finger 1 is x1 finger 2 is x2
controller.on('touchpad:x1:active', () => console.log('touchpad one finger active'));
controller.on('touchpad:x2:active', () => console.log('touchpad two fingers active'));
controller.on('touchpad:x2:inactive', () => console.log('touchpad back to single finger'));
controller.on('touchpad:x1', data => console.log('touchpad x1:', data.x, data.y));
controller.on('touchpad:x2', data => console.log('touchpad x2:', data.x, data.y));

// right-left movement
controller.on('rightLeft:motion', data => console.log(data));
// forward-back movement
controller.on('forwardBackward:motion', data => console.log(data));
// up-down movement
controller.on('upDown:motion', data => console.log(data));

// controller status
// as of version 0.6.2 you can get the battery %, if the controller is connected and if the controller is charging
controller.on('battery:change', data => console.log(data));
controller.on('connection:change', data => console.log(data));
controller.on('charging:change', data => console.log(data));

~~~~


The MIT License (MIT)

Copyright (c) 2017 Ricardo de Pena

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
