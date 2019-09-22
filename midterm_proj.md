---
layout: default
title: "Midterm project: audio generation and processing"
---

Due: TBD

Acknowledgement: This project was inspired by two assignments ([first](http://nifty.stanford.edu/2010/zingaro-song-generator), [second](http://nifty.stanford.edu/2012/zingaro-stereo-sound-processing/)) developed by [Dan Zingaro](http://www.danielzingaro.com/).

## General requirements

In this project you will implement programs to generate and transform digital audio.

This is a pair project, so you will work with one classmate.  You should practice pair programming where you work directly with you partner, in person, to design and implement the program.

You will use git for version control.  You will need to make a new private repository on bitbucket and use the repository settings to allow both team members to access it.

The project is larger and more complex than the previous homework assignments.  You’ll want to start early and make steady, incremental progress towards completing the project requirements.

## Outline

TODO

## Sound

Sound is changes in air pressure over time.  When physical objects vibrate, they cause oscillations in air pressure that propagate outwards in a wave.  A sensor such as a microphone or your ear can detect these changes in pressure over time.

The most basic type of oscillation is a sine wave, where the changes in pressure over time are modeled by the equation

> *p* = *a* × sin(*tf* × 2π)

where *p* is pressure, *t* is time, *f* is frequency, and *a* is amplitude.  Because the sine function yields both positive and negative values, the resulting pressure values (*p*) can be both positive and negative.  The pressure values represent the instantaneous air pressure relative to the overall average air pressure.

Plotted with time on the x-axis and pressure on the y-axis, a sine wave looks like this:

> <a href="img/sound-sinewave.png"><img alt="Sine wave" src="img/sound-sinewave.png" style="width: 40em;"></a>

A *cycle* consists of one full oscillation with both positive and positive peaks:

> <a href="img/sound-sinewave-cycles.png"><img alt="Sine wave" src="img/sound-sinewave-cycles.png" style="width: 40em;"></a>

When the frequency increases, the cycles become shorter, so there are more cycles per unit of time.  The standard measurement of frequency, the Hertz (abbreviated Hz), is defined as one cycle per second.  Sound waves have frequencies that range from the low tens of Hz to tens of thousands of Hz.  We perceive sounds with higher frequencies as being higher-pitched.

## Detailed requirements

This section describes the detailed project requirements.  There are several parts.

### Binary I/O routines

TODO


