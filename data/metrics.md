## Measurements and jargon


### FUnit

Font design unit, or FUnit, is the arbitrary measurement system which describes where the points of a font are plotted.

The FUnit is the smallest measurable unit in the Em Square.

- [FUnits and the em square](https://docs.microsoft.com/en-us/typography/opentype/spec/ttch01#funits-and-the-em-square)
- [Converting FUnits to pixels](https://docs.microsoft.com/en-us/typography/opentype/spec/ttch01#converting-funits-to-pixels)


### Em

An em is a simply a relative unit that's equal to the currently-specified point size.

- [Em on Wikipedia](https://en.wikipedia.org/wiki/Em_(typography))


### Em Square

The space container that every character is situated within.

The contents of the Em Square are divided by the number of FUnits you're using, as declared by `unitsPerEm` in [`head`/`bhed`](../tables/header.md).

It's preferable to do it in a power of 2, it's typically 1024 or 2048 FUnits. scfbuild uses 2048.

Keep in mind that unlike the days of printing presses, the Em square is more of an idea than a constraint; glyphs can be larger than the Em square if you find it convenient to make it that way. It's your choice.

**It's unclear how the Em Square is defined in any other location than `unitsPerEm`.**

- [The Em Square on Design With FontForge](http://designwithfontforge.com/en-US/The_EM_Square.html)
- [FUnits and the Em Square](https://docs.microsoft.com/en-us/typography/opentype/spec/ttch01#funits-and-the-em-square)
- [EM Square, Ascent and Descent Values on Fontlab Forums](https://forum.fontlab.com/font-formats-and-opentype/em-square-ascent-and-descent-values/)

----

## Bounding Boxes

- `xMin` in [`head`/`bhed`](../tables/header.md)
- `yMin` in [`head`/`bhed`](../tables/header.md)
- `xMax` in [`head`/`bhed`](../tables/header.md)
- `yMax` in [`head`/`bhed`](../tables/header.md)

A bounding box that is set around all glyphs in the font. In a normal font situation, this is calculated by looking at all contour/outline data for the glyphs.

**Presumably this constitutes the maximum outer edges of the font's characters???**

In any case, ours will be representing a square.

**I'm guessing this isn't the Em Square???**


----

## Ascenders, Descenders, Caps height and x-height

- https://docs.microsoft.com/en-us/typography/opentype/spec/ttch01#funits-and-the-em-square
- http://www.myfirstfont.com/glossary.html


#### Basic ascenders and descenders

- `ascender` (Apple ascender) in [`hhea`](../tables/horizontal_metrics.md).
- `descender` (Apple descender) in [`hhea`](../tables/horizontal_metrics.md).
- `vertTypoAscender` in [`vhea`](../tables/vertical_metrics.md).
- `vertTypoDescender` in [`vhea`](../tables/vertical_metrics.md).
- `sTypoAscender ` (OS/2 ascender) in [`OS/2`](../tables/os_2.md).
- `sTypoDescender ` (OS/2 descender) in [`OS/2`](../tables/os_2.md).

These are what they are. If you're doing CJK characters (or like emoji presumably), then you will possibly want to define these in terms of the ['ideographic em-box'](https://docs.microsoft.com/en-gb/typography/opentype/spec/baselinetags#ideoembox) to ensure clean vertical rendering.

It seems like it's best to set the descender to your `yMin` and your ascender to `yMax`.


#### usWin ascenders and descenders

- `usWinAscent ` (Windows ascender) in [`OS/2`](../tables/os_2.md).
- `usWinDescent ` (Windows descender) in [`OS/2`](../tables/os_2.md).

These basically tell Windows where to vertically crop the bitmap rendering of the glyphs according to where the ascenders and descenders are. If any cropping is unacceptable, set them to greater than or equal to yMax for `usWinAscender` and (-yMin) for `usWinDescender`. It seems like you should definitely do that.

- [Information on `usWinAscent ` and it's difference to `sTypoAscender ` and `ascender `.](https://docs.microsoft.com/en-gb/typography/opentype/spec/os2#uswinascent)
- [Information on `usWinDescent` and it's difference to `sTypoDescender` and `descender`.](https://docs.microsoft.com/en-gb/typography/opentype/spec/os2#uswindescent)

#### Caps height and x-height

- `sxHeight ` in [`OS/2`](../tables/os_2.md)
- `sCapHeight ` in [`OS/2`](../tables/os_2.md)

It seems like you should try setting `sxHeight` to the same as `yMin` and `sCapHeight` to the same as `sCapHeight`.

----

## Bearings

##### Relevant data points:

###### Horizontal
- `advanceWidthMax` in [`hhea` and `hmtx`](../tables/horizontal_metrics.md).
- `minLeftSideBearing` in [`hhea` and `hmtx`](../tables/horizontal_metrics.md).
- `maxRightSideBearing` in [`hhea`](../tables/horizontal_metrics.md).
- `xMaxExtent` in [`hhea`](../tables/horizontal_metrics.md).

###### Vertical
- `advanceHeightMax` in [`vhea`](../tables/vertical_metrics.md).
- `minTopSideBearing` in [`vhea`](../tables/vertical_metrics.md).
- `maxBottomSideBearing` in [`vhea`](../tables/vertical_metrics.md).
- `yMaxExtent` in [`vhea`](../tables/vertical_metrics.md).
- `advanceHeight` in [`vmtx`](../tables/vertical_metrics.md).
- `topSideBearing` in [`vmtx`](../tables/vertical_metrics.md).
- `topSideBearing[]` in [`vmtx`](../tables/vertical_metrics.md).

###### links

- https://ilovetypography.com/typography-terms/typography-terms-s/
- http://www.myfirstfont.com/glossary.html

##### Term explanations

###### Bearings
Bearings basically tell the glyphs if they can push in or out of each others' bounding boxes.

It seems like for monospaced fonts, these should just be 0.

##### Advance width/height
Advance height and advance width are the size of the font after bearings have been added.

It seems like for monospaced fonts, these should just be the width that you've decided to set all of the glyphs to.


----

## Carets

##### Relevant data points:
- `caretSlopeRise` in [`hhea`](../tables/horizontal_metrics.md).
- `caretSlopeRun` in [`hhea`](../tables/horizontal_metrics.md).
- `caretSlopeRise` in [`vhea`](../tables/vertical_metrics.md).
- `caretSlopeRun` in [`vhea`](../tables/vertical_metrics.md).

I'm not sure what this is but for our purposes, I think it's just fine to set Rise to 1 and Run to 0.


---

## Line gap

- `lineGap` (Apple line gap) in [`hhea`](../tables/horizontal_metrics.md).
- `vertTypoLineGap ` in [`vhea`](../tables/vertical_metrics.md).
- `sTypoLineGap ` (OS/2 line gap) in [`OS/2`](../tables/os_2.md).

It seems like you should/could just set these to 0 for emoji.

---

## Underline and Strikeout

- `underlinePosition` in [`post`](../tables/post.md).
- `underlineThickness` in [`post`](../tables/post.md).
- `yStrikeoutSize` in [`OS/2`](../tables/os_2.md).
- `yStrikeoutPosition` in [`OS/2`](../tables/os_2.md).

These are things you'll have to decide for yourself based on what you think looks good for the font.

`underlineThickness` and `yStrikeoutSize` should match each other.

---

## Other

- `xAvgCharWidth` in [`OS/2`](../tables/os_2.md)
- `lowestRecPPEM` in [`head`/`bhed`](../tables/header.md)

`xAvgCharWidth` is the arithmetic average width of all non-zero width glyph in the font in FUnits. This is a monospaced font, so it's just the same width as all the other widths.

`lowestRecPPEM` is the lowest recommended size for the font, in pixels. For Mutant Standard, that would be 16 or 18px.

