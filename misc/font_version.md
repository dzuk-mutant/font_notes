# Font Versioning

- [Font Development Best Practices: Versioning](https://silnrsi.github.io/FDBP/en-US/Versioning.html)


The font's version is declared in two places:

- [`head.version`](../tables/head.md)
- [`name` table record 5](../tables/name.md)

It has a bunch of weird stipulations  for maximum compatibility:

- Version should be a number with 3 decimal places at all times.
- The number should be 1 or higher.
- The name record where the version is should begin with the word 'Version', then the version number, then whatever notes the creator may have.

There are some other weird details and how to implement it. Check out the Font Development Best Practices link above to find out what those are.