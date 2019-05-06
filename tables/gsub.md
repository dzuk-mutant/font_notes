# `GSUB` Glyph Substitution

**This table is only necessary if there are combination emoji characters. This can be disregarded if all the emoji being input are single codepoint.**

The `GSUB` table allows you to substitute certain patterns of characters with glyphs. This is in contrast to [`cmap`](cmap.md), where you can only map single characters to single glyphs.

We need this because there are many emoji that are formed out of combination characters. For our purposes, we only need what is called a **ligature substitution** - which replaces several glyphs one. Outside of emoji, this is something commonly associated with Arabic typesetting.


**We're gonna use version 1.0 of the GSUB table spec because 1.1 introduces new features we don't need.**

- https://docs.microsoft.com/en-gb/typography/opentype/spec/gsub

## Overall Structure

This is going to be pretty bare for the most part until we get to the meaty ligatures.

```
- Header
- ScriptList
	- The most default and generic thing possible.
- FeatureList
	- A feature that points to our ligatures
- LookupList
	- Ligatures

```

`GSUB` uses the same data structures as `GPOS` does. (Not that you really needed to know that for emoji.)

## Header

As with many things, it starts with a header~~~


| Type | Name | Description |
|:--|:--|:--|
| UInt16 | majorVersion | **Set to 1.** |
| UInt16 | minorVersion | **Set to 0.** |
| Offset16 | **scriptListOffset** | Offset to ScriptList table, from the beginning of the GSUB table. |
| Offset16 | **featureListOffset** | Offset to FeatureList table, from the beginning of the GSUB table.  
| Offset16 | **lookupListOffset** | Offset to LookupList table, from the beginning of the GSUB table.  | 

TTX doesn't have the offsets, because it calculates those during composition.

----
## ScriptList

- ScriptLists identify the scripts in a font.
- Each script the font uses is represented by a Script table.
- Each script table contains script and language system data.
- Language system tables reference features (these are defined in the FeatureList).

We need this table, but we don't need it to do any particular thing for us because we're not making fonts for any particular language or script, so we have to give ScriptList the most default-ass, generic thing possible.

- https://docs.microsoft.com/en-gb/typography/opentype/spec/chapter2#slTbl_sRec



```
TTX

<ScriptList>
	<ScriptRecord index="0">
	
		<ScriptTag value="DFLT"/>
		<Script>
		
			<DefaultLangSys>
				<ReqFeatureIndex value="65535"/>
				<FeatureIndex index="0" value="0"/>
			</DefaultLangSys>
			
		</Script>
		
	</ScriptRecord>
</ScriptList>
```


#### ScriptList itself

```
TTX

<ScriptList>
	<ScriptRecord>[...]</ScriptRecord>
	....
</ScriptList>

```

| Type | Name | Description |
|:--|:--|:--|
| UInt16 | **scriptCount** | **Set to 1.** |
| ScriptRecord | scriptRecords[] | Array of `ScriptRecord`s. Alphabetically listed by their `ScriptTag`. |

The `ScriptList` enumerates the various scripts that the font covers. In our case, this means to access the glyph substitution features that apply to any particular script.

#### ScriptRecord

```
TTX

<ScriptRecord index="0">
	<ScriptTag value="DFLT"/>
	<Script>
		....
	</Script>
</ScriptRecord>

```
| Type | Name | Description |
|:--|:--|:--|
| Tag | **scriptTag** | **Set to DFLT.** |
| Offset16 | scriptOffset | Offset to the `Script` table of this `ScriptRecord`. |

Each `ScriptRecord` contains a `ScriptTag` to identify the script, and an offset to a script table. We only need one `Script` table.

#### Script

```
TTX


<Script>
	<DefaultLangSys>[...]</DefaultLangSys>
	...
	[ other languages if we needed them ]
</Script>

```

| Type | Name | Description |
|:--|:--|:--|
| Offset16 | **defaultLangSys** | Offset to the `defaultLangSys` table. |
| UInt16 | langSysCount | **Set to 0.** (It's the number of langSysRecords for this script including the default - we won't have any other than the defualt.) |
| LangSysRecord | **langSysRecords[]** | Array of LangSysRecords. Alphabetically listed by LangSys tag. |

