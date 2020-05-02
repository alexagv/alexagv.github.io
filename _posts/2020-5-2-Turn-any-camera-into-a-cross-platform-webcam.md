---
layout: post
title: Turn any camera into a cross platform webcam
---
How to get access most cameras "live view" and turn it into a webcam by utilizing gPhoto2 and Gstreamer.

## Prerequisites
Install gPhoto2 and Gstreamer follow the download links and instructions here: [https://gstreamer.freedesktop.org/download/](https://gstreamer.freedesktop.org/download/) and here [http://www.gphoto.org/](http://www.gphoto.org/)

## Universal method (via OBS Studio)
In our universal implementation we are going to use OBS and it`s virtual webcam function, if you are a MacOS user you have to download this fork of OBS Studio: https://github.com/johnboiles/obs-mac-virtualcam

First lets install the Gstreamer plugin for OBS: [https://github.com/fzwoch/obs-gstreamer](https://github.com/fzwoch/obs-gstreamer)

Then connect your DSLR via the USB cable and turn the camera on.
What we are going to do next is to tell gphoto2 to activate the live preview stream on the camera and pipe the output to stdout, then we are going to use Gstreamer to catch the frames from stdout and parse it to a local UDP stream.
Then in OBS we are going to use a Gstreamer source that are going to tap into that local UDP stream and render it in OBS with almost zero delay!

In OBS press the "+" Add button under Sources, and select Gstreamer Source. Replace the example code with the following:
```bash
udpsrc port=5000 caps="application/x-rtp, media=(string)video, clock-rate=(int)90000, payload=(int)96, encoding-name=(string)JPEG" ! rtpjpegdepay ! decodebin ! videoconvert ! video.
```

Then open your terminal and run this command:
```bash
gphoto2 --capture-movie --stdout | gst-launch-1.0 fdsrc fd=0 ! jpegparse ! rtpjpegpay pt=96 ! udpsink host=127.0.0.1 port=5000 sync=false
```

Now in OBS Studio go to "Tools" and press "Start virtual camera". Now start Skype, Discord, Zoom etc and you should be able to select your OBS Virtual Camera.

MacOS has had a quite a few issues with Zoom, you can fix them by retoggling Zoom permission as described [here.](https://www.reddit.com/r/VIDEOENGINEERING/comments/fy7xi3/fyi_zoom_v4610_breaks_blackmagic_capture_devices/?utm_source=share&utm_medium=web2x)

## Linux without OBS Studio
On Linux it`s even more simple, as Gstreamer usually is already installed you only need gPhoto2, and we can use the Gstreamer v4l2sink element to make a virtual webcam.
In the terminal run the following pipeline if you want to use the camera in any application as a webcam:
```bash
gphoto2 --capture-movie --stdout | gst-launch-1.0 fdsrc fd=0 ! jpegparse ! videoconvert ! v4l2sink device=/dev/video5
```
