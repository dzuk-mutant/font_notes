# Vertical Metrics

The font's header and metrics for vertical writing orientation. Very similar to the header and metrics for horizontal writing orientation.

### `vhea` Vertical Header

- https://docs.microsoft.com/en-gb/typography/opentype/spec/vhea
- https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6 vhea.html

Both of these references contain examples.

**This table contains generic information about the vertical layout of the font's glyphs. It also contains some prepared information about what's in the `vmtx` table.**

**This is also almost identical to `hhea`, but vertical and with different data types.**

The ascender, descender and linegap don't seem to be Apple-exclusive this time.**


| Type | Name | Description |
|:-----|:-----|:------------|
| Fixed | version | Meaningless, set it to 0x00010000. |
| Int16 | **vertTypoAscender**  | Distance in FUnits (???) from the centerline to the previous line's descent. |
| Int16 | **vertTypoDescender** | Distance in FUnits (???) from the centerline to the next line's ascent. |
| Int16 | **vertTypoLineGap** | Apple's line gap. |
| Int16 | **advanceHeightMax** | Maximum advance height measurement in FUnits. Must be consistent with entries in `vmtx`. |
| Int16 | **minTopSideBearing** | Minimum top sidebearing measurement in the font, in FUnits. Must be consistent with entries in `vmtx`.  |
| Int16 | **maxBottomSideBearing** | Minimum bottom sidebearing measurement in the font, in FUnits. Must be consistent with entries in `vmtx`. [2] |
| Int16 | **yMaxExtent** | 	`max(topSideBearing + (yMax-yMin))` (whatever that means) [2] |
| Int16 | caretSlopeRise | Try 1. |
| Int16 | caretSlopeRun | Try 0. |
| Int16 | caretOffset | set to 0 |
| Int16 | reserved |  set to 0 |
| Int16 | reserved |  set to 0 |
| Int16 | reserved |  set to 0 |
| Int16 | reserved |  set to 0 |
| Int16 | metricDataFormat |  set to 0 |
| UInt16 | **numOfLongVerMetrics** |  Number of hMetric entries in `hmtx` table. |


----

### `vmtx` Vertical Metrics

- https://docs.microsoft.com/en-gb/typography/opentype/spec/vmtx
- https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6 vmtx.html

**Like `hmtx`, it's basically an array of metric information for *each* glyph, just vertical. Some of the things here determine some of the values in `vhea`.**

##### The table structure

It's two arrays. There's no headers, but the entries in each array have to match the number of glyphs in the font.

| vMetrics array |
|--------|
| **Top-side bearings array** |


##### vMetrics array entry structure

| Type | Name | Description |
|:-----|:-----|:------------|
| UInt16 | **advanceHeight** | Signed integer in FUnits.  |
| Int16 | **topSideBearing**  | Signed integer in FUnits. |

1If your font is monospaced (where all characters have the same `advanceHeight`, then you only need one entry, but you have to make sure it's there.

##### Top-side bearings array entry structure

| Type | Name | Description |
|:-----|:-----|:------------|
| UInt16 | topSideBearing[] | Signed integer in FUnits.  |

This is optional and I don't think we need it.