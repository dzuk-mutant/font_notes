# `post`

There are different versions of this. We're using version 3, not just because it's the latest, but also because it means we provide `post` with the smallest amount of required information, which is the following table:

| Type | Name | Description |
|:--|:--|:--|
| Fixed | version | **Set to 0x0003000 for version 3.0. (or '3.0' in fonttools)** |
| Fixed | italicAngle | Italic angle. **Set to 0.** |
| FWord | **underlinePosition** | Suggested distance of the top of the underline from the baseline. |
| FWord | **underlineThickness** | Suggested values for the underline thickness. You should match this with Strikeout thickness [in the OS/2 table](os_2.md). |
| UInt32 | isFixedPitch | Make 0 if it's proportionally spaced. Not 0 if it's monospaced. **Set to 1.** |
| UInt32 | minMemType42 | Minimum memory usage when downloaded. **Just set to 0.** |
| UInt32 | maxMemType42 | Maximum memory usage when downloaded. **Just set to 0.** |
| UInt32 | minMemType1 | Minimum memory usage when downloaded as a Type 1 font. **Just set to 0.** |
| UInt32 | maxMemType1 | Maximum memory usage when downloaded as a Type 1 font. **Just set to 0.** |

That's it!