---
layout: default
title: "Midterm project: render_song"
---

This document describes the `render_song` program.

## Invocation

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

By default, notes and chords are generated using sine waves, but the `V` directive can change them to be rendered using square waves or sawtooth waves.

A [MIDI note number](https://newt.phys.unsw.edu.au/jw/notes.html) (*n*) can be converted to a frequency (*f*) using the following formula:

> *f* = 440 &times; 2<sup>(*n*-69) / 12</sup>

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
