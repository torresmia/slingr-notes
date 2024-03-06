# UI services - scanners

An UI services allows you to inject HTML, JavaScript code to the 
client side.

## Camera scanner

Its entry point is the `service.scan` method, that in ACT is called
when a button in the header menu is clicked, executing the action
`Scan sample` of the entity `Samples`.

Simply a Global entity action that calls the UI service using the
method `sys.ui.sendMessage`.

This is using a library native to the browser called 
**Barcode Detection API** and also a Polyfill for devices that do not
support this library.

The scan function initiates the scanner:

- Defines how to use the Barcode Detection
- Initializes a `BarcodeDetector` object

Then creates a modal to open the camera:

- Necessary HTML code to create the modal where the camera will be
shown

Actually open the camera:

- Using the asynchronous method `openCamera` which returns a Promise
- First it'll open the device's camera
- It'll go frame by frame trying to detect a code that can be
scanned. It's recursive.

Try to detect in each frame:

- The method `detect` from the Barcode Detection API works with
images, not videos (which is why it's called frame by frame 
recursively). 
- If `detect` finds a code, call `service.closeScan`.

Get the code from `openCamera` and verify if it's a code that's 
meaningful for the app:

- Uses regex
- If a match is found, launches a custom event with 
`service.sendEvent`

## Regular scanner

It's basically a listener waiting for events coming from the DOM.
It listens when you press a key on your keyboard in a very short
time interval (to not get confused by normal typing).

During that time interval, it'll save the alphanumeric characters
(plus `-`and `/`) in a buffer that'll be emptied when the interval
of 300ms finishes.

Before being emptied, it's sent to a function `validateCode`:

- Compares using regex to know if it a meaningful code for the app
- Sends custom event if valid code

## UI Service Listeners

This are the listeners that'll catch the custom events thrown when 
a code was detected.


