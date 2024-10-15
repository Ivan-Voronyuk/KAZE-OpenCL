# Nonlinear Diffusion Space

- Nonlinear diffusion space is used to build different levels of detail
- "Determinant Of Hessian" is used to find "points of interest"
  + Determinant of hessian in math case is something like _curvature_
  -  So one could suggest that KAZE features is about _most local-varying part of image_
- Then "non-maximum suppression" is used to cancel image noise.
	>As a result you get image points with most local variance.
	
	This points are quite far from each other because we have found maximums of this variance
	If everything is Ok they are not even connected to each other
### Here do we have feature descriptor
- feature descriptor is used to identify different features
- original KAZE features have used SIFT or SURF descriptor as they were SIFT rethinking
+ however, since SIFT and SURF uses multiple levels of details, the descriptor was adopted to use ND scale space instead of the original gaussian space in SIFT

# AKAZE

- AKAZE were introduced as faster KAZE and introduces some changes
1. FED. I don't understand it. It is a faster alternative but it's also much less stable
	- so I just replaced it with FastJacobian from the same article
2. M-[LSB](LSB%20descriptor) binary descriptor. 
	- The area response is replaced by 9 dot responses that are faster to calculate