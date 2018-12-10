## Measurements and jargon

- http://www.myfirstfont.com/glossary.html

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

The contents of the Em Square are divided by the number of FUnits you're using.

It's preferable to do it in a power of 2, it's typically 1024 or 2048 FUnits. scfbuild uses 2048.

- [The Em Square on Design With FontForge](http://designwithfontforge.com/en-US/The_EM_Square.html)

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

## Ascenders and Descenders

##### Relevant data points:
- `ascender` (Apple ascender) in [`hhea`](../tables/horizontal_metrics.md).
- `descender` (Apple descender) in [`hhea`](../tables/horizontal_metrics.md).
- `vertTypoAscender` in [`vhea`](../tables/vertical_metrics.md).
- `vertTypoDescender` in [`vhea`](../tables/vertical_metrics.md).
- `sTypoAscender ` (OS/2 ascender) in [`OS/2`](../tables/os_2.md).
- `sTypoDescender ` (OS/2 descender) in [`OS/2`](../tables/os_2.md).
- `usWinAscent ` (Windows ascender) in [`OS/2`](../tables/os_2.md).
- `usWinDescent ` (Windows descender) in [`OS/2`](../tables/os_2.md).

Apple, OS/2 and Windows ascenders and descenders are similar but not the same.

#### Basic ascenders and descenders

These are what they are. If you're doing CJK characters (or like emoji), then you want to define these in terms of the ['ideographic em-box'](https://docs.microsoft.com/en-gb/typography/opentype/spec/baselinetags#ideoembox) to ensure clean vertical rendering.


#### usWin ascenders and descenders

These basically tell Windows where to vertically crop the bitmap rendering of the glyphs according to ascenders. If any cropping is unacceptable, set them to greater than equal to yMax.

- [Information on `usWinAscent ` and it's difference to `sTypoAscender ` and `ascender `.](https://docs.microsoft.com/en-gb/typography/opentype/spec/os2#uswinascent)
- [Information on `usWinDescent` and it's difference to `sTypoDescender` and `descender`.](https://docs.microsoft.com/en-gb/typography/opentype/spec/os2#uswindescent)

**How do you even do ascenders and descenders for an emoji?**

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

Because this is an emoji font, they can all be set to something uniform.

**What should they be set to?**

##### Advance width/height
Advance height and advance width are the size of the font after bearings have been added.


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

??????


---

## Other

- `xAvgCharWidth` in [`OS/2`](../tables/os_2.md)
- `sxHeight ` in [`OS/2`](../tables/os_2.md)
- `sCapHeight ` in [`OS/2`](../tables/os_2.md)

??????
