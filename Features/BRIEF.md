Brisk is the [binary descriptor](Features/Binary%20Descriptors). It is used in ORB features.

### Inner structure
- As all binary descriptors, BRIEF constructs a binary vector by comparing different parts of point neighborhood
	- I have finally found "what to do if parts are equal"!
		$$\tau(\rho, x, y)=\left\{
                \begin{array}{ll}
                  1 \ {if} \ \rho(x)<\rho(y) \\
                  0 \ {otherwise}  \\
                \end{array}
              \right. 
        $$
		^encoding-difference
        
		- As long as "feature" describes the most varying local region, equality is very unlikely to happen.
- **They use random comparison pattern** as symmetrical pattern is less robust... ^non-symmetric-reason
	- Symmetrical patterns are more likely to intersect