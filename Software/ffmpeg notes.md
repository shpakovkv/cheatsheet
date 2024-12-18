# How to use ffmpeg tool

## Download binary/source
https://www.videohelp.com/software/ffmpeg
https://ffmpeg.org/download.html


### Get video information
```bash
path/to/bin/ffmpeg.exe -i path/to/file
```

### Trim video without recompression
```bash
path/to/bin/ffmpeg.exe -ss 00:05:01 -to 00:06:01 -i path/to/file.mp4 -c copy path/to/output_name.mp4
```


### Extract frames as Jpeg
```bash
# Extract frames from video with 240 frames per second:
path/to/bin/ffmpeg.exe -i path/to/file.mp4 -filter:v fps=fps=240 path/to/Frames%03d.jpg
```

### Recompress as 720p (-crf 28 is very lossy) h264

-preset: determines compression efficiency and therefore affects encoding speed
-c:v:  means codec:video
-c:a: means codec:audio
-vf scale=-1:720:  video filter 'scale', '-1' means save the aspect ration of original
```bash
path/to/bin/ffmpeg.exe -i path/to/input_video.mp4 -vf scale=-1:720 -c:v libx264 -crf 20 -preset veryslow -c:a copy save/as/video.mp4

```

### Recompress as 720p (-crf 28 is very lossy) h265
```bash
ffmpeg -i input -c:v libx265 -crf 26 -preset fast -c:a aac -b:a 128k output.mp4
```
