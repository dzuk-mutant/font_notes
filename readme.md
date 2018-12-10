# Emoji font making

### Purpose

Orxporter needs a font encoding library, so I'm creating my own documentation and notes to facilitate making that.

The purpose of these documents is to:

1. Digest the specifications of TrueType and OpenType in a way that strips extraneous information that's not relevant to making an emoji font.
2. Provide a guide for anyone else interested and provide justifications for the design decisions that the library this documentation was made to help will make.


### Assumptions

- This is going to assume that the relationships between characters/ligatures and emoji is 1:1 (which it basically is in Emoji)
- This is going to assume that all the glyphs that will be produced in any font all have the same *square* metrics.
- These fonts should work flawlessly on their target operating systems without any complaints or compatibility issues (as long as those platforms support the emoji font format in question).


### There may be inaccuracies at this stage

I'm not an expert on digital typography and it's not entirely complete or explained yet.

There may be information I've missed so this may not provide the basis for completely working fonts. I'm pretty sure I've gone over everything that's needed for the most part, but we'll have to see how things go.


-------

## Overview of encoding

- https://docs.microsoft.com/en-gb/typography/opentype/spec/otff
- https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6.html#Intro
- [Required tables in OpenType](https://docs.microsoft.com/en-gb/typography/opentype/spec/otff#required-tables)

### OpenType and TrueType

OpenType and TrueType are both packaged in a structure known as [sfnt](https://en.wikipedia.org/wiki/SFNT). This is basically a series of tables that may or not have information that references each other.

Luckily because we're just doing emoji, there's not too much we have to store, but there still are differences in how we have to encode things between each emoji format and between TrueType and OpenType. For the sake of ease of development and understanding, This guide will try to streamline the two together as much as possible, only breaking off in divergent ways when absolutely necessary.

This guide assumes the data types and tables are identical unless explicitly stated otherwise.


-------

## Data

### [sfnt wrapper](data/sfnt.md)

The data format, and the wrapper for all of the tables.


### The tables!

You can encode them in whatever order, but [for the best Windows performance](https://docs.microsoft.com/en-gb/typography/opentype/spec/recom#optimized-table-ordering), you should do it in the following order. (This might also be more performant elsewhere?)

`loca` is typically expected in a TTF font, but because our TTF fonts will have no TTF contours/outlines, this table shouldn't be necessary.


#### 1. metrics and technical metadata

I still need to learn more about font metrics and make assumptions based on those. (Because most if not all of the metrics for an emoji font, given the project's assumptions, should be the same.)

- [main headers](tables/header.md) - **requires more writing**
- [`hhea` + `hmtx`](tables/horizontal_metrics.md): header and metrics for horizontal writing orientation
- [`vhea` + `vmtx`](tables/vertical_metrics.md): header and metrics for vertical writing orientation
- [`maxp`](tables/maxp.md) maximum profile: defines the memory requirements for the font.
- [`OS/2`](tables/os_2.md) - Windows and OpenType-specific metadata and metrics.

`vhea` and `vmtx` aren't *technically* required, but it would be kind of ignorant and Anglocentric of us not to do them.

#### 2. [`cmap`](tables/cmap.md)

**Still needs more writing and disambiguation. I'm not really done here!**

Mapping character codes to glyphs/pictures.

#### 3. [ligatures](tables/ligatures.md)

**Yet to be written.**

#### 4. [glyphs](tables/glyphs.md)

The data about the glyphs themselves.


#### 5. [`name`](tables/name.md)

Human-readable metadata.

#### 6. [`post`](tables/post.md)


Information for PostScript printers.


## Recurring elements

##### [Platform IDs](data/platform-ids.md)

##### [Font Metrics](data/metrics.md)


-----


## Formats

### SVGinOT (Mozilla, Adobe)

Stores SVG images in glyphs.

- OpenType - otf extension

```
TABLES

- head

- hhea
- hmtx
- vhea
- vmtx
- maxp
- OS/2

- cmap
- GSUB
- SVG

- name
- post

```

### sbix (Apple)
Stores glyphs as raster images rendered at multiple resolutions that's picked dynamically based on DPI and font size. These images can be a variety of formats (PNG, JPG, TIFF, etc.).

- TrueType - ttf extension


```
TABLES

- bhed (assumed)
- bdat (assumed)
- bloc (assumed)

- hhea
- hmtx
- vhea
- vmtx
- maxp
- OS/2

- cmap
- sbix

- name
- post

```



### CBx (CBDT/CBLC) (Google)
Stores glyphs as PNGs at multiple resolutions.

- TrueType - ttf extension

```
TABLES

- bhed (assumed)
- bdat (assumed)
- bloc (assumed)

- hhea
- hmtx
- vhea
- vmtx
- maxp
- OS/2

- cmap
- CBDT
- CBLC

- name
- post

```


### Cx (COLR/CPAL) (Microsoft)
Stores glyphs as multiple layers of vector graphics (COLR) that are given colour palettes (CPAL).

This is only useful for Windows 8. [Windows 10 has support for all of the above emoji formats.](https://docs.microsoft.com/en-gb/windows/desktop/DirectWrite/what-s-new-in-directwrite-for-windows-8-consumer-preview#what_s_new_for_windows_10_anniversary_update)

- OpenType - otf extension



```
TABLES

- head

- hhea
- hmtx
- vhea
- vmtx
- maxp
- OS/2

- cmap
- GSUB
- COLR
- CPAL

- name
- post

```