Script tables identify every language system for that script that have particular requirements for particular features.

Script tables begin with the `defaultLanguageSys` table, which contains the set of features that regulate the default behaviour of this script. And this is where we end the shenanigans.

#### Language System

```
TTX

<DefaultLangSys>
	<ReqFeatureIndex value="65535"/>
	<FeatureIndex index="0" value="0"/> [our empty featureIndices array]
</DefaultLangSys>

```

| Type | Name | Description |
|:--|:--|:--|
| Offset16 | lookupOrder | **Set to NULL.** |
| UInt16 | requiredFeatureIndex | **Set to 0xFFFF.** |
| UInt16 | featureIndexCount | **This will be 0.** (Number of feature index values for this language system, excluding the required one.) |
| UInt16 | featureIndices[] | **This will be empty.** (Array of indices pointing to the FeatureList. Arbitrary order.) |

This identifies features used to render the glyphs in a particular language (in the particular script it's nested within AND SO ON).

One feature index value (and only one) can be tagged as `ReqFeatureIndex`. This is important because we can tell OpenType we don't need any features by setting it to 0xFFFF (65535 in the above example). 

----



## FeatureList

`FeatureList`'s structure is very similar to `ScriptList` - List, then Record, then a Tag accompanied by the actual thing.

Unlike ScriptList, I think we  are actually going to use a feature, the feature being character composition/decomposition, and that feature will be pointing to a lookup table, that being a table of ligatures.

**I'll have to do more research to be sure about it, and also figure out what *should* be here.**

```
TTX

<FeatureList>
  <FeatureRecord index="0">
    <FeatureTag value="ccmp"/>
    <Feature>
      <LookupListIndex index="x" value="x"/>
      ....
    </Feature>
  </FeatureRecord>
</FeatureList>
```


#### FeatureList itself

| Type | Name | Description |
|:--|:--|:--|
| UInt16 | featureCount | **Set to 1.** Number of `FeatureRecord`s. |
| **FeatureRecord** | featureRecords[] | Zero-based array. Listed alphabetically by FeatureTag. |

#### FeatureRecord

| Type | Name | Description |
|:--|:--|:--|
| Tag | featureTag | **Set to ccmp.** 4-byte identification tag. |
| Offset16 | featureOffset | Offset to the `Feature` table, from the beginning of `FeatureList`. |

[ccmp is the 'Glyph Composition/Decomposition' tag, made by Microsoft](https://docs.microsoft.com/en-gb/typography/opentype/spec/features_ae#ccmp).

In short, it means 'this is gonna put two or more characters into a glyph or vice versa'.

We're going to combine this with GSUB lookup tyoe 4 later. Microsoft says that by using `ccmp` we should put the sequences made of larger number of glyphs before the ones that require fewer ones.

#### Feature

| Type | Name | Description |
|:--|:--|:--|
| Offset16 | featureParams | **Set to NULL.** |
| UInt16 | **lookupIndexCount** | Number of LookupList indicies for this feature. |
| UInt16 | **lookupListIndices[]** | Array of Indices into the LookupList. It's zero-based, so the first lookup is LookupListIndex = 0. |

Now we have a tag which declares what this kind of feature is, we need to hook this feature to a lookup table (or more!).


-------


## LookupTable

- A lookup table defines the specific conditions and the specific results of a particular substitution action.
- Each `Lookup` Table can only contain one type of information.
- Each `LookupType` can have one or more subtables. Each subtable has a definition, providing a different representation format.

Microsoft says that by using [`ccmp`](https://docs.microsoft.com/en-gb/typography/opentype/spec/features_ae#ccmp) as we did in the `FeatureList`, we should put the sequences made of larger number of glyphs *before* the ones that require fewer ones.


````
TTX

<LookupList>

  <Lookup index="0">
    <LookupType value="4"/>
    <LookupFlag value="0"/>
    <LigatureSubst index="0" Format="1">
    
		<LigatureSet glyph="u1f4aa">
			<Ligature components="u1f3fb" glyph="u1f4aa-1f3fb"/>
			<Ligature components="u1f3fc" glyph="u1f4aa-1f3fc"/>
			<Ligature components="u1f3fd" glyph="u1f4aa-1f3fd"/>
			<Ligature components="u1f3fe" glyph="u1f4aa-1f3fe"/>
			<Ligature components="u1f3ff" glyph="u1f4aa-1f3ff"/>
			<Ligature components="u101600" glyph="u1f4aa-101600"/>
			...
			<Ligature components="u101650,u101600" glyph="u1f4aa-101650-101600"/>
			<Ligature components="u101650,u101601" glyph="u1f4aa-101650-101601"/>
			<Ligature components="u101650,u101602" glyph="u1f4aa-101650-101602"/>
			...
		</LigatureSet>
		...
    </LigatureSubst>
    
  </Lookup>
  ...
  
</LookupList

````

#### LookupTable itself

````
TTX

<LookupList>
  <Lookup ....>[...]</Lookup>
</LookupList>

````

| Type | Name | Description |
|:--|:--|:--|
| UInt16 | **lookupCount** | Number of lookups!!!@ |
| Offset16 | **lookupIndexCount** | Array of offsets to `Lookup` tables, from teh beginning of `LookupList` |



#### Lookup itself

We are using [LookupType 4 Format 1](https://docs.microsoft.com/en-gb/typography/opentype/spec/gsub#LS), which means Ligature Substitution.

````
TTX

<Lookup index="0">
	<LookupType value="4"/>
	<LookupFlag value="0"/>
	    
	[Lookup subtables]
	    
</Lookup>

````

| Type | Name | Description |
|:--|:--|:--|
| UInt16 | **lookupType** | What type of Lookup subtables this Lookup has (**in our case, it's type 4**). |
| UInt16 | **lookupFlag** | **Set to 0.** |
| UInt16 | **subTableCount** | How many subtables are in this `Lookup`. |
| Offset16 | **subtableOffsets[]** | Offsets to subtables. Starting from the beginning of this table. |



#### Lookup subtables



````
TTX

<LigatureSubst index="0" Format="1">
	<LigatureSet>...</LigatureSet>
	...
</LigatureSubst>

````

| Type | Name | TTX part | Description |
|:--|:--|:--|:--|
| UInt16 | substFormat | LigatureSubst attr - Format | **Set to 1.** |
| Offset16 | **coverageOffset** | n/a | Offset to Coverage table, from beginning of this table. |
| UInt16 | **ligatureSetCount** | n/a | How many `LigatureSet` tables are in this `Lookup`. |
| Offset16 | **ligatureSetOffsets[]** | n/a | Offsets to subtables. Starting from the beginning of this table. |


#### LigatureSet table

````
TTX


<LigatureSet glyph="u1f4aa">
	<Ligature components="u1f3fb" glyph="u1f4aa-1f3fb"/>
	<Ligature components="u1f3fc" glyph="u1f4aa-1f3fc"/>
	<Ligature components="u1f3fd" glyph="u1f4aa-1f3fd"/>
	<Ligature components="u1f3fe" glyph="u1f4aa-1f3fe"/>
	<Ligature components="u1f3ff" glyph="u1f4aa-1f3ff"/>
	<Ligature components="u101600" glyph="u1f4aa-101600"/>
	... etc.
	<Ligature components="u101650,u101600" glyph="u1f4aa-101650-101600"/>
	<Ligature components="u101650,u101601" glyph="u1f4aa-101650-101601"/>
	<Ligature components="u101650,u101602" glyph="u1f4aa-101650-101602"/>
	... etc.
</LigatureSet>
... etc.

````
`LigatureSet`s contain the meat of Ligatures. They contain information that defines each ligature, and the glyph that is meant to substitute it.

Each `LigatureSet` is a grouping of one or multiple ligatures. Ligatures are grouped by the first glyph in the sequence - this is defined by the `glyph` attribute in the tag.

Within each `LigatureSet` is one or more `Ligature` tags, defining the actual ligatures:

- attr `components` - a comma-separated list listing *the parts of the Ligature other than the first glyph, in order*.
- attr `glyph` - the glyph name that this ligature corresponds to.