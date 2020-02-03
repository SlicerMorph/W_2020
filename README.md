# Winter 2020 3D Morphometrics Workshop at FHL 
Lectures, labs and all documents associated with 3D Morphometrics workshop at FHL (Winter 2020)
<img alt="SlicerMorph logo" width="358" height="256" src="https://github.com/SlicerMorph/SlicerMorph.github.io/blob/master/SlicerMorph_Logos/SlicerMorph_Final_Logos-V2.jpg">

## Computer and software instructions for the 3D Morphometrics Workshop
**Computer specs:** Please bring a laptop with these suggested specs:
*	A quad-core CPU from last couple years (i7 or i5 is preferred). 
*	1920x1080 or higher screen resolution.
*	8GB or higher RAM (memory)
*	100-200GB available storage space (for software and data). Please note that solid state drives (SSD) both SATA and non-volatile memory express (NVMe) are preferred over spinning hard-drive disks (HDD) due their high sequential read/write performance (usually 10X or more faster than HDDs). 
*	 A discrete (not an integrated one) GPU with minimum of 2GB of RAM with the latest GPU driver installed.  
*	A three-buttoned mouse. 

Your laptop should be running windows 10 (windows 7 has issues and is not supported anymore), or Mac OS 10.11 (El Capitan) or later.  A recent version of a common Linux distribution (like Ubuntu or CentOS) is also fine.
Please note that segmentation is a memory intensive operation. It is suggested, you have 6-10X more memory than your full dataset size (i.e., if you are working on a 1024x1024x1024 dataset, you will need about 10GB RAM to work on in in Slicer). You can always reduce your dataset to match your hardware capacity.

**Required software:** You should have these software install these on your laptops before coming to workshop.
1.	We will use a customized version of 3D Slicer with all the tools we need pre-loaded. Please download it for your OS from http://download.slicermorph.org and follow the instructions in the pdf file. 
2.	Download and install git from https://git-scm.com/downloads 
3.	Download and install R 3.6.2 from https://cran.r-project.org/
4.	Download and install Rstudio Desktop from https://www.rstudio.com/products/rstudio/download/
5.	Docker Desktop (for Mac and Windows) https://www.docker.com/products/docker-desktop 

**Additional software:** We will not use them for the workshop specifically, but you might find them useful for specific tasks:
1.	Drishti (mac and windows only); https://github.com/nci/drishti/releases
2.	Convert3d (command line tools for image conversion) https://sourceforge.net/projects/c3d/
3.	Dcm2niix (DICOM to nifti conversion) https://www.nitrc.org/plugins/mwiki/index.php/dcm2nii:MainPage
4.	Fiji https://fiji.sc/

**Accounts:** Please create accounts on these websites prior to workshop
*	MorphoSource: https://morphosource.org
*	GitHub https://github.com
*	Slicer Forum, https://discourse.slicer.org (alternatively you can use your github or google accounts to signup)
*	Please sign up for the SlicerMorph announcement to keep up-to-date with SlicerMorph project and extension updates http://mailman11.u.washington.edu/mailman/listinfo/slicermorph-announcements

**Important Websites:**

*	SlicerMorph project website: https://SlicerMorph.github.io
*	Summer Workshop lectures and labs https://github.com/SlicerMorph/W_2020
* Link to the file dropbox to upload your lightning talks (or any other data) https://faculty.washington.edu/maga/data_dropbox/

#### Funding acknowledgement
Development of SlicerMorph and the intense workshops are generously funded by National Science Foundation Advances in Bioinformatics collobrative research grants to Murat Maga (ABI-1759883), Adam Summers (ABI-1759637) and Doug Boyer (ABI-1759839). 
