### `OS/2` OpenType Metrics

This is only relevant to OpenType as far as I know, but TrueType fonts can have this too, so we might as well throw it into all fonts to reduce specialised code and enhance performance/compatibility.

  - https://docs.microsoft.com/en-gb/typography/opentype/spec/os2
  - https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6OS2.html

#### The table

**All of these entries unless stated otherwise are part of the same table. I'm just breaking this down to make it easier to understand. These are still ordered how they should be.**

**This documentation is using Version 5 of this table as a reference.**

##### Initial bits

| Type | Name | Description |
|:-----|:-----|:------------|
| UInt16 | version | set to 0x0005. |
| Int16 | **xAvgCharWidth** | The arithmetic average of the width of all non-zero width glyphs in the font in font design units. It's pretty easy for us because all emoji are the same width anyway. |
| UInt16 | usWeightClass | Set to 500. |
| UInt16 | usWidthClass | Set to 5. |
| UInt16 | **fsType** | A series of bytes indicating embedding licensing rights.[ More information can be found here](https://docs.microsoft.com/en-gb/typography/opentype/spec/os2#fstype). |

##### Subscripts, Superscripts and Strikeouts

Can you really suggest subscripts and superscripts for emoji fonts?

All of these values are in font design units.

| Type | Name | Description |
|:-----|:-----|:------------|
| Int16 | **ySubscriptXSize** | X size of the font when in subscript. |
| Int16 | **ySubscriptYSize** | Y size of the font when in subscript.	|
| Int16 | **ySubscriptXOffset** | X offset of the font when in subscript. |
| Int16 | **ySubscriptYOffset** | Y offset of the font when in subscript. |
| Int16 | **ySuperscriptXSize** | X size of the font when in superscript. |
| Int16 | **ySuperscriptYSize** | Y size of the font when in superscript. |
| Int16 | **ySuperscriptXOffset** | X offset of the font when in superscript. |
| Int16 | **ySuperscriptYOffset** | Y offset of the font when in superscript. |
| Int16 | **yStrikeoutSize** | How thick the strikeout stroke will be. In font design units.  |
| Int16 | **yStrikeoutPosition** | The position of the top of the strikeout stroke relative to the baseline in font design units. Look at [Microsoft's spec](https://docs.microsoft.com/en-gb/typography/opentype/spec/os2#ystrikeoutposition) for more information. |

##### Font classifications and metrics

| Type | Name | Description |
|:-----|:-----|:------------|
| Int16 | sFamilyClass | Not important. Set to 0. |
| UInt8 | panose[10] | 10-byte series of numbers representing a [PANOSE classification number](https://monotype.github.io/panose/pan1.htm). Not important for emoji - set to 2, 0, 6, 9, 0, 0, 0, 0, 0, 0. |
| UInt32 | **ulUnicodeRange1** | A 128 bit space whereby Unicode blocks or ranges this font uses are specified. The  library could automatically set it based on what characters the user has input. Look at [Microsoft's spec](https://docs.microsoft.com/en-gb/typography/opentype/spec/os2#fsselection) for more information. |
| UInt32 | **ulUnicodeRange2** | Continuation of the above. |
| UInt32 | **ulUnicodeRange3** | Continuation of the above. |
| UInt32 | **ulUnicodeRange4** | Continuation of the above. |
| Tag | **achVendID** | ID of [a typography vendor that's been registered with Microsoft](https://docs.microsoft.com/en-gb/typography/vendors/). |
| UInt16 | fsSelection | Set all of the bytes to 0, it's irrelevant for emoji. This must agree with `macStyle` in the [header table](../head.md). Look at [Microsoft's spec](https://docs.microsoft.com/en-gb/typography/opentype/spec/os2#fsselection) for more information. |
| UInt16 | **usFirstCharIndex** | The lowest Unicode character code in this font, according to a [`cmap`](../cmap.md) subtable that has [platform ID 3 and platform-specific encoding 0 or 1](../../misc/platform_ids.md). |
| UInt16 | **usLastCharIndex** | The highest Unicode character code in this font, according to the above criteria. |
| Int16 | **sTypoAscender** | Similar to `ascender` in [`hhea`](horizontal_metrics.md) but not the same. |
| Int16 | **sTypoDescender** | Similar to `descender` in [`hhea`](horizontal_metrics.md) but not the same. |
| Int16 | **sTypoLineGap** | Similar to `lineGap` in [`hhea`](horizontal_metrics.md) but not the same.  |
| UInt16 | **usWinAscent** | "Windows Ascender". Specifies the height above the baseline for a clipping region. Similar to `sTypoAscender` above and `ascender` in [`hhea`](horizontal_metrics.md), but has important differences. [Look at Microsoft's spec for more information](https://docs.microsoft.com/en-gb/typography/opentype/spec/os2#uswinascent). |
| UInt16 | **usWinDescent** | "Windows Descender". Specifies the height below the baseline for a clipping region. Similar to `sTypoDescender` above and `descender` in [`hhea`](horizontal_metrics.md), but has important differences. [Look at Microsoft's spec for more information](https://docs.microsoft.com/en-gb/typography/opentype/spec/os2#uswindescent). |
| UInt32 | **ulCodePageRange1** | Like `ulUnicodeRange` above, but for 'Code Pages'. [Look at Microsoft's spec for more information](https://docs.microsoft.com/en-gb/typography/opentype/spec/os2#ulcodepagerange1-bits-031brulcodepagerange2-bits-3263). |
| UInt32 | **ulCodePageRange2** | Continuation of the above. |
| Int16 | **sxHeight** | The approximate distance between the baseline and the top of non-ascending lowercase letters. Measured in FUnits. Not relevant for us but we have to put something valid in it. |
| Int16 | **sCapHeight** | The approximate distance between the baseline and the top of uppercase letters. Measured in FUnits. Not relevant for us but we have to put something valid in it. |
| UInt16 | **usDefaultChar** | UTF-16 encoded codepoint of a character that can be your font's default glyph. If it's 0, glyph ID 0 will be the default character (which will be .notdef) |
| UInt16 | **usBreakChar** | Default break. Make it U+0020 (Space). |
| UInt16 | **usMaxContent** | The maximum length of a target glyph context for any feature in this font. For our purposes, that means the length of the biggest ligature. |
| UInt16 | usLowerOpticalPointSize | It's for fonts that have multiple optical styles. Not useful for us, so set to 0. |
| UInt16 | usUpperOpticalPointSize | It's for fonts that have multiple optical styles. Not useful for us, so set to 0. |


