---
layout: default
title: "Midterm project: render_tone"
---

The `render_tone` program renders a continuous tone with a specified voice, frequency, amplitude, and duration.  This document describes the requirements for the `render_tone` program.

## Invocation

The program is invoked as

> <tt>./render\_tone <i>voice</i> <i>frequency</i> <i>amplitude</i> <i>numsamples</i> <i>wavfileout</i></tt>

The <tt><i>voice</i></tt> argument is an integer specifying the waveform shape to generate: 0 for sine wave, 1 for square wave, 2 for sawtooth wave.  (See the [WAVE routines](wave.html) document for information about how to generate square and sawtooth waves.)

The <tt><i>frequency</i></tt> argument is a floating-point value specifying the frequency of the tone in Hz (cycles per second.)

The <tt><i>amplitude</i></tt> argument is a floating-point value specifying the relative amplitude of the tone, with 0.0 being complete silence, to 1.0 being maximum volume.

The <tt><i>numsamples</i></tt> argument is the number of stereo samples to generate.  Because the sampling frequency is fixed at 44.1 KHz, there are 44,100 stereo samples per second.  (Remember that a stereo sample consists of two signed 16-bit values, one for each audio channel.)

The <tt><i>wavfileout</i></tt> argument specifies the name of the output WAVE file to generate.

## Examples

Here are some examples of invoking the `render_tone` program and what the resulting wave file should sound like.

Rendering a 130.81 Hz sine wave:

```bash
./render_tone 0 130.81 0.1 44100 c131_sine.wav
```

Resulting audio:

> <audio controls><source src="snd/c131_sine.wav" type="audio/wav"></audio>

Rendering a 130.81 Hz square wave:

```bash
./render_tone 1 130.81 0.1 44100 c131_sq.wav
```

Resulting audio:

> <audio controls><source src="snd/c131_sq.wav" type="audio/wav"></audio>

Rendering a 130.81 Hz sawtooth wave:

```bash
./render_tone 2 130.81 0.1 44100 c131_saw.wav
```

Resulting audio:

> <audio controls><source src="snd/c131_saw.wav" type="audio/wav"></audio>
