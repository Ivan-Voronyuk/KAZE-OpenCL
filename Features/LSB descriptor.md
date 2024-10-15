LSB descriptor is one of [binary descriptors](Features/Binary%20Descriptors)

### Original LSB descriptor structure
1. This descriptor uses much more information than just brightness comparison
	- this includes also "light gradients", which represents local areas props
2. Comparison pattern is much more predictable but also quite chaotic.
	- It seems to me that all the patterns are not symmetric due to symmetrical ones are [more likely to intersect](Features/BRIEF#^non-symmetric-reason)
3. The resulting binary vector is 3N size (quite strange but ok)

Here I do have a question: is it possible to represent the likehood with hamming distance approach?

### Comparison pattern
LSB descriptor uses multi-level comparison pattern
- Originally they used linear approach but then exponential one was introduced
	1. The highest level do have single comparison
	2. The second has 3
	3. Then 4
	 - It seems to me that they are also random. Can't understand this idea...
- Comparison is done by three parameters:
	1. Brightness
	2. x difference
	3. y difference
	- There three are then joined into three bit pattern