# Horizontal Metrics

The font's header and metrics for horizontal writing orientation. Very similar to the header and metrics for vertical writing orientation.


### `hhea` Horizontal Header

- https://docs.microsoft.com/en-gb/typography/opentype/spec/hhea
- https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6hhea.html

**This table contains generic information about the horizontal layout of the font's glyphs. It also contains some prepared information about what's in the `hmtx` table.**

A lot of this seems very unimportant because we're just interested in emoji. But we have to enter in *something*.

| Type | Name | Description |
|:-----|:-----|:------------|
| UInt16| majorVersion | Meaningless, set it to 1. Only shown in the OpenType documentation. |
| UInt16| minorVersion | Meaningless, set it to 0. Only shown in the OpenType documentation. |
| FWord | **ascender**  | Apple's typographic ascent. It's the distance the highest ascender from the baseline.[1] |
| FWord | **descender** | Apple's typographic descent. It's the distance the lowest descender from the baseline.[1] |
| FWord | **lineGap** | Apple's line gap. |
| uFWord | **advanceWidthMax** | Maximum advance width value that exists in the `hmtx` table. |
| FWord | **minLeftSideBearing** | Minimum left sidebearing value in the `hmtx` table. [2] |
| FWord | **maxRightSideBearing** | Minimum right sidebearing value in the `hmtx` table. Calculated as `Min(advanceWidth - lsb - (xMax - xMin)` [2] |
| FWord | **xMaxExtent** | 	`max(lsb + (xMax-xMin))` (whatever that means) [2] |
| Int16 | caretSlopeRise | See Apple's documentation on this. It's probably not that important what it's set to. |
| Int16 | caretSlopeRun | See Apple's documentation on this. It's probably not that important what it's set to. |
| Int16 | caretOffset | set to 0 |
| Int16 | reserved |  set to 0 |
| Int16 | reserved |  set to 0 |
| Int16 | reserved |  set to 0 |
| Int16 | reserved |  set to 0 |
| Int16 | metricDataFormat |  set to 0 |
| Int16 | **numberOfHMetrics** |  Number of hMetric entries in `hmtx` table. |

As far as I can tell, there isn't much that actually needs to be entered here, but I'll have to do more research.

1. `ascender`, `descender` and `lineGap` are Apple-specific. Windows' versions of these can be found in the `OS/2` table.
2. If your glyphs don't have contours, you don't have to compute `minRightSidebearing`, `minLeftSideBearing` or `xMaxExtent`.



----

### `hmtx` Horizontal Metrics

- https://docs.microsoft.com/en-gb/typography/opentype/spec/hmtx
- https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6hmtx.html

**This contains metric information about the horizontal layout of *each* glyph in the font.**

`hhea` contains information that is dependent on what is in this table.

##### The table structure

| Type | Name | Description |
|:-----|:-----|:------------|
| longHorMetric | **hMetrics[number of  hMetrics]** | Pairs of advance width and left side bearing values for each glyh. Records are indexed by glyph ID. |
| Int16 | **leftSideBearings[ number of glyphs - number of hMetrics]** | Left side bearings for glyph IDs greater than or equal to the number of hMetrics. |

##### A single `longHorMetric` Record:

| Type | Name | Description |
|:-----|:-----|:------------|
| UInt16 | **advanceWidth** | Advance width. In font design units (???) |
| Int16 | **lsb** | Left side bearing of the glyph. In font design units (???) |
