# sbix

sbix (Standard Bitmap Graphics Table) stores glyphs as raster images rendered at multiple resolutions that's picked dynamically based on DPI and font size. These images can be a variety of formats (PNG, JPG, TIFF, etc.). 

For our purposes (emoji), PNG is the only adequate and necessary format.




- [sbix table in Apple TrueType Reference Manual](https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6sbix.html)


This was originally a proprietary part of TrueType designed for Apple's emoji. It's now a part of the TrueType reference proper. It has also since been incorporated (mostly) into OpenType.


sbix is functionally similar to CBDT/CBLC, but instead of putting information in two tables and being horribly, needlessly complex, sbix just does it in one.



### Compatibility

sbix has multiple versions and components, here's what they are and what formats support them, and what OSes they support.

| Thing | Format | OS | 
|:--|:--|:--|
| sbix base stuff | TrueType, OpenType | macOS 10.7+, iOS 2.2+(?), Windows 10 Creators Update (DirectWrite).  |
| `dupe` data type | TrueType, OpenType | macOS 10.9+, iOS 7+, Unclear Windows 10 support. 
| `sbixDrawOutlines` flag | TrueType, OpenType | Planned for future iOS/macOS versions. Unclear Windows 10 support. |
| `pdf` and `mask` data types | TrueType | Planned for future iOS/macOS versions. Unclear Windows 10 support.  |

Basically, we just need the base stuff, both for compatibility reasons, and just what we need to care about.

### Dependencies

- `maxp` to infer glyph count
- `hmtx` for horizontal layout
- `vmtx` for vertical layout

### Combination sbix and CBx

It seems possible to combine sbix and CBx formats into the same font, if you point their table data both to the same glyph bitmap data sources.

(CBx uses the CBLC to point to bitmap data, and the CBDT table to store it.)

This isn't possible in TTX though, because TTX automatically calculates offsets.

```
TTX


<sbix>
	<version value="1"/>
	<flags value="00000000 00000001"/> <!-- big endian -->
	
	<strike>
		<ppem value="150"/>
		<resolution value="72"/>
		
		<glyph name=".notdef"/>
		<glyph name="CR"/>
		<glyph name="space"/>
		
		<glyph graphicType="png " name="hyphen" originOffsetX="0" originOffsetY="0">
		
			<hexdata>
				[hexadecimal image data in sets of 8]
			</hexdata>
			
		</glyph>
		  
		...
		[more glyphs, both empty and non-empty]
		...
		
	</strike>
	
	...
	[more strikes if you want them]
	...
	
</sbix>
	  
```

----

## Header


|Type |	Name |	Description |
|:--|:--|:--|
| UInt16 | version | Table version number. **Set to 1.** |
| UInt16 | flags | **Set to 1000000 00000000.** |
| UInt32 | **numStrikes** | Number of bitmap strikes. |
| Offset32 | **strikeOffsets[numStrikes]** | Offsets for the bitmap strikes. Starts from the beginning of the `sbix` table. |


Note on flags - the first bit is always set to 1. the second bit is `sbixDrawOutlines` flag - whether you want `glyf` outline data to be drawn on top of the bitmap (in our case, that would never be desirable, so it's a no). The rest are reserved and should be set to 0.

----

## Strikes

Strikes consist of two parts:

### Strike Header
|Type |	Name |	Description |
|:--|:--|:--|
| UInt16 | **ppem** | The PPEM size this strike was designed for. |
| UInt16 | **ppi** | The PPI this strike was designed for. |
| Offset32 | **glyphDataOffsets[numGlyphs+1]** | Offset for the bitmap data for an individual glyph ID. Starts from the beginning of this strike header. |

[more information????]

### Glyph Data

Consists of another header, then immediately followed by the actual data.

|Type |	Name |	Description |
|:--|:--|:--|
| Int16 | originOffsetX | The horizontal offset from the left edge of the graphic to the glyph's origin. |
| Int16 | originOffsetY | The vertical offset from the left edge of the graphic to the glyph's origin. |
| Tag | graphicType | The format of the embedded data - 'jpg', 'png' (ours), 'tiff' or 'dupe' (special) |
| UInt8 | **data[]** | The embedded graphic data. The length is inferred from sequential entries in `glyphDataOffsets[]` and the fixed size of the header. |

[more information????]