# Platform IDs

- https://docs.microsoft.com/en-gb/typography/opentype/spec/name#platform-specific-encoding-and-language-ids-windows-platform-platform-id-3

A series of small data structures that get repeated across particular areas of the font tables. They imbue certain subtables or data entries with metadata and/or special properties. They're kinda not useful for what we're doing.

This is a weird thing that basically defines certain encoding functionalities and how the font gets interpreted by particular operating systems. We might be able to just set the unicode one and call it a day, but I'm not 100% sure.

### The bits

Here are the different ones as well as the recommended/default values.

##### Unicode (Platform ID 0)
- Language ID 0 (there is no language ID, but we're setting it anyway)
- Platform Specific ID 0 (for Default semantics)


##### Macintosh (Platform ID 1)
The use of this is discouraged by Apple. So we're not using it.

##### Microsoft (Platform ID 3)
- Language ID 0x0809 (for UK English) < needs to be variable, hexadecimal
- Platform Specific ID 1 (for Unicode BMP)

### They need to be consistent across the font

Whatever data you choose here has to be the same in both of them or Windows will not load the font.

### You have to duplicate the information they are attached to if there are multiple instances

This information exists in both [`cmap`](../tables/cmap.md) subtables and in [`name`](../tables/cmap.md) data entries and can only exist once in each of them.

This means if you have multiple platform ID bits, you have to duplicate those `cmap` subtables and `name` data entries.