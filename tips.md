---
layout: default
title: "Midterm project: tips and tricks"
---

This document has some useful tips and tricks to consider as you work on the project.

## Passing part of an array to a function

Because arrays are passed as pointers (where the pointer points to the first element of the array), you can pass *part* of an array to a function by passing a pointer to one of the array's elements.

For example, let's say that you want to pass part of the `sample_buf` array to the `render_sine_wave` function, starting at index `start`.  You could do something like this:

```c
render_sine_wave(&sample_buf[start], /* ...other arguments... */)
```

## Clearing the sample buffer before rendering a waveform

Because generated waveforms should be rendered additively, you'll need to make sure your sample buffer is filled with 0 values initially.

## Math functions

Standard math functions such as `sin`, `pow`, `floor`, and potentially others will be useful for rendering waveforms.

## Converting command line arguments to numeric values

The `render_tone` and `render_echo` programs both take command line arguments which need to be converted to a numeric value.  The `sscanf` function (available if you include the `<stdio.h>` standard header) is very useful for this purpose, since it reads text data from a character string.

For example: the `render_tone` program's second command line argument is a floating point value specifying the frequency of the generated tone.  Your `main` function could use code like the following to get the frequency:

```c
float freq;
if (sscanf(argv[2], "%f", &freq) != 1) {
  fatal_error("Invalid frequency");
}
```

Note that you will need to be careful about using the correct placeholder, depending on the type of the variable you want to use to store the data.  For example, to read a floating-point value into a `double` variable, you need to use the `%lf` placeholder.

## Use binary mode for input and output

For reading and writing binary files such as WAVE files, make sure you use "b" in the file mode.  For example:

```c
FILE *in = fopen("input.wav", "rb");
FILE *out = fopen("output.wav", "wb");
```

The "b" flag is optional on Unix systems (including Linux and MacOS), but is necessary on some systems (such as Windows).
