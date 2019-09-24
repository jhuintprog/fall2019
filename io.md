---
layout: default
title: "Midterm project: I/O routines"
---

This document describes the I/O routines that your programs will use to read binary data and write binary data.  This program module will also define an error reporting function called `fatal_error`.  These functions should be declared in `io.h` and defined in `io.c`.

## `fatal_error` function

You can start by implementing (in `io.c`) the `fatal_error` function. It is declared in the `io.h` header file, as follows:

```c
void fatal_error(const char *message);
```

<!--
It should print message to `stderr` and then call the `exit` function (defined in `<stdlib.h>`) with the argument 1.
-->

When this function is called, it should print a line of text to `stderr` of the form

<blockquote>
<tt>Error: <i>message</i></tt>
</blockquote>

where <tt><i>message</i></tt> is the string passed as the `message` parameter.

The purpose of `fatal_error` is to immediately exit the program if an unrecoverable error, such as unexpected input data, is encountered.  Although this is a somewhat heavy handed way of dealing with errors, it is simple.  We will see a more sohisticated and flexible way of reporting and handling runtime errors later in the course when we cover *exceptions* in C++.

## Binary I/O routines

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

or (equivalently) the code

```c
x & 0xFF
```

The most significant byte could be computed using the code

```c
(x / 256) % 256
```

or (equivalently) the code

```c
(x >> 8) & 0xFF
```

The complete value of `x` would be written by first writing the least significant byte, then writing the most significant byte.  (Aside: use your favorite C reference to look up the `&` and `>>` operators so that you can completely understand what the code examples above are doing!)

A single byte of binary data can be written to a `FILE *` file handle using the `fputc` function.  We recommend that you implement the `write_` functions using a series of calls to `fputc`, one for each byte that needs to be written.

The `write_` functions should call `fatal_error` if an error occurs while writing data.

The `write_bytes` and `write_s16_buf` functions write a sequence of `char` or `int16_t` values, respectively.  They should be implemented as a series of calls to `write_byte` or `write_s16`, respectively, one for each element of the argument array.

The `read_byte`, `read_u16`, `read_u32`, and `read_s16` functions read a single data value of type `char`, `uint16_t`, `uint32_t`, or `int16_t`, respectively, storing the value read in a variable whose address is passed as a pointer parameter to the function.

The `read_u16`, `read_u32`, and `read_s16` functions should read individual data bytes and then reconstruct the overall value.  As with the `write_` functions, the data bytes will be in little endian order.  For example, if two bytes `v1` and `v2` are read, a `uint16_t` value could be constructed using the code

```c
v1 + (v2 * 256)
```

or (equivalently)

```c
v1 | (v2 << 8)
```

We recommend that you use the `fgetc` function to read an individual data byte.  This function returns the special `EOF` value if it can't read a data byte, otherwise it returns the value of the byte (in the range 0–255.)

The `read_bytes` and `read_s16_buf` functions read a sequence of `char` or `int16_t` values, respectively, and store them in the array passed as a parameter.

If any of the `read_`  functions fail to read the required number of data bytes, they should call the `fatal_error` function.
