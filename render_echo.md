---
layout: default
title: "Midterm project: render_echo"
---

This document describes the requirements for the `render_echo` program.

## Invocation

The `render_echo` program processes an input WAVE file to generate an echo effect, and is invoked as follows:

> <tt>./render&#95;echo <i>wavfilein</i> <i>wavfileout</i> <i>delay</i> <i>amplitude</i></tt>

The <tt><i>wavfilein</i></tt> argument names an input WAVE file.

The <tt><i>wavfileout</i></tt> argument names an output WAVE file to write, which will be a transformed version of <tt><i>wavfilein</i></tt>.

The <tt><i>delay</i></tt> argument is an integer delay, specified as a number of samples.

The <tt><i>amplitude</i></tt> argument is a floating point value in the range 0.0 to 1.0, specifying a relative amplitude of the echo effect to generate.

## Generating an echo

Generating an echo is simple: the program should add an attenuated copy of the audio signal at an offset as indicated by the <tt><i>delay</i></tt> argument.  For example, if <tt><i>delay</i></tt> is 22050, that indicates a delay of 0.5 seconds.  The amplitude of the copied signal is specified by the <tt><i>amplitude</i></tt> argument.  For example, if <tt><i>amplitude</i></tt> is 0.4, then the echoed copy of the signal should be 40% of the amplitude of the original.

## Examples

As an example, let's say that `ticktock_simple.wav` is the original version of the example song described in the [render\_song](render_song.html) specification, and we run the command

<blockquote>
<pre>
./render&#95;echo ticktock&#95;simple.wav ticktock&#95;echo.wav 11025 0.4
</pre>
</blockquote>

Recall that the original sounded like this:

> <audio controls><source src="snd/ticktock_simple.wav" type="audio/wav"></audio>

The result is this:

> <audio controls><source src="snd/ticktock_echo.wav" type="audio/wav"></audio>

The following (famous!) audio clip was used to test sound support early in the development of the Linux operating system:

> <audio controls><source src="snd/linux.wav" type="audio/wav"></audio>

Assuming that this file is called `linux.wav`, we could add an echo with a 0.5 second delay with the command:

<blockquote>
<pre>
./render&#95;echo linux.wav linux&#95;echo.wav 22050 0.4
</pre>
</blockquote>

This yields the following result:

> <audio controls><source src="snd/linux_echo.wav" type="audio/wav"></audio>

We can add an additional echo with a 0.25 second delay with the command:

<blockquote>
<pre>
./render&#95;echo linux&#95;echo.wav linux&#95;echo2.wav 11025 0.4
</pre>
</blockquote>

The result of this second command is the following:

> <audio controls><source src="snd/linux_echo2.wav" type="audio/wav"></audio>

## Approach

Your program will need to open the input file, call `read_wave_header` to read the WAVE header and get the number of (stereo) samples, then read the sample data into an array.  Adding the echo effect can be accomplished by adding attenuated sample values to the sample value at a later audio position, effectively mixing the echo into the original audio.  Note that you need to be careful about the order in which you process the samples!

Once the sample data has been updated, open the output file, call `write_wave_header` to write the WAVE header, then write the updated sample values.
