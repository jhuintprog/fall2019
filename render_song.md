---
layout: default
title: "Midterm project: render_song"
---

This document describes the `render_song` program.

## Invocation, input data format

The `render_song` program, whose `main` function should be defined in `render_song.c`, is invoked as follows:

> <tt>./render&#95;song <i>songinput</i> <i>waveoutput</i></tt>

The file <tt><i>songinput</i></tt> is a text file describing a "song".  The first two values in a song file are the total number of samples, followed by the number of samples in one "beat".  For example, if these values are

<blockquote>
<pre>
352800
22050
</pre>
</blockquote>

then the song is 352,800 samples long, and one beat is 22,050 samples long.  With the fixed 44.1 KHz sample rate, this means that the song is exactly 8 seconds long, and that one beat is exactly 0.5 seconds.

Next, the song file will have a series of *directives*.  The following directives should be supported:

> <tt>N <i>b</i> <i>n</i></tt>: play MIDI note number <tt><i>n</i></tt> for <tt><i>b</i></tt> beats
>
> <tt>C <i>b</i> <i>notes</i> 999</tt>: play a chord consisting of MIDI note numbers <i>notes</i> for <tt><i>b</i></tt> beats; the value <tt>999</tt> is used to end the sequence of notes that are part of the chord
>
> <tt>P <i>b</i></tt>: pause for <tt><i>b</i></tt> beats
>
> <tt>V <i>v</i></tt>: switch to voice <tt><i>v</i></tt>, where 0 is sine, 1 is square, and 2 is sawtooth
>
> <tt>A <i>a</i></tt>: change current amplitude to <tt><i>a</i></tt>, which must be in the range 0.0 to 1.0

The program should render notes (`N`), chords (`C`), and pauses (`P`) in the order they occur in the input.  The first note, chord, or pause should be rendered starting at time zero.  Each subsequent note, chord, or pause should start where the previous note/chord/pause ended.  Note that `V` and `A` directives (to change the current voice or amplitude) don't affect the time positions of notes/chords/pauses.

By default, notes and chords are generated using sine waves, but the `V` directive can change them to be rendered using square waves or sawtooth waves.

By default, waveforms are generated using an amplitude of 0.1.  The <tt>A</tt> directive changes the current amplitude.

A [MIDI note number](https://newt.phys.unsw.edu.au/jw/notes.html) (*n*) can be converted to a frequency (*f*) using the following formula:

> *f* = 440 &times; 2<sup>(<i>n</i>-69) / 12</sup>

## Examples

Here is a fairly simple song:

<blockquote>
<pre>
352800
22050

N 0.4 63
P 0.6
N 0.4 61
P 0.6
N 0.4 63
P 0.6
N 0.4 61
P 0.6
N 0.4 63
P 0.6
N 0.4 61
P 0.6
N 0.4 63
P 0.6
N 0.4 61
P 0.6
N 0.8 66
P 1.2
N 0.8 66
P 1.2
N 0.8 66
P 1.2
</pre>
</blockquote>

When rendered to a WAVE file using `render_song`, it should sound like this:

> <audio controls><source src="snd/ticktock_simple.wav" type="audio/wav"></audio>

Here is a slightly more interesting version of the song, this time using chords:

<blockquote>
<pre>
352800
22050

C 0.4 63 66 999
P 0.6
C 0.4 61 65 999
P 0.6
C 0.4 63 66 999
P 0.6
C 0.4 61 65 999
P 0.6
C 0.4 63 66 999
P 0.6
C 0.4 61 65 999
P 0.6
C 0.4 63 66 999
P 0.6
C 0.4 61 65 999
P 0.6
C 0.8 66 70 999
P 1.2
C 0.8 66 70 999
P 1.2
C 0.8 66 70 999
P 1.2
</pre>
</blockquote>

When this version of the song is rendered to a WAVE file using `render_song`, it should sound like this:

> <audio controls><source src="snd/ticktock.wav" type="audio/wav"></audio>

Let's consider a third version, this time using square waves:

<blockquote>
<pre>
352800
22050

V 1
C 0.4 63 66 999
P 0.6
C 0.4 61 65 999
P 0.6
C 0.4 63 66 999
P 0.6
C 0.4 61 65 999
P 0.6
C 0.4 63 66 999
P 0.6
C 0.4 61 65 999
P 0.6
C 0.4 63 66 999
P 0.6
C 0.4 61 65 999
P 0.6
C 0.8 66 70 999
P 1.2
C 0.8 66 70 999
P 1.2
C 0.8 66 70 999
P 1.2
</pre>
</blockquote>

When this third version of the song is rendered to a WAVE file using `render_song`, it should sound like this:

> <audio controls><source src="snd/ticktock_sq.wav" type="audio/wav"></audio>

Here is an example of using the `A` directive to change the current amplitude:

<blockquote>
<pre>
132300
44100

A 0.005
C 0.5 63 66 999
A 0.01
C 0.5 63 66 999
A 0.02
C 0.5 63 66 999
A 0.04
C 0.5 63 66 999
A 0.08
C 1.0 63 66 999
</pre>
</blockquote>

When rendered to a WAVE file using `render_song`, it should sound like this:

> <audio controls><source src="snd/ampl.wav" type="audio/wav"></audio>

## Approach

The program should open the input text file and read the duration and beat length values from it.  It can use the duration to create a stereo sample array of an appropriate length.

The program's main loop should read directives one at a time and process them.

`N` and `C` directives can be carried out by calling the `render_voice_stereo` function from your [WAVE routines](wave.html).

If, when attempting to read a directive character, the program encounters end of file, that means that the song has ended.

If the input is malformed, the program should call the `fatal_error` function.
