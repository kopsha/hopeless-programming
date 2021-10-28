## Welcome to hopeless programming

Bits and bytes of code from all hopeless programmers I've met.

Nobody cares for actual good (or even best) practices, so here you can enjoy the stinkers I had work on.


### Terrible idea one

Hopefully this will go well here

```typescript
let i = iStart;
let j = jStart;
while (i < nColumns && j < nRanges && i >= 0 && j >= 0) {
	let plot = this.plot(i, j);
	if (plot) {
		this.sorted.push(plot);
	}

	if (walkOrder === 'left_right') {
		i += iDir;
		if (i >= nColumns || i < 0) {
			i = iStart;
			j += jDir;
		}
	} else if (walkOrder === 'top_down') {
		j += jDir;
		if (j >= nRanges || j < 0) {
			j = jStart;
			i += iDir;
		}
	} else if (walkOrder === 'zig_zag_horizontal') {
		if (j % 2 === jStart % 2) {
			i += iDir;
			if (i >= nColumns || i < 0) {
				i = iEnd;
				j += jDir;
			}
		} else {
			i -= iDir;
			if (i < 0 || i >= nColumns) {
				i = iStart;
				j += jDir;
			}
		}
	} else if (walkOrder === 'zig_zag_vertical') {
		if (i % 2 === iStart % 2) {
			j += jDir;
			if (j >= nRanges || j < 0) {
				j = jEnd;
				i += iDir;
			}
		} else {
			j -= jDir;
			if (j < 0 || j >= nRanges) {
				j = jStart;
				i += iDir;
			}
		}
	} else if (walkOrder === '2rows_horizontal') {
		if (j % 4 === jStart % 4) {
			j += jDir;
			if (j >= nRanges || j < 0) {
				j = jEnd;
				i += iDir;
			}
		} else if (j % 4 === (jStart + jDir) % 4) {
			i += iDir;
			if (i >= nColumns || i < 0) {
				i = iEnd;
				j += jDir;
			} else {
				j -= jDir;
			}
		} else if (j % 4 === (jStart + 2 * jDir) % 4) {
			j += jDir;
			if (j >= nRanges || j < 0) {
				j = jEnd;
				i -= iDir;
			}
		} else {
			i -= iDir;
			if (i >= nColumns || i < 0) {
				i = iStart;
				j += jDir;
			} else {
				j -= jDir;
			}
		}
	}
}
```


### Support or Contact

If you reconize your little stinkers, please write send me a short message [here](http://dev/null) and you might restore my respect to you.
