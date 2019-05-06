# CBx (CBDT/CBLC)
Stores glyphs as PNGs at multiple resolutions.

It's basically a lot like [`sbix`](sbix.md) but a lot more complicated.

This uses two tables - `CBLC` and `CBDT` to provide all of the glyph data. To simplify things - `CBLC` locates it, `CBDT` stores it.


- [CBDT table in Microsoft OpenType spec](https://docs.microsoft.com/en-gb/typography/opentype/spec/cbdt)

#### Ancestry



Most of `CBDT/CBLC`'s structures and how they work (with exceptions) are derived from the `EBDT/EBLC/EBSC` table formats (not covered in this guide). These are the grayscale parents of these tables.

#### Differences between other bitmap table formats

[See here.](../misc/bitmap_table_differences.md)

#### Weird metrics

It's important to notice that all of the metrics for CBDT/CBLC tables are either Int8 or UInt8 data types. This means they at the very least do not directly correlate to FUnits, if they do at all. (I'm not sure about the relationship of this just yet)



-----


## CBLC

**Color Bitmap Location Table.** It's the bitmap data!

```
<CBLC>
	<header version="3.0"/>
	    
	<strike index="0">
		<bitmapSizeTable>
			  
			<sbitLineMetrics direction="hori">
				[... hori metrics ...]
			</sbitLineMetrics>
			    
			<sbitLineMetrics direction="vert">
				[... vert metrics ...]
			</sbitLineMetrics>
			
			<colorRef value="0"/> <!--disused-->
			<startGlyphIndex value=""/>
			<endGlyphIndex value=""/>
			<ppemX value=""/>
			<ppemY value=""/>
			<bitDepth value=""/>
			<flags value=""/>
			
		</bitmapSizeTable>
			
		<eblc_index_sub_table_1 imageFormat="17" firstGlyphIndex="" lastGlyphIndex="">
			<glyphLoc id="" name=""/>
			
			...
			[more glyphLocs...]
			...
			
		</eblc_index_sub_table_1>
		
		...
		[... more EBLC/CBLC subtables ...]
		...
		
	</strike>
	
	...
	[... more strikes ...]
	...
	
</CBLC>
		
```

### Header

 The first and only CBLC version (at the time of writing) is 3.0.

|Type | Name | Rec. Value | Description |
|:--|:--|:--|
| Int8 | **majorVersion** | 3 | |
| Int8 | **minorVersion** | 0 | |
| UInt8 | **numSizes** | | Number of 


### BitmapSize Table

Each strike is defined by one of these tables.

|Type | Name | Description |
|:--|:--|:--|
| Offset32 | **indexSubTableArrayOffset** | | Offset: beginning of CBLC -> index subtable |
| UInt32 | **indexTablesSize** | | ??? |
| UInt32 | **numberofIndexSubTables** | | ?? |
| UInt32 | colorRef | Not used. **Set to 0.** |
| SbitLineMetrics | **hori** | [1] |
| SbitLineMetrics | **vert** | [1] |
| UInt16 | **startGlyphIndex** | First glyph index for this strike. |
| UInt16 | **endGlyphIndex** | Last glyph index for this strike. |
| UInt8 | **ppemX** | horizontal PPEM. [2] |
| UInt8 | **ppemY** | vertical PPEM. [2] |
| UInt8 | bitDepth | Either 1, 2, 4, 8 or 32. **In most circumstances it should be 32.** |
| UInt8 | flags | Either 1, 2, 4, 8 or 32. **In most circumstances it should be 32.** |

1. SbitLineMetrics tables are described in the next section.
2. [Refer to this document](../data/metrics.md) to understand what PPEM is and how to use it.


### SbitLineMetrics

This subtable dictates the line metrics for rendering for the glyphs. 

