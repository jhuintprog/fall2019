---
layout: default
title: "Midterm project: audio generation and processing"
---

Due: TBD

*This is a preliminary project description, so definitely not official!*

**Acknowledgement**: This project was inspired by two assignments ([first](http://nifty.stanford.edu/2010/zingaro-song-generator), [second](http://nifty.stanford.edu/2012/zingaro-stereo-sound-processing/)) developed by [Dan Zingaro](http://www.danielzingaro.com/).

## General requirements

In this project you will implement programs to generate and transform digital audio.

This is a pair project, so you will work with one classmate.  You should practice pair programming where you work directly with you partner, in person, to design and implement the program.

You will use git for version control.  You will need to make a new private repository on bitbucket and use the repository settings to allow both team members to access it.

The project is larger and more complex than the previous homework assignments.  You’ll want to start early and make steady, incremental progress towards completing the project requirements.

## Outline

TODO

## Sound

You should read the [Sound](sound.html) section for some useful background on sound and how it can be represented digitally.

## Detailed requirements

This section describes the detailed project requirements.  There are several parts.

### `fatal_error` function

You can start by implementing (in `io.c`) the `fatal_error` function. It is declared in the `io.h` header file, as follows:

```c
void fatal_error(const char *message);
```

It should print the specified message to `stderr` and then call the `exit` function (defined in `<stdlib.h>`) with the argument 1.

The purpose of `fatal_error` is to immediately exit the program if an unrecoverable error, such as unexpected input data, is encountered.  Although this is a somewhat heavy handed way of dealing with errors, it is simple.  We will see a more sohisticated and flexible way of reporting and handling runtime errors later in the course when we cover *exceptions* in C++.

### Binary I/O routines

WAVE files are *binary* files, meaning their contents consist of arbitrary byte values.

Furthermore, the specific type of WAVE file generated and transformed by the programs you will implement in this assignment use *little-endian* binary data, where multi-byte values are represented as a sequence of bytes such that less significant bytes occur before more significant bytes.

You should start your work by implementing the following binary I/O (input and output) functions, which are declared in the `io.h` header file:

```c
void write_byte(FILE *out, char val);
void write_bytes(FILE *out, const char data[], unsigned n);
void write_u16(FILE *out, uint16_t value);
void write_u32(FILE *out, uint32_t value);
void write_s16(FILE *out, int16_t value);
void write_s16_buf(FILE *out, const int16_t buf[], unsigned n);

void read_byte(FILE *in, char *val);
void read_bytes(FILE *in, char data[], unsigned n);
void read_u16(FILE *in, uint16_t *val);
void read_u32(FILE *in, uint32_t *val);
void read_s16(FILE *in, int16_t *val);
void read_s16_buf(FILE *in, int16_t buf[], unsigned n);
```

The `write_byte`, `write_u16`, `write_u32`, and `write_s16` functions write a single data value of type `char`, `uint16_t`, `uint32_t`, or `int16_t` respectively.

Multi-byte data values (`uint16_t`, `uint32_t`, `int16_t`) must be written in *little-endian* format, such that less significant bytes are written before more significant bytes.  For example, a `uint16_t` data value consists of two bytes.  Lets say we want to write a `uint16_t` value `x`.  The least significant byte can be computed using the code

```c
x % 256
```

or the code

```c
x & 0xFF
```

The most significant byte could be computed using the code

```c
(x / 256) % 256
```

or the code

```c
(x >> 8) & 0xFF
```

A single byte of binary data can be written to a `FILE *` file handle using the `fputc` function.  We recommend that you implement the `write_` functions using a series of calls to `fputc`, one for each byte that needs to be written.

The `write_` functions should call `fatal_error` if an error occurs while writing data.

The `write_bytes` and `write_s16_buf` functions write a sequence of `char` or `int16_t` values, respectively.  They should be implemented as a series of calls to `write_byte` or `write_s16`, respectively, one for each element of the argument array.

The `read_byte`, `read_u16`, `read_u32`, and `read_s16` functions read a single data value of type `char`, `uint16_t`, `uint32_t`, or `int16_t`, respectively, storing the value read in a variable whose address is passed as a pointer parameter to the function.  Each data value should be read as a sequence of byte values in little-endian format.

*TODO* say more about how to read little-endian bytes and reconstruct a multi-byte data value.

The `read_bytes` and `read_s16_buf` functions read a sequence of `char` or `int16_t` values, respectively, and store them in the array passed as a parameter.

### Wave file routines

Once you have implemented the binary I/O functions in `io.h`, you can read and write WAVE files.  Two functions called `read_wav_header` and `write_wav_header` are provided (including implementations in `wave.c`) which, respectively, read the header information from a WAVE file, and write header information for a WAVE file.  WAVE files are essentially just header information followed by a sequence of sample values.

You can look at the implementation of `read_wav_header` and `write_wav_header` to see how they use your binary I/O functions.  Note that both routines are hard-coded to support only 16 bits per sample, 44.1 KHz, stereo (two channel) WAVE files.

The header file `wave.h` has the following suggested contents:

```c
#define PI 3.14159265358979323846
#define SAMPLES_PER_SECOND 44100u
#define BITS_PER_SAMPLE 16u

/* voices */
#define SINE       0
#define SQUARE     1
#define SAW        2
#define NUM_VOICES 3 /* one greater than maximum legal voice */

void write_wav_header(FILE *out, unsigned num_samples);
void read_wav_header(FILE *in, unsigned *num_samples);
void render_sine_wave(int16_t buf[], unsigned num_samples, unsigned channel,
  float freq_hz, float amplitude);
void render_sine_wave_stereo(int16_t buf[], unsigned num_samples,
  float freq_hz, float amplitude);
void render_square_wave(int16_t buf[], unsigned num_samples, unsigned channel,
  float freq_hz, float amplitude);
void render_square_wave_stereo(int16_t buf[], unsigned num_samples,
  float freq_hz, float amplitude);
void render_saw_wave(int16_t buf[], unsigned num_samples, unsigned channel,
  float freq_hz, float amplitude);
void render_saw_wave_stereo(int16_t buf[], unsigned num_samples,
  float freq_hz, float amplitude);
void render_voice(int16_t buf[], unsigned num_samples, unsigned channel,
  float freq_hz, float amplitude, unsigned voice);
void render_voice_stereo(int16_t buf[], unsigned num_samples, float freq_hz,
  float amplitude, unsigned voice);
```

The `render_sine_wave`, `render_square_wave`, and `render_saw_wave` functions generate (respectively) a sine wave, square wave, or sawtooth wave of the specified frequency into the specified stereo sample buffer.  The `channel` parameter indicates which channel to generate (0=left, 1=right).  The `buf` parameter is the pointer to the first element of an array of `int16_t` elements, each pair of elements representing the left and right samples at one instant in a 44.1 KHz audio stream.

The `render_sine_wave_stereo`, `render_square_wave_stereo`, and `render_saw_wave_stereo` functions render a sine, square, or sawtooth waves into both channels of a stereo sample buffer.

The `render_voice` function renders either a sine, square, or sawtooth wave into one channel (0=left, 1=right) of a stereo sample buffer, as indicated by the `voice` parameter.  The value of `voice` should be either `SINE`, `SQUARE`, or `SAW`.

The `render_voice` function renders a sine, square, or sawtooth wave into both channels of a stereo sample buffer, as indicated by the `voice` parameter.

**Important**: Note that you do not need to implement any of the `render_` functions exactly as described above.  You will need to implement functions to generate tones, but you are free to implement them in whatever way you think is appropriate.

**Recommendation**: Don't implement all of the audio generation functions at once.  Implement them as needed to support the three programs you will be implementing, `render_tone`, `render_song`, and `render_echo`.

### `render_tone` program

*TODO* describe the `render_tone` program

### `render_song` program

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
