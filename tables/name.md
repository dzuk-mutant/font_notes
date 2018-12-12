# `name`

- https://docs.microsoft.com/en-gb/typography/opentype/spec/name
- https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6name.html

This table basically contains font metadata. Some of them are absolutely required for proper functioning.

Each piece of metadata is identifiable by a particular number.

### Encoding

Each piece of metadata needs to be attached to platform IDs. If there are multiple platform IDs, then each piece of metadata has to be repeated, each with the different platform ID information.

### Metadata

In this case what's required (shown in 'req.') may not be strictly technically required, but some OSes or applications might reject it if you don't, so you really should.

| ID | req. | name | example | notes |
|:--:|:---:|:--|:---|:---|
| 0 |   | **Copyright** | Copyright © 2017-2018 Dzuk |
| 1 |  y | **Family** | Mutant Standard emoji (SVGinOT) | [5]
| 2 |  y | **Subfamily** | Regular | [5]
| 3 |  y  | **Unique font identifier** | Mutant Standard emoji SVGinOT 0.4.0 | As far as I can tell, it's just a string that attempts to distinguish your font from others. Include version information here. macOS will consider the structure of this table invalid if this is not present. | 
| 4 |  y | **Full font name** | Mutant Standard emoji (SVGinOT) | A combination of 1 + 2, or 16 + 17.
| 5 |  y | **Version** | 0.4.0 (11th December 2018) | 
| 6 |  y | **PostScript name** | MutantStandard-SVGinOT-Regular | has to be restricted to 'printable' ASCII characters. (U+0021 through U+007E, and not '[', ']', '(', ')', '{', '}', '<', '>', '/', and '%'.)|
| 7 |   | Trademark |  |
| 8 |   | **Manufacturer Name** | Mutant Standard |
| 9 |   | **Designer Name** | Dzuk |
| 10 |   | Description | When using the special emoji within this font that aren't supported by Unicode, make sure you are not using them in situations where other people or devices may not have this font installed, or those who are visually impaired. See [URL] for more information. | 
| 11 |   | **Vendor URL** | https://mutant.tech | [3]
| 12 |   | **Designer URL** | https://noct.zone | [3]
| 13 |   | **License** | Mutant Standard emoji is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. | [4]
| 14 |   | **License URL** | https://creativecommons.org/licenses/by-nc-sa/4.0/ |  [3][4]
| 15 |   | | |
| 16 |  y | **Typographic/Preferred Family** | Mutant Standard emoji (SVGinOT) | [5]
| 17 |  y | **Typographic/Preferred Subfamily** | Complete | [5]
| 18 |   | Compatible Full |  | Something to do with Macintosh???
| 19 |   | Sample Text |  | You can't actually put emoji as sample text. Encoding standards like Apple require a restricted set of characters you put in here.
| 20 |   | PostScript CID findfont name | | ???
| 21 |   | WWS Family Name | | ???
| 22 |   | WWS Subfamily name | | ???
| 23 |   | Light Background Palette | | Something related to Windows and CPAL?
| 24 |   | Dark Background Palette | | Something related to Windows and CPAL?
| 25 |   |Variations PostScript Name Prefix | | ???

2. 6 has to be restricted to 'printable' ASCII characters. (U+0021 through U+007E, and not '[', ']', '(', ')', '{', '}', '<', '>', '/', and '%'.)
3. URLs have to contain the protocol (ie. http://, ftp://, mailto:).
4. 'License' should just have a brief summary of the license, not legalese. Use 'License URL' to direct people to the legalese.
5. 1 and 2 are similar to 16 and 17 but not identical. 1 and 2 are legacy versions and are strictly restricted to 'Regular', 'Italic', 'Bold' and 'Bold Italic'. With 16 and 17 you can put whatever you want in them. If an application supports 16 and 17, they will take precedence over 1 and 2. If it doesn't, it will just use 1 and 2.




