---
title: Camera Matrix
---

- The *image point* $I$ is the set of coordinates of the object as projected on the camera plane
$$
I = 
\lambda
\begin{bmatrix}
	x \\
	y \\
	1
\end{bmatrix}
$$
- The camera's [[Intrinsic calibration matrix]] 
$$
IC = 
\begin{bmatrix}
	f & 0 & 0 \\
	0 & f & 0 \\
	0 & 0 & 1
\end{bmatrix}
$$
>[!Danger] Intrinsic Matrix
>In reality, the [[Intrinsic calibration matrix|intrinsic matrix will be more complicated than this]]!

- The *projection matrix* 
$$
P = 
\begin{bmatrix}
	1 & 0 & 0 & 0 \\
	0 & 1 & 0 & 0 \\
	0 & 0 & 1 & 0
\end{bmatrix}
$$
- The [[Euclidean Transformation|Euclidean transformation between the world and camera frames]] called the *extrinsic calibration matrix*, which is often an identity matrix
$$
EC = \begin{bmatrix}
	r_{11} & r_{12} & r_{13} & t_1 \\
	r_{21} & r_{22} & r_{23} & t_2 \\
	r_{31} & r_{32} & r_{33} & t_3 \\
	0 & 0 & 0 & 1
\end{bmatrix}
$$
- The *world point* $X$ a.k.a the point that is being projected

The final projection equation is 
$$
I = IC \times P \times EC \times X
$$