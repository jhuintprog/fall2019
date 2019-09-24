---
layout: default
title: "Midterm project: audio generation and processing"
---

Due: TBD

*This is a preliminary project description, so definitely not official!*

**Acknowledgement**: This project was inspired by two assignments ([first](http://nifty.stanford.edu/2010/zingaro-song-generator), [second](http://nifty.stanford.edu/2012/zingaro-stereo-sound-processing/)) developed by [Dan Zingaro](http://www.danielzingaro.com/).

## Introduction

In this project you will implement programs to generate and transform digital audio.

This is a pair project, so you will work with one classmate.  You should practice pair programming where you work directly with you partner, in person, to design and implement the program.  We will spend some time in class working on the project, so your partner needs to be in your section.

You will use git for version control.  You will need to make a new private repository on Bitbucket and set the repository settings to allow both team members to access it.  (In Bitbucket, one team member should create a repository, navigate to it, choose **Settings**, then choose **User and group access**, then add the other teammate with at least **Write** permission.)

The project is larger and more complex than the previous homework assignments.  You’ll want to start early and make steady, incremental progress towards completing the project requirements.

There is quite a bit of information in this project description, so it's split into several web documents.  Click the links as noted below.

## Outline

TODO

## Sound

You should read the [Sound](sound.html) section for some useful background on sound and how it can be represented digitally.

## Detailed requirements

This section describes the detailed project requirements.  There are several parts.

### I/O routines

Your first task is to (at least partially) implement a set of functions for error reporting and binary input and output.  These will be defined in the header `io.h` and the source file `io.c`.  Read the [I/O routines](io.html) document for complete details.

### Wave file routines

After you've completed `fatal_error` and the binary I/O functions, you can work on the [WAVE routines](wave.html), which allow your programs to read and write WAVE files, and render digital audio waveforms.

### `render_tone` program

Once you have the WAVE file routines at least partially working, you can implement the [render\_tone](render_tone.html) program.  This is the simplest program in the assignment; it just renders a single tone to an output WAVE file.

### `render_song` program

The `render_song` program reads a song file in a text-based format, and renders note and chords as digital audio.  Read the [render\_song](render_song.html) document for a complete specification.

### `render_echo` program

The `render_echo` program processes an input WAVE file to generate an echo effect, and is invoked as follows:

> <tt>./render&#95;echo <i>wavfilein</i> <i>wavfileout</i> <i>delay</i> <i>amplitude</i></tt>

The <tt><i>wavfilein</i></tt> argument names an input WAVE file.

The <tt><i>wavfileout</i></tt> argument names an output WAVE file to write, which will be a transformed version of <tt><i>wavfilein</i></tt>.

The <tt><i>delay</i></tt> argument is an integer delay, specified as a number of samples.

The <tt><i>amplitude</i></tt> argument is a floating point value in the range 0.0 to 1.0, specifying a relative amplitude of the echo effect to generate.

Generating a echo is simple: the program should add an attenuated copy of the audio signal at an offset as indicated by the <tt><i>delay</i></tt> argument.  For example, if <tt><i>delay</i></tt> is 22050, that indicates a delay of 0.5 seconds.  The amplitude of the copied signal is specified by the <tt><i>amplitude</i></tt> argument.  For example, if <tt><i>amplitude</i></tt> is 0.4, then the echoed copy of the signal should be 40% of the amplitude of the original.

As an example, let's say that <tt>ticktock.wav</tt> is the original version of the example song described in the `render_song` section, and we run the command

<blockquote>
<pre>
./render&#95;echo ticktock.wav ticktock&#95;echo.wav 11025 0.4
</pre>
</blockquote>

The result is this:

> <audio controls><source src="snd/ticktock_echo.wav" type="audio/wav"></audio>
