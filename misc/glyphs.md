## Mandatory/Conventional Glyphs

Therre are some ideas of what characters should be in a font as a baseline and what things shouldn't. There are conflicting opinions about it!

- https://scripts.sil.org/cms/scripts/page.php?site_id=nrsi&id=IWS-Chapter08
- https://silnrsi.github.io/FDBP/en-US/Glyph_Encoding_below_0020.html


| Unicode Codepoint | Name | Situation |
|:--------|:---------|:----------|
| U+0 | .notdef / null | Font Development Best Practices says no character below U+20 should be allowed because it can cause glitches, but Google and SIL think that every font should have notdef. |
| U+20 | breaking space | This is uncontroversially something every font should have. |
| U+a0 | non-breaking space | |