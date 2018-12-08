# glyph tables

This is where the meat of the font is. This is where the graphical information is stored.

How they are encoded is the basis for what we consider the format of the font is - so a font with sbix glyph tables is an sbix format font, a font with SVG glyph tables is an SVGinOT font, and so on.

Depending on the format, the visual information may be stored solely in one table (SVG, sbix), or two tables working together (CBDT/CBLC, COLR/CPAL).



#### SVGinOT
Stores SVG information as glyphs. Not all SVG attributes are supported (but for Mutant Standard's purposes, it's not a problem).

- OpenType - otf extension
- [SVG table in Microsoft OpenType spec](https://docs.microsoft.com/en-gb/typography/opentype/spec/svg)

#### sbix
Stores glyphs as raster images rendered at multiple resolutions that's picked dynamically based on DPI and font size. These images can be a variety of formats (PNG, JPG, TIFF, etc.).

You should ignore `pdf` and `mask` data types. Probably `dupe` as well.

- TrueType - ttf extension
- [sbix table in Apple TrueType Reference Manual](https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6sbix.html)

#### CBDT/CBLC
Stores glyphs as PNGs at multiple resolutions.

- TrueType - ttf extension
- [CBDT table in Microsoft OpenType spec](https://docs.microsoft.com/en-gb/typography/opentype/spec/sbix)
- [CBLC table in Microsoft OpenType spec](https://docs.microsoft.com/en-gb/typography/opentype/spec/cblc)

#### COLR/CPAL
Stores glyphs as multiple layers of vector graphics (COLR) that are given colour palettes (CPAL).

- OpenType - otf extension
- [COLR table in Microsoft OpenType spec](https://docs.microsoft.com/en-gb/typography/opentype/spec/colr)
- [CPAL table in Microsoft OpenType spec](https://docs.microsoft.com/en-gb/typography/opentype/spec/cpal)
