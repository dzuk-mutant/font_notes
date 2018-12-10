# `cmap`

- https://docs.microsoft.com/en-gb/typography/opentype/spec/cmap
- https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6cmap.html

`cmap` maps glyphs (the actual emoji pictures) to character codes (the characters people type).

The assumptions of this software is that there is a 1:1 mapping between characters/ligatures and glyphs.



## index

`cmap` begins with an index containing the table version number, plus the number of subtables that are in this table.

| Type    | Name     | Desc      |
|:--------|:---------|:----------|
| UInt16 | version   | Always set to 0. |
| UInt16  | numTables | The number of subtables that come after this. |

The subtables then come after this.

----

## subtables

cmap subtables come in a number of formats.
A bunch of them existed for use cases that are now obsolete.

Each table is expected to have each of the [platform ID information sets](../data/platform-ids.md). The platform ID information here must match the information in the `name` table.

Apple says you only really need:

- 4
- 6
- 12

We probably only need:

- 12 (because Windows needs it for high-number Unicode characters (U+10000 - U+10FFFF))
- 14 (to indicate what variation selectors we're using)

----

## Subtable Format 12 (Segmented coverage)
On Windows, this is required for really high number characters, including SPUA characters (U+10000 - U+10FFFF).


##### Apple subtable format (?)

###### The head

| Type    | Name     | Desc      |
|:--------|:---------|:----------|
| UInt16 | format   | Subtable format, set to '12'.          |
| UInt32  | length   | Byte length of this subtable, including header.          |
| UInt32  | language | Apple language code (should be 0 if this isn't an Apple format table) |
| UInt32  | nGroups  | The number of groupings to follow |

###### The groupings

| Type    | Name     | Desc      |
|:--------|:---------|:----------|
| UInt32 | startCharCode   | First character code in this group. |
| UInt32  | endCharCode   | Last character code in this group.          |
| UInt32  | startGlyphCode | Glyph index corresponding to `startCharCode`. Subsequent characters are mapped to subsequent glyphs sequentially. |


##### Microsoft subtable  format

https://docs.microsoft.com/en-gb/typography/opentype/spec/cmap#format-12-segmented-coverage

###### The head

| Type    | Name     | Desc      |
|:--------|:---------|:----------|
| UInt16 | format   | Subtable format, set to '12'. |
| UInt16  | reserved | Set to 0. |
| UInt32  | length | Byte length of this subtable, including header. |
| UInt32  | language  | Windows language code |
| UInt32  | numGroups | The number of groupings to follow |

###### The groupings

| Type    | Name     | Desc      |
|:--------|:---------|:----------|
| UInt32 | startCharCode   | First character code in this group. |
| UInt32  | endCharCode   | Last character code in this group.          |
| UInt32  | startGlyphCode | Glyph index corresponding to `startCharCode`. Subsequent characters are mapped to subsequent glyphs sequentially. |


----

## Subtable Format 14 (Variation Sequences)
This tells the computer what Unicode Variation Sequences this font supports.

A Unicode Variation Sequence (UVS) is a base character followed by a variation selector. (ie. `U+200D, U+FE0F`).

Because this is a table about other characters, this is the only subtable that cannot be on it's own.

I need to do more research on this

- https://docs.microsoft.com/en-gb/typography/opentype/spec/cmap#format-14-unicode-variation-sequences
- https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6cmap.html


---

## Mandatory/Conventional Glyphs

You should always encode these.

- https://scripts.sil.org/cms/scripts/page.php?site_id=nrsi&id=IWS-Chapter08

| Glyph ID    | Codepoint     | Desc      |
|:--------|:---------|:----------|
| 0 | ????   | .notdef / Unknown Glyph |
| 1 | ????   | Null |
| 2 | ?????   | Carriage Return |
| 3 | 0x0020   | Space |

