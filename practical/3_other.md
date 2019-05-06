# Other things

The GlyphOrder table must be ordered like the following:

- lowest to highest codepoints
- lowest to highest codepoint sequence length

This means the list should start with single codepoints from lowest to highest value.

If not, cmap subtables will not be recognised properly and compilation will fail.

