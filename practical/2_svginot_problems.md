# SVGinOT metrics problems

Getting SVGinOT metrics to actually work.

![](_images/2_apple.png) ![](_images/2_noto.png)![](_images/2_ms.png)

----

#### ...and I found the problem!


---

## SVGinOT renderers don't like `viewbox` attributes.

SVGinOT renderers don't like the viewbox attribute and will render it incorrectly in a variety of ways.

In macOS, this means all SVG glyphs appear 1em below where they should be. In other software environments, people have reported various other sizing and alignment errors.

This problem (and the fix that I have used) is mentioned here:

- https://github.com/eosrei/emojione-color-font/issues/50#issuecomment-236987303

### How To Fix

- remove the `viewbox` attribute
- encapsulate all of the contents of `<svg>` in a `<g>`
- give that `<g>` a transform attribute with the following:
	- `translate(0, -(Em square vertical))`
	- `scale(Em square/viewbox square)`

	
In Mutant Standard's case, the `<g>` would be `<g transform="translate(0,-2048) scale(64)">`

![](_images/2_fixed.png)


now the metrics just need to be refined by throwing in a little below-baseline positioning!