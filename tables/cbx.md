# CBx (CBDT/CBLC)
Stores glyphs as PNGs at multiple resolutions.

It's basically a lot like [`sbix`](sbix.md) but needlessly more complicated. This format sucks so fucking much.


- [CBDT table in Microsoft OpenType spec](https://docs.microsoft.com/en-gb/typography/opentype/spec/cbdt)



SO. This uses CBLC and CBDT to provide all of the glyph data. CBLC locates it, CBDT stores it, basically.


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
			
			<colorRef value=""/>
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


### SbitLineMetrics

god I hate how convoluted this is

```
TTX

<sbitLineMetrics direction="hori">
	<ascender value="101"/>
	<descender value="-27"/>
	<widthMax value="136"/>
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
	<ascender value="101"/>
	<descender value="-27"/>
	<widthMax value="136"/>
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


## CBDT

**Color Bitmap Data Table.** It's the bitmap data!


```
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
```

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