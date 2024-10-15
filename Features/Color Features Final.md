## Descriptor is a combination of colored and grayscale parts
- ##### Grayscale is a direct copy of M-LSB
	- M-LSB detector is a faster reimplemention of [LSB](LSB%20descriptor#Original%20LSB%20descriptor%20structure)
		- Instead of calculating the whole differential, they use just 9 points
- ##### Colored part is an original invention

---
## Detector
The colored feature detector is highly inspired by [[KAZE]] detector.
#### Nonlinear Diffusion Space
[As KAZE detector](KAZE#Nonlinear%20Diffusion%20Space), we also use Nonlinear Diffusion Space to build a scale pyramid.
- Firstly, let's define our color space as follows:
	${L} = L(r,g,b)$     is used for brightness (or luminance).
	$\omega = \vec{\omega}(r,g,b)$      is a 2-dimensional vector describing Hue and Saturation
##### Due to color existence we need to adjust some aspects of diffusion.
- Firstly, we redefine contrast function $g(k, |\nabla L|^2)$  to be  $g(k, |\nabla L|^2 + |\nabla \omega |^2)$
	as $\omega$ uses different space from $\nabla$, $g(k, |\nabla L|^2 + |\nabla \omega |^2) \Longleftrightarrow g(k, |\nabla L|^2 + |\nabla \omega_{x}|^2 + |\nabla \omega_{y}|^2)$
	This mapping also redefines the $A_{l}$ matrix which leads to color-based diffusion
- Secondly, we apply the diffusion equation to $L$ and $\omega$ elementwise

#### Feature detector
After Nonlinear Diffusion Space calculation we apply square average of original ${L_{Hessian}}$ and color $\sqrt{ {H}(w_{x})^2 + H(w_{y})^2 }$ which allows us to calculate final response.

#### Feature derotation
- In order to obtain rotational invariance we should estimate overall feature rotation.
Similar to original KAZE rotation estimation, we measure the biggest response vector defined as
	$$
	\left\{
	\begin{array}{ll}
V_{x} = \sum \frac{\partial L}{\partial x} \\
V_{y} = \sum \frac{\partial L}{\partial y} \\
\vec{V}_{ij} = \sum \frac{\partial \vec{\omega_{ij}}}{\partial r} \\
\end{array}
    \right.
    $$

---
## Colored part of descriptor
Firstly, let us divide the color space into two non-intersecting areas:
1. Brightness (or Luminance)
2. Hue-Saturation vector
The brightness is obvious. 
The Hue-Saturation part is described as circle with polar coords.
1. ${let} \ \ \ \theta = {Hue}$
2. ${let} \ \ \ r = {Saturation}$
Thus we could obtain the ability to calculate variance and mean of color with some meaning.
Color comparison is than done using the same p attern as grayscale one. But now we compare only _value_, not the _difference_ (similar to [[BRIEF]] approach)
##### Calculating 3-bit color representation
The color difference could be encoded as direction of line that splits pair of colors on Hue circle and difference in Saturation. This approach allows to properly divide different points and also not divide similar ones.
Color Difference encoding exploits ["binary cube" distance interpretation](Hamming%20Distance#Understanding%20the%203-bit). The vertical is set to line (0,0,0)->(1,1,1) and is used for "Saturation Difference". The middle plane is used for "Dividing Line Direction". Than the closest point is selected and used as descriptor color response.
These grant us several things:
1. If one of points is "colorless" and the line could not be selected we use top or bottom bits.
2. Points swap creates fully opposite pattern
3. Saturation swap (Hues are the same) leads to corresponding changes in pattern (0-3 bits)
4. Changes in one of colors leads to relatively small changes in direction

 **difference between colors breaks when colors are too similar**
1. This is very unlikely to happen due to detector selecting strategy (we do have either Brightness variance which leads to Saturation difference or Hue difference which could be used directly)
2. With no color difference this approach leads to response that is nearly equal to grayscale response