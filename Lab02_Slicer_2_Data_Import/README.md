# Lab 02 Slicer#2: Data formats, importing data from MorphoSource, saving
* **image stacks vs volume files** We prefer to import the image sequences and save them as 3D volume to the convenience of working with single files. More importantly, except for DICOM format, most 2D image format are not well-suited to preserve the geometry and resolution information of volumetric scans. In this lab we will review how to import a non-DICOM stack, check and correct its voxel spacing, and then make sure that it is correctly converted to a scalar volume (as oppose to multichannel, vector volumes). 

Two well-documented, and commonly supported image volume formats are:
* [Nearly Raw Raster Data: NRRD](http://teem.sourceforge.net/nrrd/format.html)
* [Neuroimaging Informatics Technology Initiative: Nifti](https://nifti.nimh.nih.gov/)

* [List of image formats supported by Slicer](https://www.slicer.org/wiki/Documentation/Nightly/SlicerApplication/SupportedDataFormat)
* [pixel coordinates vs physical coordinates](https://www.slicer.org/wiki/Coordinate_systems)

## Image Import Examples
