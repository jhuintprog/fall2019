---
layout: default
title: "Midterm project: testing"
---

This document describes how you can test your programs.

## Example files

The following example files are available.

### Song files

Here are some song files you can use as input to your [render\_song](render_song.html) program.

Link | Description
---- | -----------
[a440.txt](res/a440.txt) | Generate 440 Hz sine wave, output should be [a440.wav](snd/a440.wav)
[a440\_sq.txt](res/a440_sq.txt) | Generate 440 Hz square wave, output should be [a440\_sq.wav](snd/a440_sq.wav)
[a440\_saw.txt](res/a440_saw.txt) | Generate 440 Hz sawtooth wave, output should be [a440\_saw.wav](snd/a440_saw.wav)
[ticktock\_simple.txt](res/ticktock_simple.txt) | Simple version of ticktock song, output should be [ticktock\_simple.wav](snd/ticktock_simple.wav)
[ticktock.txt](res/ticktock.txt) | Better version of ticktock song using chords, output should be [ticktock.wav](snd/ticktock.wav)
[ticktock\_sq.txt](res/ticktock_sq.txt) | Version of `ticktock.txt` using square wave voice, output should be [ticktock\_sq.wav](snd/ticktock_sq.wav)

You should make your own song files, too!

### Sound files

The [render\_echo](render_echo.html) program requires a WAVE file as input.  You can use any of the files linked above, or one of the following files:

Link | Description
---- | -----------
[linux.wav](snd/linux.wav) | Linus Torvalds explaining how to pronounce "Linux"
[linux\_echo.wav](snd/linux_echo.wav) | Output of <tt>./render&#95;echo linux.wav linux&#95;echo.wav 22050 0.4</tt>
[linux\_echo2.wav](snd/linux_echo2.wav) | Output of <tt>./render&#95;echo linux&#95;echo.wav linux&#95;echo2.wav 11025 0.4</tt>
