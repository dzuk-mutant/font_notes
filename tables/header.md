# header

- https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6head.html

The header table contains basic, global information about the font.


### Header types
Depending on what the glyps are, the table to achieve this may be different, but both of these are byte-for-byte identical. The presence of them is merely a way of indicating what kind of glyphs are in the rest of the font.

- If the font has outlines/contours, the `head` table should be used.
- If the font has no outlines/contours, only bitmaps, the `bhed` table should be used.


#### `head`
- https://docs.microsoft.com/en-gb/typography/opentype/spec/head
- https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6head.html

#### `bhed`
This is a TrueType header table that indicates that this font is wholly bitmap (ie. sbix). The font must have no `head` or `glyf` tables for this to be used.

- https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6bhed.html

It also requires `bdat` and `bloc` tables as dependencies:

- https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6bdat.html
- https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6bloc.html


### Header information

Either in `head` or `bhed`, it's the same:

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

1. Apple: "To compute: set it to 0, calculate the checksum for the 'head' table and put it in the table directory, sum the entire font as a uint32_t, then store 0xB1B0AFBA - sum. (The checksum for the 'head' table will be wrong as a result. That is OK; do not reset it.)"
1. The date format is the number of seconds from 12:00 midnight on January 1st 1904 in GMT/UTC time zone as a 64-bit integer.
2. We're gonna create a square bounding box.


##### flags

| bit | important? |  desc  |
|---:|:---:|:-----|
| **0** | y? | y value of 0 specifies baseline (?) |
| **1** | y? | x position of left most black bit is LSB (?) |
| **2** | y? | scaled point size and actual point size will differ (i.e. 24 point glyph differs from 12 point glyph scaled by factor of 2) |
| **3** | y? | use integer scaling instead of fractional |
| **4** | y? | (used by the Microsoft implementation of the TrueType scaler) |
| **5** | y? | This bit should be set in fonts that are intended to e laid out vertically, and in which the glyphs have been drawn such that an x-coordinate of 0 corresponds to the desired vertical baseline. |
| 6 |  |Set to 0. (Just because the documentation said so.) |
| **7** | n? | This bit should be set if the font requires layout for correct linguistic rendering (e.g. Arabic fonts). |
| 8 |  | Set to 0. (This is for AAT fonts.) |
| 9 |  | Set to 0. (This bit should be set if the font contains any strong right-to-left glyphs.) |
| 10 |  | Set to 0. |
| 11 |  | Set to 0. (Defined by Adobe.) |
| 12 |  | Set to 0. (Defined by Adobe.) |
| 13 |  | Set to 0. (Defined by Adobe.) |
| 14 | n? |This bit should be set if the glyphs in the font are simply generic symbols for code point ranges, such as for a last resort font.