# head

- https://docs.microsoft.com/en-gb/typography/opentype/spec/head
- https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6head.html


| Type     | Name    | Description |
|:--------|:--------|:---------|
| Fixed	| version	| 0x00010000 if (version 1.0) |
| Fixed	| **fontRevision**	| set by us |
| uint32	| checkSumAdjustment |	[1] |
| uint32	| magicNumber	| set to 0x5F0F3CF5 (wtf????) |
| uint16	| **flags**	| Look at the table below for flags. |
| uint16 | **unitsPerEm** | between 64 and 16384 |
| longDateTime | **created**	| Date of creation. [2] |
| longDateTime	| **modified**	| Date of modification.[2] |
| FWord | **xMin** | For all glyph bounding boxes.[3] |
| FWord | **yMin** | For all glyph bounding boxes.[3] |
| FWord | **xMax** | For all glyph bounding boxes.[3] |
| FWord | **yMax** | For all glyph bounding boxes.[3] |
| uint16 | macStyle | Set all of these to 0, they are irrelevant for emoji. This must agree with `fsSelection` in the [OS/2 table](os_2.md). |
| uint16 | **lowestRecPPEM** | smallest readable size in pixels |
| int16 | fontDirectionHint | Depreciated - set to 2. |
| int16 | indexToLocFormat | 0 for short offsets (16), 1 for long (32). IDK what this is? |
| int16 | glyphDataFormat | 0 for current format |

1. [Microsoft](https://docs.microsoft.com/en-gb/typography/opentype/spec/head): "To compute: set it to 0, sum the entire font as uint32, then store 0xB1B0AFBA - sum. If the font is used as a component in a font collection file, the value of this field will be invalidated by changes to the file structure and font table directory, and must be ignored."
1. The date format is the number of seconds from 12:00 midnight on January 1st 1904 in GMT/UTC time zone as a 64-bit integer.
2. We're gonna create a square bounding box.


##### flags

**In an OpenType font, all TrueType flags must be set to 0 and vice versa.**

**Remember that TTX arranges this in the order they are encoded in bytes (Big-endian), so reverse your typing order.**

| bit | important? | format | desc  |
|---:|:---:|:-----|:-----|
| **0** | y? | all | If the baseline for the font is at y=0. **Set to 1. **|
| **1** | y? | all |x position of left most black bit is LSB (?). **Try 1.** |
| **2** | y? | all |scaled point size and actual point size will differ (i.e. 24 point glyph differs from 12 point glyph scaled by factor of 2). **Set to 0.**  |
| **3** | y? | all | Use integer scaling instead of fractional. **Try 1.** |
| **4** | y? | all |(used by the Microsoft implementation of the TrueType scaler) **Set to 0.** |
| **5** | y? | **TrueType** |This bit should be set in fonts that are intended to be laid out vertically, and in which the glyphs have been drawn such that an x-coordinate of 0 corresponds to the desired vertical baseline. **Try 1.** |
| 6 |  | **TrueType** | **Set to 0.** (Just because the documentation said so.) |
| **7** | n? | **TrueType** | This bit should be set if the font requires layout for correct linguistic rendering (e.g. Arabic fonts). |
| 8 |  | **TrueType** | **Set to 0.** (This is for AAT fonts.) |
| 9 |  | **TrueType** | **Set to 0.** (This bit should be set if the font contains any strong right-to-left glyphs.) |
| 10 |  | **TrueType** | **Set to 0.** |
| 11 |  | **OpenType** | Set to 1 if this font is compressed in a way that binary compatibility between input and output isn't guaranteed (like WOFF). **Set to 0.** |
| 12 |  | **OpenType** | If this font has been converted? **Set to 0.** |
| 13 |  | **OpenType** | This font has been optimised for ClearType. It should always be 0 if the font relies on bitmaps. **We don't care about ClearType so set it to 0.** |
| 14 | | all | "Last Resort font". This bit should be set if the glyphs in `cmap` are simply generic code point ranges and not actually indicative of all the glyps in the font. **Set it to 0.**
| 15 | | all | Reserved. **Set to 0.** |
