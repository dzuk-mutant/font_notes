# `name`

- https://docs.microsoft.com/en-gb/typography/opentype/spec/name
- https://developer.apple.com/fonts/TrueType-Reference-Manual/RM06/Chap6name.html

This table basically contains font metadata. Some of them are absolutely required for proper functioning.

Each piece of metadata is identifiable by a particular number.

Each piece of metadata needs to be attached to platform IDs. If there are multiple platform IDs, then each piece of metadata has to be repeated, each with the different platform ID information.

### important ones

The software should complain if any of these are missing or not entered correctly.

```
- family (1)*             "Mutant Standard emoji"
- subfamily (2)*          "Regular"
- typographic/preferred family (16)*       "Mutant Standard emoji"
- typographic/preferred subfamily (17)*    "Regular"
- full font name (4)*     "Mutant Standard emoji"
- postscript name (6)*    "MutantStandard-Regular"
```

6 has to be restricted to 'printable' ASCII characters. (U+0021 through U+007E, and not '[', ']', '(', ')', '{', '}', '<', '>', '/', and '%'.)



### less important

These aren't necessary for the function of the font, but are important nonetheless.
The software probably doesn't need to enforce these.

URLs have to contain the protocol (ie. http://, ftp://, mailto:).

```
- version (5)            "Version 0.4.0, 23rd January 2018"
- copyright (0)
- trademark (7)
- manufacturer (8)       "Mutant Standard"
- designer (9)           "Dzuk"
- description (10)
- url vendor (11)        "https://mutant.tech"
- url designer (12)      "https://noct.zone"
- license (13)           "Mutant Standard emoji is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License."
- url license (14)       "https://creativecommons.org/licenses/by-nc-sa/4.0/"
- sample text (19)       "[a bunch of cool emoji]"
```


