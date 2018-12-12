# macOS validation

How to make the font render as valid and display colour emoji characters in macOS.

#### SVGinOT


##### `glyf` table.
- Has to have a `glyf` table.
- TTX stipulates that `glyf` table must have characters that represent everything declared in `cmap`.
- All of your emoji glyphs have to *also* have monochromatic `<TTGlyph>` glyph data. If they don't, the SVG will not display.

##### use svgcleaner beforehand
Your SVG images have to be run through svgcleaner in order to be valid for SVGinOT (Affinity Designer puts in a lot of invalid things, notably styles).

##### `post` psNames and extraNames
The SVG image has to be referenced in an 'extraNames' array in the `post` table.

It also has to have an empty <psNames> section above. Dunno if it *needs* to be empty, but I had no reason to fill it so it's empty in this example.

````
<post>
	....
	<psNames>
    </psNames>
	<extraNames>
		<psName name="CR"/>
		<psName name="hot_shit"/>
		...
	</extraNames>
</post>

````

##### You need `nameRecord`s that are Mac-formatted.

More duplication~~~

##### You need a `nameRecord ID` 3 in the `name` table or it will complain.

macOS will deem the `name` table structure invalid if this isn't here.