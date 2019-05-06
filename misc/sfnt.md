# sfnt Wrapper


- https://docs.microsoft.com/en-gb/typography/opentype/spec/otff#organization-of-an-opentype-font
- https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6.html#Directory
- https://developer.apple.com/library/archive/documentation/mac/Text/Text-254.html#HEADING254-0

### Header table

| Type | Name | Description |
|:-----|:-----|:------------|
| UInt32 | sfntVersion ('scaler type' in Apple spec) | [1] |
| UInt16| numTables | The number of tables in this file. |
| UInt16| searchRange | "(Maximum power of 2 <= numTables) x 16." |
| UInt16| entrySelector | "Log2(maximum power of 2 <= numTables)." |
| UInt16| rangeShift | NumTables x 16-searchRange. |


1. `0x00010000` or `0x4F54544F` ('OTTO') for OpenType. Apple's TrueType spec recognises 'true' (`0x74727565`) and `0x00010000`.