### Three-way comparison
- Hamming distance encodes _binary_ distance. Although, could we encode other alphabet length?

The first and most obvious test is implementing three-way comparison which has potential to be used in color comparison. We do have a lot of situations when color difference is too confusing so the ability to say "they are equal" is a must.

### Understanding the [original approach](BRIEF#^encoding-difference)

| **A-B** | -1  |  0  |  1  |     |       | **0** | **0** | **1** |
| :-----: | :-: | :-: | :-: | --- | :---: | :---: | :---: | :---: |
| **-1**  | *0* |  0  | *1* |     | **0** |  *0*  |  *0*  |  *1*  |
|  **0**  |  0  | *0* |  1  |     | **0** |  *0*  |  *0*  |  *1*  |
|  **1**  | *1* |  1  | *0* |     | **1** |  *1*  |  *1*  |  *0*  |
This allow to encode difference with single bit but we do have loss. It is very unimportant since we only use light but as we start to use colors, everything changes
#### Try to encode a trit

|        | **-1** | **0** | **1** |     |           | 00  | 10/01 | 11  |
| :----: | :----: | :---: | :---: | :-: | :-------: | :-: | :---: | :-: |
| **-1** |  *0*   |  *1*  |  *2*  |     |  **00**   |  0  |   1   |  2  |
| **0**  |  *1*   |  *0*  |  *1*  |     | **10/01** |  1  |   0   |  1  |
| **1**  |  *2*   |  *1*  |  *0*  |     |  **11**   |  2  |   1   |  0  |
As one could see, we could easily do this, but only _using an extra bit_. This is very inefficient.
What about powers-of-two? Are they more efficient?

##### Rethinking ternary distance
|        | **-1** | **0** | **1** |
| :----: | :----: | :---: | :---: |
| **-1** |  *0*   |  *2*  |  *2*  |
| **0**  |  *2*   |  *0*  |  *2*  |
| **1**  |  *2*   |  *2*  |  *0*  |
- This may be better for encoding but we loose to much as I think. So let's check this last of all variants.

### Is it possible to tightly pack trits?
- Probably we should not even try.

### Direct Encoding
- Is it possible to *directly* encode color difference into hamming distance?

This could be much more efficient if we could do this.
As I could suggest, the color and the luminance could be used quite independently. Like the color differences could be same but luminance could be different at the same time. This should not lead to complete difference between points.
Without luminance we still have three color components but only two degrees of freedom. This leads to some problems.
The difference [should have direction](Color%20Binary%20Distance#Colorspace) but you could not create order in circle.
One possible solution is to divide the circle into four sectors instead of three ones. Than one could use _five_ possible variations for color difference. 

## Why should we encode the color difference instead of the colors themselves?
- It's a big misunderstanding...

So we could just encode colors but not the difference.
Let's draw a line dividing two colors into two sections and then describe the line's direction.
Then the circle is divided into three sectors and the fourth option is "no difference".

|         | **000** | 010 | 001 | 100 | 011 | 110 | 101 | 111 |
| :-----: | :-----: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| **000** |    0    |  1  |  1  |  1  |  2  |  2  |  2  |  3  |
| **010** |    1    |  0  |  2  |  2  |  1  |  1  |  3  |  2  |
| **001** |    1    |  2  |  0  |  2  |  1  |  3  |  1  |  2  |
| **100** |    1    |  2  |  2  |  0  |  3  |  1  |  1  |  2  |
| **011** |    2    |  1  |  1  |  3  |  0  |  2  |  2  |  1  |
| **110** |    2    |  1  |  3  |  1  |  2  |  0  |  2  |  1  |
| **101** |    2    |  3  |  1  |  1  |  2  |  2  |  0  |  1  |
| **111** |    3    |  2  |  2  |  2  |  1  |  1  |  1  |  0  |

# Understanding the 3-bit
N-dimentional binary distance could be interpreted as "the shortest way to travel from one vertex of an N-dimentional cube to another".
This becomes very useful for 2D and 3D cases.
So, here you can see the 3D distance cube.

<img src="https://upload.wikimedia.org/wikipedia/commons/6/6e/Hamming_distance_3_bit_binary_example.svg" style="background-color: gray;">

- The **red** path shows 3-bit difference: (1,0,0) -> (0,1,1)
- The **blue** path shows 2-bit difference: (0,1,0) -> (1,1,1)

If the cube is rotated in such a way that (1,1,1) vertex is pointing up and (0,0,0) is pointing down, one could spot that other six vertices is forming a rectangle. This is extremely useful for color comparison.