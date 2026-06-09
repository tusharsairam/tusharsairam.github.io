---
date: 2024-12-14 19:16
title: MRI Imaging in Python
---
**T1-weighted image**[^1] - Basic pulse sequence in MRI. These are images taken with short TE and TR times
* **TE** - Echo time. The time difference between an RF pulse and the resulting echo
* **TR** - Repetition time. The time difference between consecutive RF pulses

Using the `nibabel`, `nilearn`, and `nipype` Python libraries to develop data pipelines and machine learning for neuroimaging
- Each `.nii.gz` (NiFTi format 4D image) with dimensions `[w, h, d, t]`
- `affine` is a transformation matrix converting the voxel space `i,j,k` to the scanner reference space `x, y, z`

Questions to clarify
- [?] What exactly is the affine transformation matrix here intuitively?
	- [?] How is is calculated?
	- [?] How is it practically useful?
- [?] What is the fourth graph "Volumes" of the `OrthoSlicer3D` plotting object?
- [?] What do the axis code L, A, S, I, and R stand for in an MRI image slice?
- [?] How to read the voxel size a.k.a the output of `nib.affines.voxel_sizes(affine)`? It looks like an array of 3 elements
- [?] What is `get_zooms()` from the scan header? 
- [?] What are `get_qform()` and `get_sform()` from the header?

![[Attachments/mri_image_2.png]]
- [!] The brain scan on the right isn't fully processed, that's why the first 5 slices can be removed. *I don't understand this though, the images look the same*

![[Attachments/mri_image_1.png]]
- [?] What is the mean image? It seems to have a bunch of brain scans

[[MRI - Image Smoothing]]

[^1]: https://radiopaedia.org/articles/t1-weighted-image
