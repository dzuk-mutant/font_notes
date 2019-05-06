# Emoji font making

### Purpose

In order to make [forc](https://github.com/mutantstandard/forc), an emoji creation tool, I've been creating my own documentation and notes to facilitate making that.

The purpose of this is to:

- Digest the specifications of TrueType and OpenType in a way that's easier to digest, and strip extraneous information that's not relevant to making an emoji font.
- Create connections between various separate aspects of font making to streamline the understanding of making an emoji font.
- Provide justifications to decisions made in forc, because there is so much information to digest when embarking on something like this.
- Provide a guide for anyone else interested in font creation but finds the standard documentation a bit scary.


### Assumptions

- This is going to mostly assume that the relationships between characters/ligatures and emoji is 1:1 (which it basically is in Emoji)
- This is going to assume that the font is monospaced.
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

### [sfnt wrapper](misc/sfnt.md)

The data format, and the wrapper for all of the tables.


### The tables!

You can encode them in whatever order, but [Microsoft has a list of what works best in Windows](https://docs.microsoft.com/en-gb/typography/opentype/spec/recom#optimized-table-ordering). (This might also be more performant elsewhere?)

`loca` is typically expected in a TTF font, but because our TTF fonts will have no TTF contours/outlines, this table shouldn't be necessary.


### 1. metrics and technical metadata

I still need to learn more about font metrics and make assumptions based on those. (Because most if not all of the metrics for an emoji font, given the project's assumptions, should be the same.)

These tables often do the same thing as each other, just in slightly different ways, different contexts or different encoding systems, so they are all getting lumped together.

- [`head`](tables/head.md)
- [`hhea` + `hmtx`](tables/horizontal_metrics.md): header and metrics for horizontal writing orientation
- [`vhea` + `vmtx`](tables/vertical_metrics.md): header and metrics for vertical writing orientation
- [`maxp`](tables/maxp.md) maximum profile: defines the memory requirements for the font.
- [`OS/2`](tables/os_2.md) - Windows and OpenType-specific metadata and metrics.
- [`post`](tables/post.md) - Information for PostScript printers.

`vhea` and `vmtx` aren't *technically* required, but it would be kind of ignorant and western-centric to not at least give it a basic shake.

### 2. glyph and ligature mapping


##### single glyphs
- [`cmap`](tables/cmap.md) - **Still needs more writing and disambiguation. I'm not really done here!**

##### ligatures
- [`GSUB`](tables/gsub.md) - OpenType ligatures.
- `morx` - TrueType ligatures (might not be necessary if I can just make the sbix font OpenType without macOS/iOS complaining) (https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6morx.html)

### 3. placeholder tables

There are various tables that are required in some form to please font rendering clients, even when they aren't actually doing anything.

- `loca` - Index to Location (macOS would rather this be here, even if it's empty)
- `gasp` - Grid-fitting and Scan-conversion Procedure Table (Google Fonts recommends at least a basic table for this)
- `DSIG` - digital signature table (Google Fonts recommends at least a basic table for this, even if it doesnt actually have any signatures)


### 4. glyph data

This is where the meat of the font is. This is where the graphical information is stored.

How they are encoded is the basis for what we consider the format of the font is - so a font with sbix glyph tables is an sbix format font, a font with SVG glyph tables is an SVGinOT font, and so on.

Depending on the format, the visual information may be stored solely in one table (`SVG`, `sbix`), or two tables working together (`CBDT/CBLC`, `COLR/CPAL`).

#### [`SVG`](tables/svg.md) (SVGinOT)
The most supported and the most ideal format.

- Windows 10 (Creators Update)
- macOS 10.14+ (unclear if this can be implemented system-wide or not)
- iOS 12+?
- Various Linux distros
- Firefox 50+

#### [`sbix`](tables/sbix.md) (Apple)
The most supported colour bitmap format, primarily used by Apple.

- macOS 10.7+
- iOS 7+ (technically iOS 2.2+, but iOS 7 introduced Configuration Profiles which enable the installation of 3rd party fonts)
- Windows 10 (Creators Update)

#### [`CBDT/CBLC`](tables/cbx.md) (Google)
Colour bitmap format, primarily used by Google.

- Android 4.4+
- Seems to be a thing in Chrome OS but how one would use different fonts in it is unknown to me.
- Windows 10 (Creators Update)

#### [`COLR/CPAL`](tables/cx.md)
Vector graphics format, primarily used by Microsoft for their emoji font.

It's unclear if this guide will actually look into this format right now.

#### [Differences between bitmap table formats](misc/bitmap_table_differences.md)



### 5. [`name`](tables/name.md)

Human-readable metadata.

---

## Recurring elements

- [**Platform IDs**](misc/platform_ids.md)
- [**Font Metrics**](misc/metrics.md)
- [**Font Versioning**](misc/font_version.md)
- [**Mandatory/Conventional Glyphs**](misc/glyphs.md)



-----

## Current practical notes

What I've found out while making fonts.

1. [macOS SVGinOT validation](practical/1_macos_svginot.md)
2. [SVGinOT metrics problems](practical/2_svginot_problems.md)


-----

## iOS Configuration Profile

Creating a Configuration profile to install fonts on iOS

- [Norbert Lindenberg - Installing Fonts on iOS](https://norbertlindenberg.com/2015/06/installing-fonts-on-ios/)
- [Apple's Configuration Profile reference PDF](https://developer.apple.com/business/documentation/Configuration-Profile-Reference.pdf)


---

## Todo

- Refine metrics used. (There should be some descending, your use of vertical metrics is totally off)
