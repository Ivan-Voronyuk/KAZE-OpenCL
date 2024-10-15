
### Colorspace
Generally, color is a 3-dimensional vector, so we have 3 degrees of freedom. However, this representation is not very useful in computer vision. HSV is much better for color segmentation, but it's very difficult to use cyclic color (hue) representation to calculate distance. 
- That is why we need to develop binary color distance not based on circular Hue
	==IMPORTANT!!!== The distance must be _signed_, or _ordered_ in other words.

#### Color relative positions


### Hamming distance
- Hamming distance is about "how much bits differ"
As I'm trying to develop [binary descriptor](Features/Binary%20Descriptors), I need to use this approach in order to write _hash of difference_ as I could call it.
