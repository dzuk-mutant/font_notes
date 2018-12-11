# SVG (SVGinOT)

Stores SVG information as glyphs. Not all SVG attributes are supported (but for Mutant Standard's purposes, it's not a problem).

- [SVG table in Microsoft OpenType spec](https://docs.microsoft.com/en-gb/typography/opentype/spec/svg)

````
|- Header
|- SVGDocumentList
	|- SVGDocumentRecord
	|	|- SVGDocument
	|...

````

````
TTX basic structure

<SVG>
	<version value="0"/>
	<svgDoc>
	  <![CDATA[<?xml version='1.0' encoding='UTF-8'?>
		<svg>
		
			[ All of the juicy image stuff]
		
		</svg>
		]]>
	</svgDoc>
	<svgDoc></svgDoc>
	<svgDoc></svgDoc>
	<svgDoc></svgDoc>
	...
</SVG>

````

````
TTX beefier example

<SVG>
	<version value="0"/>
	<svgDoc startGlyphID="[ID]" endGlyphID="[ID]">
	  <![CDATA[<?xml version='1.0' encoding='UTF-8'?>
		<svg xmlns="http://www.w3.org/2000/svg" id="[ID]">
		
			[ All of the juicy image stuff]
		
		</svg>
		]]>
	</svgDoc>
	...
	...
	[more <svgDoc>s]
	...
</SVG>

````
## Header

| Type | Name | Description |
|:--|:--|:--|
| UInt16 | version | Table version. **Set to 0.** |
| Offset32 | **offsetToSVGDocumentList** | Offset to the SVG Document List from the start of this table. Must be non-zero. |
| UInt32 | reserved | **Set to 0.** |

TTX handles most of these by itself.


## SVGDocumentList

This provides a set of SVG documents. In our case, it's just one.


| Type | Name | Description |
|:--|:--|:--|
| UInt16 | **numEntries** | Number of SVGDocumentRecords. Must be non-zero |
| UInt16 | **documentRecords[numEntries]** | The array of SVG document records. |

**TTX compiles these elements automatically.**


## SVGDocumentRecord
| Type | Name | Description |
|:--|:--|:--|
| UInt16 | **startGlyphID** | The first glyph ID for the range covered by this record.[1] |
| UInt16 | **endGlyphID** | The first glyph ID for the range covered by this record.[1] |
| Offset32 | svgDocOffset | Offset from the beginning of SVGDocumentList to an SVG document. Must be non-zero.[2] |
| UInt32 | svgDocOffset | Length of the SVG data. Must be non-zero.[2] |

1. We are only doing 1:1 mappings, so these will always be the same as each other. **All SVG Documents have to be ordered from smallest to largest.**
2. Compiled automatically by fonttools from TTX. They are not entered.


## SVGDocument

[There are some special requirements for SVGs.](https://docs.microsoft.com/en-gb/typography/opentype/spec/svg#svg-documents) Not all SVG elements are allowed. This shouldn't affect Mutant Standard SVG images.
