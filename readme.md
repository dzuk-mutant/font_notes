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
- Fuck Silicon Valley, fuck capitalism.


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

**This guide assumes the data types and tables are identical between OpenType and TrueType unless explicitly stated otherwise.**


-------

## Data

### [sfnt wrapper](data/sfnt.md)

The data format, and the wrapper for all of the tables.


### The tables!

You can encode them in whatever order, but [Microsoft has a list of what works best in Windows](https://docs.microsoft.com/en-gb/typography/opentype/spec/recom#optimized-table-ordering). (This might also be more performant elsewhere?)

`loca` is typically expected in a TTF font, but because our TTF fonts will have no TTF contours/outlines, this table shouldn't be necessary.


### 1. metrics and technical metadata

I still need to learn more about font metrics and make assumptions based on those. (Because most if not all of the metrics for an emoji font, given the project's assumptions, should be the same.)

These tables often do the same thing as each other, just in slightly different ways, different contexts or different encoding systems, so they are all getting lumped together.

- [main headers](tables/header.md) - **requires more writing**
- [`hhea` + `hmtx`](tables/horizontal_metrics.md): header and metrics for horizontal writing orientation
- [`vhea` + `vmtx`](tables/vertical_metrics.md): header and metrics for vertical writing orientation
- [`maxp`](tables/maxp.md) maximum profile: defines the memory requirements for the font.
- [`OS/2`](tables/os_2.md) - Windows and OpenType-specific metadata and metrics.
- [`post`](tables/post.md) - Information for PostScript printers.

`vhea` and `vmtx` aren't *technically* required, but it would be kind of ignorant and Anglocentric of us not to do them.

### 2. glyph and ligature mapping

- [`cmap`](tables/cmap.md) - **Still needs more writing and disambiguation. I'm not really done here!**
- [`GSUB`](tables/gsub.md) - **in progress.** Unclear if this is only applicable to OpenType fonts or not.

### 3. glyph data

This is where the meat of the font is. This is where the graphical information is stored.

How they are encoded is the basis for what we consider the format of the font is - so a font with sbix glyph tables is an sbix format font, a font with SVG glyph tables is an SVGinOT font, and so on.

Depending on the format, the visual information may be stored solely in one table (SVG, sbix), or two tables working together (CBDT/CBLC, COLR/CPAL).

- [`SVG`](tables/svg.md): SVGinOT
- [`sbix`](tables/sbix.md): Apple
- [`CBx`](tables/cbx.md): CBDT/CBLC - Google
- [`Cx`](tables/cx.md): COLR/CPAL - Will look into later if we see the point in doing it.


### 4. [`name`](tables/name.md)

Human-readable metadata.




### Recurring elements

#### [Platform IDs](data/platform-ids.md)

#### [Font Metrics](data/metrics.md)


-----

## Current practical notes

What I've found out while making fonts.

1. [macOS SVGinOT validation](practical/1_macos_svginot.md)
2. [SVGinOT metrics problems](practical/2_svginot_problems.md)


---

## Todo

- Refine metrics used. (There should be some descending, your use of vertical metrics is totally off)
- I don't understand what PPEM is. (Used in `sbix` strikes)

-----


## Formats

### SVGinOT (Mozilla, Adobe)

The most supported and the most ideal format. Other formats have to be done for old OS support, however.

- Windows 10 (Creators Update)
- macOS 10.14+
- iOS 12+?
- Various Linux distros
- Firefox 50+


**OpenType - otf extension**

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

- macOS 10.7+
- iOS 2.2+

**TrueType - ttf extension**


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

- Android 4.4+?
- Chrome OS (what version?)

**TrueType - ttf extension**

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

**OpenType - otf extension**



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
