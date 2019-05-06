# `cmap`

- https://docs.microsoft.com/en-gb/typography/opentype/spec/cmap
- https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6cmap.html

`cmap` maps glyphs (the actual emoji pictures) to character codes (the characters people type).

cmap only deals with single glyphs. mixing glyphs to form ligatures is dealt with by other tables.

```
TTX

<cmap>
	<tableVersion version="0"/>
	<cmap_format_12 platformID="0" platEncID="10" language="0" format="12" reserved="0" length="?"  nGroups="?">
		<map code="0x0" name=".notdef"/><!-- .notdef -->
		<map code="0xd" name="CR"/><!-- Carriage Return -->
		<map code="0x20" name="space"/><!-- Space -->
		<map ... />
		<map ... />
		<map ... />
		<map ... />
		....
	</cmap_format_12>
				    	
	<cmap_format_12 platformID="3" platEncID="1" language="0x0809" format="12" reserved="0" length="?"  nGroups="?">
		<map code="0x0" name=".notdef"/><!-- .notdef -->
		<map code="0xd" name="CR"/><!-- Carriage Return -->
		<map code="0x20" name="space"/><!-- Space -->
		<map ... />
		<map ... />
		<map ... />
		<map ... />
		....
	</cmap_format_12>
		
	<cmap_format_14 platformID="0" platEncID="10" format="14" length="?" numVarSelectorRecords="?">
      <map uvs="0xfe0f" uv="[some codepoint]" name="None"/>
      <map uvs="0xfe0f" uv="[some codepoint]" name="None"/>
      <map uvs="0xfe0f" uv="[some codepoint]" name="None"/>
      ....
     </cmap_format_14>
      
   <cmap_format_14 platformID="3" platEncID="1" language="0x0809" format="14" length="?" numVarSelectorRecords="?">
      <map uvs="0xfe0f" uv="[some codepoint]" name="None"/>
      <map uvs="0xfe0f" uv="[some codepoint]" name="None"/>
      <map uvs="0xfe0f" uv="[some codepoint]" name="None"/>
      ....
     </cmap_format_14>
      
      
</cmap>

```

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

Each table is expected to have each of the [platform ID information sets](../misc/platform_ids.md). The platform ID information here must match the information in the `name` table.

Here are the useful ones:

- **0** (1-byte codepoints ceiling: U+00 - U+FF)
- **4**	(2-byte codepoints ceiling: U+00 - U+FFFF)
- **12** (4-byte codepoints ceiling: U+00 - U+10FFFF*)
- **14** (To indicate varition selectors)

(*The Unicode Standard only goes up to U+10FFFF at the time of writing)

As this list suggests, subtables 0, 4 and 12 will probably contain some of the same codepoints. That's fine, you should repeat the codepoints that fit into each subtable's possible codepoint range.

If the range of glyphs in your font don't go high enough for one of the higher subtables, don't build that subtable. (ie. if you don't have any codepoints higher than U+FFFF, don't build cmap subtables for format 12 at all.)

----

## Subtable Format 0
This covers one-byte unicode codepoints - U+00 - U+FF.

## Subtable Format 4
This covers two-byte unicode codepoints - U+00 - U+FF.


## Subtable Format 12 ('Segmented coverage')
This is required for really high number codepoints (four-byte), including SPUA codepoints (U+10000 - U+10FFFF) - U+00 - U+FF.


###### The head

| Type    | Name     | Desc      |
|:--------|:---------|:----------|
| UInt16 | format   | Subtable format, set to '12'.          |
| UInt16  | reserved | Set to 0. |
| UInt32  | length   | Byte length of this subtable, including header.          |
| UInt32  | language | Apple language code (should be 0 if this isn't an Apple format table) |
| UInt32  | nGroups  | The number of groupings to follow |

###### The groupings

| Type    | Name     | Desc      |
|:--------|:---------|:----------|
| UInt32 | startCharCode   | First character code in this group. |
| UInt32  | endCharCode   | Last character code in this group.          |
| UInt32  | startGlyphCode | Glyph index corresponding to `startCharCode`. Subsequent characters are mapped to subsequent glyphs sequentially. |




----

## Subtable Format 14 (Variation Sequences)

This tells the client what Unicode Variation Sequences this font supports.

A Unicode Variation Sequence (UVS) is a base character followed by a variation selector. (ie. `U+FE0F`).

Because this is a table about other characters, this is the only subtable that cannot be on it's own.

Because this guide is about emoji,`U+FE0F` is the only relevant variation sequence.

```
TTX

<cmap_format_14 platformID="3" platEncID="1" language="0x0809" format="14" length="?" numVarSelectorRecords="1">
      <map uvs="0xfe0f" uv="[some codepoint]" name="None"/>
      <map uvs="0xfe0f" uv="[some codepoint]" name="None"/>
      <map uvs="0xfe0f" uv="[some codepoint]" name="None"/>
      ....

```
##### The subtable itself

| Type    | Name     | Desc      |
|:--------|:---------|:----------|
| UInt16 | format | **Set to 14.** |
| UInt32  | length | Byte length of this subtable, including the header. |
| UInt32  | numVarSelectorRecords | Number of Variation Selector Records. **Set to 1 (because we're only dealing with U+FE0F).** |
| VariationSelector  | varSelector[numVarSelectorRecords] | The array containing the VariationSelector records. |

 

##### VariationSelector Record
| Type    | Name     | Desc      |
|:--------|:---------|:----------|
| UInt24 | startCharCode   | Variation Selector |
| Offset32  | defaultUVSOffset   | Offset from the start of subtable 14 to the default UVS table. May be 0. |
| Offset32  | nonDefaultUVSOffset | Glyph index corresponding to `startCharCode`. Subsequent characters are mapped to subsequent glyphs sequentially. |

[There's a bunch more data points here that speaks for themselves, really.](https://docs.microsoft.com/en-gb/typography/opentype/spec/cmap#format-14-unicode-variation-sequences)

