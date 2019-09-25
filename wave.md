---
layout: default
title: "Midterm project: WAVE routines"
---

This document describes functions for working with WAVE files and digital audio waveforms.  This module of the program consists of the header file `wave.h` and the source file `wave.c`.

The `write_wave_header` and `read_wave_header` functions require the [binary I/O functions](io.html), so implement those first.

## `wave.h`

The header file `wave.h` has the following suggested contents:

```c
#include <stdint.h>

#define PI                 3.14159265358979323846
#define SAMPLES_PER_SECOND 44100u
#define NUM_CHANNELS       2u
#define BITS_PER_SAMPLE    16u

/* voices */
#define SINE       0
#define SQUARE     1
#define SAW        2
#define NUM_VOICES 3 /* one greater than maximum legal voice */

void write_wave_header(FILE *out, unsigned num_samples);
void read_wave_header(FILE *in, unsigned *num_samples);
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

The constants `SAMPLES_PER_SECOND`, `NUM_CHANNELS`, and `BITS_PER_SAMPLE` define the parameters of stereo CD-quality PCM audio.

The constant `PI` closely approximates the value of π.

The constants `SINE`, `SQUARE`, and `SAW` distinguish the three kinds of audio waveforms that the [render\_song](render_song.html) program can generate.

The function declarations are for functions defined in `wave.c`: these are described in more detail below.

## `read_wave_header`, `write_wave_header`

Two functions called `read_wave_header` and `write_wave_header` are provided (including implementations in `wave.c`) which, respectively, read the header information from a WAVE file, and write header information for a WAVE file.  WAVE files are essentially just header information followed by a sequence of sample values.

You can look at the implementation of `read_wave_header` and `write_wave_header` to see how they use your binary I/O functions.  Note that both routines are hard-coded to support only 16 bits per sample, 44.1 KHz, stereo (two channel) WAVE files.  For our purposes, the only "interesting" value in the WAVE header is the number of *stereo* samples, which determines the length of the audio.  Note that a stereo sample consists of *two* 16-bit signed integer values, one for each channel (left and right).

Here is a very simple example program that calls `read_wave_header`:

```c
#include <stdio.h>
#include <stdlib.h>
#include "io.h"
#include "wave.h"

int main(int argc, char **argv) {
  if (argc != 2) {
    fprintf(stderr, "Usage: %s <wave file>\n", argv[0]);
    exit(1);
  }

  FILE *in = fopen(argv[1], "rb");
  if (in == NULL) {
    fatal_error("Couldn't open wave file");
  }

  unsigned num_samples;
  read_wave_header(in, &num_samples);
  printf("Wave file has %u stereo samples\n", num_samples);

  fclose(in);

  return 0;
}
```

Let's say that we compile and link this program as an executable called `readwav`.  If we run the program on the WAVE file called [a440.wav](snd/a440.wav) as follows

```bash
./readwav a440.wav
```

The output will be

```
Wave file has 44100 stereo samples
```

## Rendering tones

The `render_sine_wave`, `render_square_wave`, and `render_saw_wave` functions generate (respectively) a sine wave, square wave, or sawtooth wave of the specified frequency into the specified stereo sample buffer.  These functions take several parameters:

* The `buf` parameter is the pointer to the first element of an array of `int16_t` elements, each pair of elements representing the left and right samples at one instant in a 44.1 KHz audio stream
* The `num_samples` indicates how many samples to generate; this effectively determines the duration of the rendered audio waveform
* The `channel` parameter indicates which channel to generate (0=left, 1=right)
* The `freq_hz` parameter specifies the frequency of the generated waveform in Hz (cycles per second)
* The `amplitude` parameter indicates the relative amplitude of the generated waveform, where 1.0 is the maximum possible amplitude

A square wave is a waveform where the generated sample values oscillate between maximum and minimum sample values (with respect to the amplitude), i.e.

> <a href="img/sound-square.png"><img alt="Square wave" src="img/sound-square.png" style="width: 40em;"></a>

Square waves are easy to generate using digital electronics, and were used frequently in early home computers and video games.  Here is a 440 Hz square wave:

> <audio controls><source src="snd/a440_sq.wav" type="audio/wav"></audio>

A sawtooth wave rises steadily from the minimum sample value to the maximum sample value (with respect to amplitude) on each cycle, i.e.

> <a href="img/sound-saw.png"><img alt="Sawtooth wave" src="img/sound-saw.png" style="width: 40em;"></a>

Sawtooth waves have a more "horn-like" sound than a plain sine wave.  Here is a 440 Hz sawtooth wave:

> <audio controls><source src="snd/a440_saw.wav" type="audio/wav"></audio>

The `render_sine_wave_stereo`, `render_square_wave_stereo`, and `render_saw_wave_stereo` functions render a sine, square, or sawtooth waves into both channels of a stereo sample buffer.

The `render_voice` function renders either a sine, square, or sawtooth wave into one channel (0=left, 1=right) of a stereo sample buffer, as indicated by the `voice` parameter.  The value of `voice` should be either `SINE`, `SQUARE`, or `SAW`.

The `render_voice` function renders a sine, square, or sawtooth wave into both channels of a stereo sample buffer, as indicated by the `voice` parameter.

**Important**: Note that you do not need to implement any of the `render_` functions exactly as described above.  You will need to implement functions to generate tones, but you are free to implement them in whatever way you think is appropriate.

**Recommendation**: Don't implement all of the audio generation functions at once.  Implement them as needed to support the three programs you will be implementing, `render_tone`, `render_song`, and `render_echo`.

## Rendering is additive!

**Extremely important consideration**: Your tone-rendering functions should be `additive`, meaning that the waveforms that are rendered should be *added* to whatever sample values are already present.  This is necessary, for example, to allow the [render\_song](render_song.html) program to produce chords, where several frequencies are generated at the same time.

There are a couple of things you should think about in order to make this work correctly:

1. You will want all sample values to be initialized to zero, prior to any of the `render_` functions being called.  This ensures that the initial signal is silence.
2. If the result of adding a new sample value to an existing sample value is either greater than 32,767 or less than -32,768 (these are the maximum and minimum `int16_t` values, respectively), then your code should clamp the result at 32,767 or -32,768.  (See the [Sound](sound.html#mixing-digital-audio-signals-clipping) document for details.)  To detect when a computed sample value is outside the legal range, your code will need to do the addition using a data type with a larger range of possible values (e.g., `int32_t`.)
