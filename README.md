# Cactas
###### *The spelling is intentional* 

This repo's primary purpose is to store my profile picture somewhere I can get quick and easy access to it. It's secondary purpose is to give a write-up on the steps I take to ensure the highest quality final result. Hopefully the latter is helpful or interesting to someone.



## The Write-Up
GIFs are tricky. They're an unfortunately low quality format that has grown to high popularity, which leaves problems to be solved when working with them. Luckily, it's 2022 and someone has already had all the struggles you can run into and created a tool to solve each of your problems to the best of their ability.


### 1. Getting the *original* version of your GIF
It's important to start with the highest quality copies of your initial assets as possible, garbage-in means garbage-out.

The ideal situation would be to have the original quality frames that were used to create the GIF. If you have the frames on hand or you know the original creator and can get them, that's the best spot you can be in.

For those of us who aren't that lucky, we can upload our GIFs to [TinEye](https://tineye.com) and [Yandex Images](https://yandex.com/images) to try and find the original version. I recommend giving the top handful of results a once-over to see if someone credited the original creator, a lot of the times putting the creator's name in "double quotes" on Google will get you to a portfolio where you can find the original.


### 2. Extracting the frames from a compiled GIF (or video)
There are several options you can use to rip your GIF into individual frames that we can work on. My personal recommendations are [Gifsicle](https://www.lcdf.org/gifsicle/) for GIFs and [FFmpeg](https://ffmpeg.org) for videos.

#### Using Gifsicle ([Download](https://www.lcdf.org/gifsicle/), [docs](https://www.lcdf.org/gifsicle/man.html))
```
gifsicle --unoptimize --explode already-compiled.gif --output frame
```
`--unoptimize` makes sure that "each frame is a faithful representation of what a user would see at that point in the animation". Some optimisation methods will only include data that applies to every frame once, this makes the file size smaller since it's not including redundant data, but it means that without "unoptimizing", most of your frames could be missing important data.

`--explode` tells Gifsicle to extract all the individual frames from the given GIF and put them in separate files. The first frame will be `filename.000`, the second will be `filename.001`, and so forth.

`--output` determines the output filename used for the individual frames.

#### Using FFmpeg ([Download](https://ffmpeg.org/download.html), [docs](https://www.ffmpeg.org/ffmpeg.html))

```
ffmpeg -i video.mp4 -fps_mode passthrough frame%03d.png
```
`-i` specifies the input file

`-fps_mode passthrough` "Each frame is passed with its timestamp from the demuxer to the muxer.", basically it means we won't lose any frames.

`frame%03d.png` is the output file name (and file type). %03d will be replaced 3-digit decimal (as in base 10) integer with trailing zeros (1 will be output as 001). This means your first frame's file name will look like `frame001.png`.

### 3. Editing your GIF
You can now edit the individual frames of your GIF. You can use your editing software of choice, mine is [paint.net](https://www.getpaint.net), you might prefer [GIMP](https://www.gimp.org), or if you own a license [Adobe Photoshop](https://www.adobe.com/products/photoshop.html) obviously works fine. The only caveat is that Gifsicle exports files in GIF formatting and FFmpeg will export them as PNGs like we instructed, if you used Gifsicle and your software doesn't suppport editing single-frame GIFs, you'll need to convert them from GIFs to PNG.

**Make sure your edited frames numbered in the same order as your original frames and saved as PNGs named like `frame001.png`!**


### 4. Compiling your final GIF with Gifski ([Download](https://github.com/ImageOptim/gifski/releases/latest))
The last tool we want to use is [Gifski](https://gif.ski). 
To compile your final GIF you'll need to fill in some parameters that to match your GIF
```
gifski --fps 9999 --width 9999 --height 9999 --extra --quality 100 --motion-quality 100 --lossy-quality 100 --repeat 0 --output compiled.gif frame*.png
```
`--fps 9999` is your desired framerate, replace 9999 with whatever the framerate of your GIF is.

`--width 9999` is the desired width in pixels of your GIF.

`--height 9999` is the desired height in pixels of your GIF.

`--extra` causes "50% slower encoding, but 1% better quality"

`--quality 100`, `--motion-quality 100`, and `--lossy-quality 100` define the general, motion, and lossy quality respectively. They all take values from 1-100 (1 meaning worst, 100 meaning best).

`--repeat 0` determines how many times the GIF will loop. -1 means never, 0 means forever, or if you provide a number greater than 0 that will be the number of times your GIF loops before it just sits on the final frame.

`--output compiled.gif` determines your final output file. If you're for some reason using my write-up in a greater chain of commands you can use `-` instead of `compiled.gif` for stdout.

## Done!
Now don't forget to back up your end results or you'll end up like me having redone this process multiple times months/years apart.
