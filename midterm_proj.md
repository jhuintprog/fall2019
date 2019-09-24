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

The `render_echo` program adds an "echo" effect to a digital audio file.  Read the [render\_echo](render_echo.html) document for a complete specification.
