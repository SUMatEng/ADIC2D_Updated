# ADIC2D_Updated
(This repository houses an updated version of the ADIC2D code such that the code of the original repository is consistent with that of the accompanying article. A list of changes can be found at the end of this document)

## Description:
This repository houses a modular, open-source MATLAB code to perform two dimensional Digital Image Correlation (DIC). This code forms part of the paper titled: "A 117 Line 2D Digital Image Correlation Code Written in MATLAB" which aims to serve as an educational resource to bridge the gap between the theory of DIC and its practical implementation. Furthermore, although the code is designed as an educational resource, its validation combined with its modularity makes it attractive as a starting point to develop the capabilities of DIC.

More specifically, DIC has become a popular tool in many fields to determine the displacements and deformations experienced by an object from images captured of the object. However the DIC process is complicated, being composed of several intricate elements. Although there are papers which explain DIC in its entirety while still catering to newcomers to the concept, these publications neglect to discuss how the theory presented is implemented in practice. This gap in literature makes it difficult to gain a working knowledge of DIC, which is necessary in order to contribute towards its development. The code, named ADIC2D, is a 2D, subset-based DIC framework that is predominantly consistent with current (as of the writing of the paper) state-of-the-art techniques. ADIC2D was validated showing that it performs on par with well-established DIC algorithms and thus is sufficiently reliable for practical use (refer to the paper, referenced below, for more details). 

## How to use:
ADIC2D can be used through the command line of MATLAB. However it is advised to create a run file, such as the example file provided which is called runme.m, in order to run ADIC2D. This file (runme.m) performs DIC analysis on an experimental image set (a specimen consisting of a steel plate with a hole in the centre subjected to tension loading). The images for this example file are contained within the "Resources" folder. The calibration targets (both in the world CS and the distorted sensor CS) are contained in the file CT.mat file in the "Resources" folder. The variables declared on lines XX set up the parameters of the DIC analysis (these can be change by the user as desired). 

Mask, state that user can create mask but hole masked out automatically on line XX
analysis may become unstable as user changes parameters


 runme.m, in order to run ADIC2D. This file (runme.m) runs the ADIC2D code for Samples 1, 2 or 3 of the 2D DIC challenge (version 1). The sample being analysed can be controlled by changing the value assigned to the variable "Sample" one line 4. The function input values on lines 9-18 of runme.m can be changed in order to set up the DIC analysis in the desired way. Once ADIC2D has analysed the images and returns the results (as ProcData on line 21), the displacement error metrics are computed and displayed in a tabulated format.

### Parallel processing:
The correlation aspect of ADIC2D can be run using parallel processing by changing the for loop line 18 of the function ImgCorr to a parfor loop. Note however that this does increase memory required by ADIC2D in order to run (exceeding memory limits of your PC will result in the code crashing).

### Displaying computed displacements:
ProcData stores the results in a vector format. In order to display the displacement and position information of the subsets easily a gridded format is necessary. The function AddGridFormat accepts ProcData and adds fields with gridded matrices for the purpose of displaying the results. More specifically, AddGridFormat adds the following fields:
* PD(d).POSX 	- which for image d stores the x-positions of the subsets (in the world CS) in a gridded format
* PD(d).POSY 	- which for image d stores the y-positions of the subsets (in the world CS) in a gridded format
* PD(d).UX 		- which for image d stores the x-displacements of the subsets (in the world CS) in a gridded format
* PD(d).UY 		- which for image d stores the y-displacements of the subsets (in the world CS) in a gridded format

This function is called as shown on line 30 of runme.m and the displacements can be plotted as shown on lines 31-38.

### Alternative implementation:
In the case where subsets fail to be correlated successfully the function CSTrans can crash. In such a case it is advised to use the alternative function CSTransRobust to avoid performing displacement transformation on these subsets which fail correlation. This simply requires changing CSTrans, of line 10 of function ADIC2D, from CSTrans to CSTransRobust.

### Analysing the DIC Challenge Image Sets:
An example file which runs the 2D DIC Challenge version 1 image sets of Samples 1, 2 and 3 is provided in the repository. This example file (runme_DIC_Challenge_2D.m) runs the ADIC2D code for Samples 1, 2 or 3 of the 2D DIC challenge (version 1). The sample being analysed can be controlled by changing the value assigned to the variable "Sample" one line 4. The function input values on lines 9-18 of runme_DIC_Challenge_2D.m can be changed in order to set up the DIC analysis in the desired way. Once ADIC2D has analysed the images and returns the results (as ProcData on line 21), the displacement error metrics are computed and displayed in a tabulated format.

In order to run this example file Samples 1, 2 and 3 of the DIC challenge must be downloaded from the SEM website https://www.idics.org/challenge/ (these image sets are contained in the "Previous DIC Challenge 1.0 Data" link). Thereafter they need to be unzipped and the unzipped folders of each sample need to be placed in a folder within the working directory named "DIC Challenge 1.0". (Alternatively you can save the Sample images to a location of your choice and modify the variable "ImageLocation", of line 99, to have the appropriate string for the full path to the folder containing the images)

## License:
Note that use of this code (ADIC2D and all its sub-functions) falls under the GNU GENERAL PUBLIC LICENSE Version 3. Furthermore, note that in the case of using this code for works related to publications (scientific or otherwise) requires citing of the source paper (citation given below).

## Note:
The stopping criterion for the second-order shape function, given in Equation (23) of the source paper (cited below), is incorrect (apologies if this has caused any inconvenience). The code has been updated to contain the correct form of the equation (line 20 SFExpressions).

## Citing ADIC2D:
Atkinson, D.; Becker, T. A 117 Line 2D Digital Image Correlation Code Written in MATLAB. Remote Sens. 2020, 12, 2906.
Paper can be accessed at: https://doi.org/10.3390/rs12182906

## List of changes:
* Line 10 of ImgCorr has been updated to normalise the image gradients determined by imgradientxy.