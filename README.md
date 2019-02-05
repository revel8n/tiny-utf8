# TINY <img src="https://github.com/DuffsDevice/tinyutf8/raw/master/UTF8.png" width="47" height="47" align="top" alt="UTF8 Art" style="display:inline;">

[![Build Status](https://travis-ci.org/DuffsDevice/tinyutf8.svg?branch=master)](https://travis-ci.org/DuffsDevice/tinyutf8)&nbsp;&nbsp;[![Licence](https://img.shields.io/badge/licence-BSD--3-e20000.svg)](https://github.com/DuffsDevice/tinyutf8/blob/master/LICENCE)

### DESCRIPTION
TINYUTF8 is a library for extremely easy integration of Unicode into an arbitrary C++11 project.
The library consists solely of the class `utf8_string`, which acts as a drop-in replacement for `std::string`.
Its implementation is successfully in the middle between small memory footprint and fast access.

#### FEATURES
- **Drop-in replacement for std::string**
- **Very Lightweight** (~4K LOC)
- **Very fast**, i.e. highly optimized decoder, encoder and traversal routines
- **Advanced Memory Layout**, i.e. Random Access is O( #Codepoints > 127 ) for the average case
- **Small String Optimization** (SSO) for strings up to an UTF8-encoded length of `sizeof(utf8_string)-1`
- **Growth in Constant Time** (Amortized)
- **Conversion between UTF32 and UTF8**
- Small Stack Size, i.e. `sizeof(utf8_string)` = 16 Bytes (32Bit) / 32 Bytes (64Bit)
- Codepoint Range of `0x0` - `0xFFFFFFFF`, i.e. 1-7 Code Units/Bytes per Codepoint (Note: This is more than specified by UTF8, but until now otherwise considered out of scope)
- Single header file, single source file
- Straightforward C++11 Design
- Possibility to prepend the UTF8 BOM (Byte Order Mark) to any string when converting it to an std::string
- Supports raw (Byte-based) access for occasions where Speed is needed.
- Supports `shrink_to_fit()`
- Malformed UTF8 sequences will **lead to defined behaviour**

## THE PURPOSE OF TINYUTF8
Back when I decided to write a UTF8 solution for C++, I knew I wanted a drop-in replacement for `std::string`. At the time mostly because I found it neat to have one and felt C++ always lacked accessible support for UTF8. Since then, several years have passed and the situation has not improved much. That said, things currently look like they are about to improve - but that doesn't say much, does it?

The opinion shared by many "experienced Unicode programmers" (e.g. by [UTF-8 Everywhere](utf8everywhere.org)) is that "non-experienced" programmers both *under* and *over*estimate the need for Unicode- and encoding-specific treatment: This need is...
  1. **overestimated**, because many times we really should care less about codepoint/grapheme borders within string data;
  2. **underestimated**, because if we really want to "support" unicode, we need to think about *normalizations*, *visual character comparisons*, *reserved codepoint values*, *illegal code unit sequences* and so on and so forth.

Unicode is not rocket science but nonetheless hard to get *right*. The benefit of TINYUTF8 is not to have [ICU](http://site.icu-project.org/) re-implemented in C++. The goal of TINYUTF8 is to
  1. bridge as many gaps to "supporting Unicode" as possible by 'just' replacing `std::string` with a custom class which means to
  2. provide you with a Codepoint Abstraction Layer that takes care of the Run-Length Encoding, without you noticing.

TINYUTF8 does not give you the Unicode puzzle, its the first piece.

#### WHAT TINYUTF8 IS NOT AIMED FOR
- Conversion between ISO encodings and UTF8
- Interfacing with UTF16
- Visible character comparison (`'ch'` vs. `'c'+'h'`)
- Codepoint Normalization
- Correct invalid Code Unit sequences

Note: ANSI suppport was dropped in Version 2.0 in favor of execution speed.

## EXAMPLE USAGE

```cpp
#include <iostream>
#include <algorithm>
#include <tinyutf8.h>
using namespace std;

int main()
{
    utf8_string str = u8"!🌍 olleH";
    for_each( str.rbegin() , str.rend() , []( char32_t codepoint ){
      cout << codepoint;
    } );
    return 0;
}
```

## BUGS

If you encounter any bugs, please file a bug report through the "Issues" tab.
I'll try to answer it soon!

Jakob
