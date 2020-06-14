---
layout: post
title: Super compress videos
---
How to effectivly compress any video with minimal quality loss using FFmpeg

## Introduction
Imagine getting a 300MB video from the marketing department and they want to publish on the company website as a background element. 
However, you refuse to publish any video bigger than 6MB to prevent the video for slowing down page reload times. 
What do you do? Compressing 300MB to 6MB without making the video looked like it was filmed with a potato might sound hard, but it isnt!

## The solution
By using FFmpeg you can actually do it right away with this simple one-liner:
```
ffmpeg -i input_video.mov -c:v libx264 -preset veryslow -tune film  -crf 30 -pix_fmt yuv420p -c:a copy output_compressed.mp4
```

If you are not happy with the result tune the `crf` number until you are satisfied (higher number equals smaller file size)

## Explanation of the parameters
You can read in more detail about the different encoder options [here](https://trac.ffmpeg.org/wiki/Encode/H.264), but below is a quick explanation: 

`input_video.mov` is the filename you want to compress, it can be a .mp4 .avi etc doesnt matter as long as FFmpeg is able to read it

`libx264` is the encoder we are using, x264 is an open source software based h264 encoder so it migth run a bit slow depending on your CPU but it has a ton of features and is perfect for this scenario

`-preset veryslow` is a paramater for the x264 encoder. You can read more about it here, but in short it will tell the encoder to prioritize quality over computation time

`-tune film` tells the encoder that the video is a film and not a cartoon. If your video is from an animation with reduced nr of colors, straight lines and one colored backgrounds you might want to change this parameter

`-crf 30` tells the encoder to keep the same level of visual quality over the entire video instead of keeping a constant bitrate. The scale of crf goes from 0 (lossless) to 51 (worst quality) If your video consists of several shots and a constant bitrate is set some of the shots with a lot of details might look quite blocky and bad compared to some easier scenes with reduced motion/details like a potrait interview

`yuv420p` just makes the video playable in quicktime player

`-c:a copy` copies the exisiting soundtrack without any transcoding. In my case the video had a muted soundtrack, but if you dont want sound you can skip this. However, I`ve seen some video players refusing to play a video without sound so the safest bet is to have an empty soundtrack on your video instead of none.

`output_compressed.mp4` is your output video
