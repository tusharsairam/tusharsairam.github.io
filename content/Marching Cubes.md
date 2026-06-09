---
title: Marching Cubes
created: 2025-09-06 20:37:14
modified: 2025-09-06 20:37:14
tags:
  - software/algorithms
aliases: []
---
A pretty interesting and simple algorithm that *renders graphics on computers by resolving shapes into triangular elements* that are connected together to form a mesh. I first came across this when I was trying to plot a 3D figure of [[@NASA - Maximum Torque and Momentum Envelopes for Reaction Wheel Arrays|Reaction Wheel envelopes]]. I could do that without using Marching Cubes because it was pretty much a simple 3D shape but for fun, I wanted to also plot it as a meshed shape. How can I show the individual mesh elements? I asked ChatGPT and it recommended the marching cubes method, which is available in the `scikit-image` package for Python[^3]

>[!tip] Origins of the marching cubes algorithm
>Developed and published in the 1987 SIGGRAPH proceedings by William Lorensen and Harvey Cline, the motivation behind marching cubes came from finding a suitable way to efficiently reconstruct CT and MRI images

It's called **marching cubes** because the algorithm "marches" across cubes. A grid of cubes is overlayed on the shape (bounded by the shape's function naturally), and then the computer marches or iterates across each of the cells. The algorithm generates [[Isosurfaces|isosurfaces]]
- [x] In the website[^1] I linked, there are 15 unique cases. I am not able to *visualize them intuitively*[^2]
	- Ben Anderson says there are 14 unique cases. There are ==8 vertices== always, and ==each vertex can only either be *inside* or *outside*==, so there are $2^8 = 256$ possibilities here. Not a lot when we're crunching numbers with computers. ==This is stored in a LookUp Table (LUT)==. In fact, ==we can reduce the number of **unique** possibilities to just 14 when we take into account vertex opposite symmetry and mirroring==. What this really means is that a + vertex on either of the cube's diametrically opposite corners would functionally be the same, it's just mirrored. Thus, the LUT is encoded with 14 possibilities, like `[e1, e2, e3]` where `eX` is the Xth edge (something like that)
- [x] Implement this in Python to build a good practical understanding of this
- [x] What are the applications of the marching cubes algorithm?
	- It's fast, accurate, and scalable to arbitrarily shaped objects. It's useful for **surface reconstruction**[^2]
	- **Weather maps** or **topographical maps** find use in the marching cubes. I am not sure they exactly use this but the application is there
- [ ] Where does it fail?

## Python Implementation
`scikit-image`'s documentation already has an example program for demo'ing marching cubes
- [x] What is a level set in the context of creating ellipsoid objects? *Not related to the marching cubes algorithm though, just a general question about level sets because this came up in the script*
	- My initial guess is that a level set denotes a "frame of reference" to tell whether a point is inside or outside the ellipsoid. Turns out I was *almost right*! A `levelset` of `False` is exactly what I thought of. Setting it to `True` gives a more continuous scalar function, where points outside the ellipsoid would have -ve values, points on the ellipsoid surface would be ~0, and points inside would have +ve values
I found interesting results while playing around with the code given in the documentation, mainly toying with the `level` option of `measure.marching_cubes()`

`level=0`
![[Marching Cubes - Double Ellipsoid level=0.png]] 
 
 `level=1`![[Marching Cubes - Double Ellipsoid level=1.png]]
When I set `level` to 1, many surfaces disappeared. My best understanding of why this is happening is because of the interplay between `levelset` of the geometry and `level` of the marching cubes algorithm. When `levelset=True`, it created a continuous function with the levels marked as scalars as I described previously: *+ve (outside)* <--- ==0 (on surface)== ---> **-ve (inside)**. Now, when `level=1`, the algorithm slices ==1 unit deeper into the geometry== from the surface. I think of it as a cut-off. Surfaces whose values on the level set are greater than 1 get removed because the cubes overlaid on them will see all of their vertices being +ve and greater than 1. Only those surfaces whose level is $\leq 1$ remain and are meshed. Increasing `level` to 2 would make more surfaces disappear, and in this particular example, `3` wouldn't work because the surface level isn't within the volume data range anymore and the code would throw an error. Interestingly, setting `level` to -1 completely annihilated everything and gave me an empty plot. I think it's because the algorithm "sliced" 1 unit out from the surface instead of into the surface. Effectively, all surfaces are out of the cubes, so there's no result
- [ ] The 3D geometry from `level=1` has actually expanded in size compared to that of `level=0`. How did that happen?

Now, if I set `levelset` to False and ignore `level`, I get a pixelated rendering of the two ellipsoids like this. If `level` is not specified, the average of the min and max of the volume is used[^4]
![[Pasted image 20250906234952.png]]

The marching cubes algorithm can be simple for rendering images but *it can fail in more complex situations though*. 
* ==When all vertices are +ve or -ve, the cube is entirely above (+) or below (-) the surface formed by intersection of the cube and the shape.== Funnily enough, I was confused about this notation before, and I thought it was with reference to the global coordinate frame and not relative to the shape

[^1]: [Matt's Webcorner - Marching Cubes](https://graphics.stanford.edu/~mdfisher/MarchingCubes.html)
[^2]: [Ben Anderson: An implementation of Marching Cubes](https://www.cs.carleton.edu/cs_comps/0405/shape/marching_cubes.html)
[^3]: [scikit-image: Marching Cubes](https://scikit-image.org/docs/0.25.x/auto_examples/edges/plot_marching_cubes.html)
[^4]: [scikit-image: measure.marching_cubes()](https://scikit-image.org/docs/0.25.x/api/skimage.measure.html#skimage.measure.marching_cubes)