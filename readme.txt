/*
 
 Copyright (C) Adrian Buriks 2012
 
 Permission is hereby granted, free of charge, to any person obtaining a copy
 of this software and associated documentation files (the "Software"), to deal
 in the Software without restriction, including without limitation the rights
 to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
 copies of the Software, and to permit persons to whom the Software is
 furnished to do so, subject to the following conditions:
 
 The above copyright notice and this permission notice shall be included in
 all copies or substantial portions of the Software.
 
 THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
 IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
 FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
 AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
 LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
 OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
 THE SOFTWARE.
 
 Copyright (C) Adrian Buriks 2012
 
 */


Read Me About Cam-y-Sol (October 12, Release)
========================

This application implements a camera with flash and torch disabled that can
only take pictures during a local eclipse. The application accepts current
dates form 2012 - 2035, finds the next eclipse, and displays related information.

Location services are required for this application at present.

On devices with focus capabilities:
A single-tap will cause an auto focus at a specific point, then lock focus. 
A double-tap will change back to continuous auto focus.

Photos that are taken are stored in the standard iPhone photo library.

Here is a description of its behavior...

Behavior 15 minutes before the start of a partial eclipse at current location:
- The "Photo" button which was gray turns white.
- The counter down timer stops counting and says "Camera Enabled"

Behavior 15 minutes after the end of partial eclipse at the current location:
- The "Photo" button is again grayed.
- The count down timer says "Photo Session Complete"

Behavior 4 hours after the start of a partial eclipse at the current location:
All data is updated for the next eclipse at current location.

Additional notes...

- To save power, location is only updated when the app is closed and re-opened.
  The one exception is at the end of an eclipse, when all data is refreshed.
-If no GPS is available, the last known location continues to be used.


NEW FEATURE: EXIF DATA
Apple provides a means of adding this to images when saving to the Asset Library.
However, it is not well documented. Using the spec (in this directory) and online
resources, data has successfully been added using Apple's APIs.

In addition to data added automatically by the device, the following information
is added to JPEG image by this version of the code:

EXIF
Orientation: EXIF value describing orientation
DateTimeOriginal: UTC Date/Time

GPS
Latitude: degrees
Longitude: degrees
Altitude: 0
Latitude Ref: N,S
Longitude Ref: W,E
Attempts to add GPS DateStamp and TimeStamp fields have thus far been unsuccesful.

Remaining workÉ

1:
- White balance and exposure controls
  Approach to attempt = Move exposure rectangle slowly outside of image.
  For example, center = .5,.5 -> When is off screen.
  Can monitor the EXIF data (I believe).
  Values include:
      Exposure Program
      
- Auto photo during Eclipse
- Add the name of the app, etc.

2:
- Legal disclaimer added (protection from burning out device or eyes)
- Unit testing
- Memory leak scans etc.
- Regression tests

3:
- Put it up on the App store.

NOTES FROM APPLE ABOUT THE ORIGINAL DEMO APP (called AVCam)

AVCam runs on iOS 4.0 and later.

Building the Sample
-------------------
AVCam was built with Xcode 3.2.5 on Mac OS X 10.6.6 with the iOS 4.2 SDK. To build the app, simply open the project and choose Build from the Build menu. 
NOTE: This project CAN be built for the iOS simulator. However, no image will appear. Timing calculations and behavior can be tested.


How It Works
------------

Adding EXIF information:
The following function allows saving an image to the Asset library by providing
a library with metatdata. 
[library writeImageToSavedPhotosAlbum:[image CGImage]
                                           //Setting the orientation this way is another call, so we do it immediately before
                                           //orientation:(ALAssetOrientation)[image imageOrientation]
                                              metadata:metadataAsMutable
                                       completionBlock:completionBlock];

Obtaining Location:
Uses location services (My note)
Low level image capture: AV Foundation AVCapture classes.

Access to Low Level Camera functionality:
AVCam makes use of the following AV Foundation AVCapture classes to provide audio, video and still image capture and control:

AVCaptureConnection
AVCaptureDevice
AVCaptureFileOutput
AVCaptureInput
AVCaptureMovieFileOutput
AVCaptureOutput
AVCaptureSession
AVCaptureStillImageOutput
AVCaptureVideoPreviewLayer

See the AV Foundation documentation for more information.


