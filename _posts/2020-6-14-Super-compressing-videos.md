---
layout: post
title: Super compressing videos
---
How to effectivly compress any video with minimal quality loss using FFmpeg

## Introduction
Imagine getting a 300MB video from the marketing department and its your job to publish it on the company website as a background element. 
However, you refuse to publish any video bigger than 6MB to prevent the video for slowing down page reload times. 


What do you do? Compressing 300MB to 6MB without making the video look like it was filmed with a potato might sound hard, but it isnt!

## The solution
By using [FFmpeg](https://ffmpeg.org/download.html) you can actually do it right away with this simple one-liner:
```
ffmpeg -i input_video.mov -c:v libx264 -preset veryslow -tune film  -crf 30 -pix_fmt yuv420p -c:a copy output_compressed.mp4
```

If you are not happy with the result tune the `crf` number until you are satisfied (higher number equals smaller file size)

## The result
Can you guess the filesize of the 4K video below?
<video muted autoplay controls>
    <source src="/images/output_compressed.mp4" type="video/mp4">
</video>
Its actually only 4.7MB! 

In this case I used 35 as CRF number. You can find the original file [here (88.6MB)](https://www.pexels.com/video/low-lying-clouds-covers-the-mountain-side-2888383/).

## Explanation of the parameters
You can read in more detail about the different encoder options [here](https://trac.ffmpeg.org/wiki/Encode/H.264).

`input_video.mov` is the filename you want to compress, it can be a MP4-file, AVI-file etc doesn't it doesnt matter as long as FFmpeg is able to read it.

`libx264` is the video encoder we are using. x264 is an open source software based h264 encoder so it migth run a bit slow depending on your CPU, but it is cross-platform so it will run no matter what OS you are using. Why h264 when we have more effective codecs such as AV1 or h265? In this case it is a matter of compability for the web. A search on [caniuse.com](https://caniuse.com/#search=h264) reveals that h264 is supported by most browsers while h265 or AV1 is not. 

`-preset veryslow` tells the encoder to prioritize quality over computation time.

`-tune film` tells the encoder that the video is a film and not a cartoon. If your video is an animation or a cartoon `animation` should be used and if the video is a slideshow `stillimage` should be used. 

`-crf 30` tells the encoder to keep the same level of visual quality over the entire video instead of keeping a constant bitrate. The scale goes from 0 (lossless) to 51 (worst quality) and is exponential so increasing it with 6 will basically half the size of your previous value.

`yuv420p` makes the video playable in QuickTime player.

`-c:a copy` copies the exisiting soundtrack without any transcoding. In my case the video had a muted soundtrack, but if you dont want sound you can skip this. However, I've seen some video players refusing to play a video without sound so the safest bet is to have an empty soundtrack on your video instead of none.

`output_compressed.mp4` is your output video.