```

TTX

<sbitLineMetrics direction="hori">
	<ascender value="?"/>
	<descender value="?"/>
	<widthMax value="?>"/>
	<caretSlopeNumerator value="0"/>
	<caretSlopeDenominator value="0"/>
	<caretOffset value="0"/>
	<minOriginSB value="0"/>
	<minAdvanceSB value="0"/>
	<maxBeforeBL value="0"/>
	<minAfterBL value="0"/>
	<pad1 value="0"/>
	<pad2 value="0"/>
</sbitLineMetrics>
	
<sbitLineMetrics direction="vert">
	<ascender value="?"/>
	<descender value="?"/>
	<widthMax value="?"/>
	<caretSlopeNumerator value="0"/>
	<caretSlopeDenominator value="0"/>
	<caretOffset value="0"/>
	<minOriginSB value="0"/>
	<minAdvanceSB value="0"/>
	<maxBeforeBL value="0"/>
	<minAfterBL value="0"/>
	<pad1 value="0"/>
	<pad2 value="0"/>
</sbitLineMetrics>

```
 

|Type | Name | Rec. Value | Description |
|:--|:--|:--|
| Int8 | **ascender** | | |
| Int8 | **descender** | | |
| UInt8 | **widthMax** | | |
| Int8 | caretSlopeNumerator | 0 | Caret stuff that isn't important for emoji fonts. Leave at 0. |
| Int8 | caretSlopeDenominator | 0 | |
| Int8 | caretOffset | 0 | |
| Int8 | minOriginSB | 0 | [1] |
| Int8 | minAdvanceSB | 0 | [1] |
| Int8 | maxBeforeBL | 0 | [1] |
| Int8 | minAfterBL | 0 | [1] |
| Int8 | pad1 | 0 | |
| Int8 | pad2 | 0 | |

1. These are for scalers that might need more metric information to position glyphs or pre-allocate memory. They are available to clients that want to parse the EBLC table, but the rasteriser doesn't directly use these.  These don't seem particularly useful, especially in a modern context and Google's emoji font sets them to 0.



## CBDT

**Color Bitmap Data Table.** It's the bitmap data!


````

TTX

<CBDT>
	<header version="3.0"/>
	<strikedata index="0">
		<cbdt_bitmap_format_17 name="glyph name">
		
			<BigGlyphMetrics>
				<height value="?"/>
				<width value="?"/>
				<horiBearingX value="?"/>
				<horiBearingY value="?"/>
				<horiAdvance value="?"/>
				<horiBearingX value="?"/>
				<horiBearingY value="?"/>
				<horiAdvance value="?"/>
			</BigGlyphMetrics>
			
			<rawimagedata>
				[PMG image data]
			</rawimagedata>
			
		</cbdt_bitmap_format_17>
		
		...
		[...more cbdt_bitmap_format...]
		...
		
	</strikedata>
</CBDT>

````

### Header

Just contains the version number.

The first and only version of CBDT is 3.0.


|Type |	Name |	Description |
|:--|:--|:--|
| UInt16 | majorVersion | **Set to 3.** |
| UInt16 | minorVersion | **Set to 0.** |

### The rest

This can be in 3 possible formats, which one it is is defined by information given in the CBLC table.


#### Bitmaps

Images for each individual glyph is pure PNG data. 

Only the following chunks are allowed in the PNG - IHDR, PLTE, tRNS, sRGB, IDAT, and IEND. The others will be considered undefined.

The image data is presented in sRGB, regardless of what other chunks of the PNG might say.

The images must have the same size as expected 


#### Metrics

Metrics are defined by two different formats. We are only using `BigGlyphMetrics` because the other does not support vertical writing orientation (and we shouldn't consider that optional).

A lot of this mirrors what you'd find in the vertical and horizontal headers and metrics tables.

##### BigGlyphMetrics

|Type |	Name |	Description |
|:--|:--|:--|
| UInt8 | height | |
| UInt8 | width | |
| Int8 | horiBearingX | |
| Int8 | horiBearingY | |
| UInt8 | horiAdvance | |
| Int8 | vertBearingX | |
| Int8 | vertBearingY | |
| UInt8 | vertAdvance | |


[more information needed]