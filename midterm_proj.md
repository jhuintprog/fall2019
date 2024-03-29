---
layout: default
title: "Midterm project: audio generation and processing"
---

Due: Thursday, October 17th by 11pm

**Acknowledgment**: This project was inspired by two assignments ([first](http://nifty.stanford.edu/2010/zingaro-song-generator), [second](http://nifty.stanford.edu/2012/zingaro-stereo-sound-processing/)) developed by [Dan Zingaro](http://www.danielzingaro.com/).

<div style="padding: 1em; border: 3px solid #b66; border-radius: 1em; background: #ecc;">
<b>Update 10/15</b>: The <a href="testing.html">testing</a> document has been updated
with information about how to use spectrograms to compare audio files.
</div>

## Introduction

In this project you will implement programs to generate and transform digital audio.

This is a pair project, so you will work with one classmate.  You should practice pair programming where you work directly with you partner, in person, to design and implement the program.  We will spend some time in class working on the project, so your partner needs to be in your section.

You will use git for version control.  You will need to make a new private repository on Bitbucket and set the repository settings to allow both team members to access it.  (In Bitbucket, one team member should create a repository, navigate to it, choose **Settings**, then choose **User and group access**, then add the other teammate with at least **Write** permission.)

The project is larger and more complex than the previous homework assignments.  You’ll want to start early and make steady, incremental progress towards completing the project requirements.

There is quite a bit of information in this project description, so it's split into several web documents.  Click the links as noted below.

## Getting started

Once you and your partner have set up your Bitbucket repository, you can get started by cloning it, and then copying the files from the `midterm-project` directory of the `cs220fa19-public` repository into it.

## Sound

You should read the [Sound](sound.html) section for some useful background on sound and how it can be represented digitally.

## Functional requirements

This section describes the project's functional requirements.  There are several parts.

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

## Tips and tricks

The [Tips and tricks](tips.html) document has some important suggestions for implementing the functional requirements.

## Testing

Please see the [testing](testing.html) document for information about how to test your programs.  This document has links to example input and output files.

## General requirements and expectations

We expect that you will submit well-designed, well-implemented, and well-tested code.  If your code does not compile or execute properly, you will receive a zero.

Run `valgrind` on your code and make sure there are no memory-related issues (memory leaks, out of bounds array accesses, uses of uninitialized memory, etc.)

Comment your code.  The general expectation for code comments is

* Each function has a comment describing the purpose of the function, parameters, and return value
* Significant regions of code within function bodies (loops, important computations, etc.) have comments explaining their intent

Commit early and often.  The way to implement a complex program is by steady progress in adding small but useful improvements to the program.  Use git commits to record these increments of progress.  Make sure that each commit has a meaningful log message.

Be a team player.  Make sure you are contributing your fair share of effort to the success of the project. Make sure that you are working closely with your partner and allowing your partner to contribute.

## Packaging requirements

Your project must include a `Makefile` which has targets which build the `render_tone`, `render_song`, and `render_echo` programs.  These targets must specify dependencies on the appropriate object (`.o`) files.  The targets for object files must have appropriate dependencies on a source (`.c`) file and any header (`.h`) files used directly or indirectly in compiling the object file.

Your `Makefile` must also have a `clean` target which deletes all object (`.o`) files and executables (`render_tone`, `render_song`, and `render_echo`.)

Keep in mind that your programs will need to link with the math library, so add `-lm` to your linking commands.

Your project must include a `README` file that contains your names and JHED IDs.

## Submitting

To submit, first generate a Git log:

```bash
git log > gitlog.txt
```

Then create a zipfile with with your `.c` files, `.h` files, `Makefile`, `README`, and Git log:

```bash
zip midterm_proj.zip Makefile *.c *.h README gitlog.txt
```

Upload `midterm_proj.zip` to Gradescope as **Midterm project**.

Make sure that you do **not** submit any WAVE (`.wav`) files.
