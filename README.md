# <h1 align = "center">Interferometry Analysis - LIP (version 4.0)</h1>
<p align="justify">
  Interferometric techniques are important tools for analysis and diagnosis in astronomy, spectroscopy, metrology, plasma physics, particle physics, and other areas, frequently applied to quantify changes in the refractive index of a material or a medium. For example, knowing the density distribution of a gas target is crucial to understanding laser plasma interactions and processes. This software was developed in Python to recover the accumulated optical phase-shift across a plasma induced by focusing laser radiation, as well as estimate the plasma density distribution.
</p>

<p align="center">
  <img src = '/Images/Figure0.PNG' width="80%" align="center">
</p>

![license](https://img.shields.io/badge/license-MIT-green)
![version](https://img.shields.io/badge/version-v.1.2-green)
![status](https://img.shields.io/badge/status-under%20development-green)

## Summary
* [Introduction](#introduction)
* [Installation](#installation)
* [How to use it](#how-to-use-it)
  * [Main Screen](#main-screen)
  * [Interferogram Images](#interferogram-images)
  * [Options](#options)
  * [LIP Profile](#lip-profile)
* [How it works](#how-it-works)
* [Example](#example)
* [Authors](#authors)
* [Acknowledgment](#acknowledgment)
* [License](#license)
* [Citation](#citation)
* [Reference](#reference)

## Introduction
The development of diagnostic tools is very important for a better understanding of laser-plasma interactions [[1]](#reference). An accurate diagnostic is crucial, as instabilities in both target and laser pulses can result in low reproducibility of processes and impair the quality of the intended interaction [[2]](#reference). Among the various non-disturbing optical methods that can be used to diagnose a gaseous target [[3-6]](#reference), interferometry is a very accurate technique capable of quantifying very small optical path differences and, therefore, suitable for measuring density variations of gases [[7, 8]](#reference) and laser-induced plasmas [[1]](#reference). The main drawback of the technique is that returns the integrated phase along the light path, requiring deconvolution methods for retrieving the target density profile. The software “Interferometry Analysis – LIP” was developed to meet the need for a new diagnostic tool to aid in the characterization of laser-induced plasmas, quickly and reliably. It was developed by our research group as part of the work to implement a laser-plasma accelerator infrastructure at the Nuclear and Energy Research Institute (IPEN), in Brazil.

## Installation
The *Interferometry Analysis - LIP* software was developed in Python 3.11. The use of this algorithm requires the installation of the following packages: [NumPy](https://numpy.org/) [[9]](#reference), [Scipy](https://scipy.org/) [[10]](#reference) and [PyAbel](https://pyabel.readthedocs.io/en/latest/index.html) [[11]](#reference) for data processing, [Pillow](https://pypi.org/project/Pillow/) [[12]](#reference) and Scikit-image [[13]](#reference) for the processing of interferogram images, [Matplotlib](https://matplotlib.org/stable/index.html) [[14]](#reference) to plot results, and [PySimpleGui](https://www.pysimplegui.org/en/latest/) to create the user's template.

Users also can create a single .exe file using the [pyinstaller](https://pyinstaller.org/en/stable/) package through the following terminal command:

<code>   pyinstaller --onefile -w IntAnalysis_LIP_v1.0.py                </code>

Users who do not use Python IDEs can utilize the software through the executable file available for download [here](https://drive.google.com/file/d/1KXjkSNreBf5OsbCz0-O0pDPYxaD-Rx6_/view?usp=sharing)

## How to use it
The *Interferometry Analysis – LIP* has a graphical user interface (GUI) to facilitate its use, and this section provide a simple review of the software functions and how to employ them.

### Main Screen
The Software Main Screen (*Fig. 1*) can be divided into 3 main parts: Interferograms, Options, and LIP Profile. Each of these parts will be detailed below.

|<img src = '/Images/Figure1.PNG'>|
|:--:| 
|*Fig.1 - Software Main Screen*|

### Interferograms
- ***1. [Interferogram (LIP)]*** interferogram frame.

  - ***[Open File(s)]*** Open interferogram(s) file(s) with the presence of a laser-induced plasma. Image file extensions should preferably be *.png* or *.snp.* (Newport proprietary format) for Newport CCD. However, all image extensions (*.gif*, *.jpg*, *.bmp*, etc) can be used. The path to the opened file is shown in the text box immediately above. If more than one file has been opened, each one is analyzed individually, and the average of all results is presented to the user.
  > **Warning**   
  >  Interferometry Analysis - LIP software only works with grayscale image files. 
  
  - ***[Rotate]*** The image rotation in degrees. Positive degrees promote counterclockwise rotation.  

  - ***[Original Size]*** Original dimensions of the image file (width, height). 
  > **Note** The interferogram shown is scaled to screen size (428,342) for users' viewing only. However, all processes to determine the plasma density profile are done with the original dimensions of the image file.

- ***2. [Interferogram (Ref.)]*** Scaled reference interferogram.

  - ***[Open File]*** Open an undisturbed interferogram file. Image file extensions should preferably be .png or .snp. However, all image extensions (*.gif*, *.jpg*, *.bmp*, etc) can be used. The path to open the file is shown in the textbox above. Unlike interferogram gas jet files, the algorithm allows the insertion of only one reference file.
  > **Warning**   
  >  Interferometry Analysis - LIP software only works with grayscale image files. 

- ***3. [Analyse Data]*** From this command button, the software will apply data processing to generate the accumulated phase-shift map, the radial phase-shift map, and the map of the electron density distribution of the plasma.

- ***4. [Clear]*** Button to clear input and output data.
> **Note:** The algorithm sets the frequency that generates a negative phase map. Because the refractive index of the plasma is less than 1. This is an intrinsic characteristic of plasmas, and it is considered in the calculations of the algorithm.

### Options
- ***5. [Select Area]*** Parameters frame for users select the interferogram area to apply the algorithm. The selected area is defined by a rectangle with edges defined by X and Y coordinates. The user can select an area using the mouse click over the image or the combo box ***[Y Coord]*** and ***[X Coord]***.
  > **Note:** The first click of the mouse defines de first value of the X and Y triangle coordinates, and the second click defines the end coordinates of the triangle. Case, the initial X (or Y) is bigger than the final X (or Y), these values will be exchanged. 
  - ***[Phase BG]*** This parameter defines the border size border used to construct the background of the accumulated phase $\Delta\phi$. The borders are defined based on a percentage of the selected area, and the background is obtained using a 4th-order 2D polynomial fitting from the selected border as shown in *Fig. 3.a*. The *Fig. 3.b* shows the accumulated phase-shift without the fitted background.
  - 
|<img src = '/Images/Figure2.png'>|
|:--:| 
| *Fig. 2. (a) Accumulated phase map of LIP in 3D with a non-linear background and the selected border (in gray) used to construct the isolated background map; (b) Accumulated phase map of LIP in 3D after removing the background.* |

- ***6. [Input Parameteres]*** Frame to set the experimental parameters used to obtain the interferogram. These parameters are:
  - ***[Scaling Factor]*** and ***[Uncertainty Scaling Factor]*** Interferogram scale in micrometers/pixel.
  - ***[Fringes Orientation]*** Definition of the interferogram fringes orientation (vertical or horizontal).
  - ***[Laser Wavelength]*** ($\lambda$) and ***[Laser FHWM]*** ($\Delta\lambda$) in nm;

- ***7. [Analysis Parameters]*** Parameters frame to analyze the interferogram.
  - ***[Filter Frequency]*** ($\nu_0$) This parameter is set automatically by the algorithm and this position defines which frequency will be used to apply the Inverse Fourier Transform and build the phase map of the plasma. This parameter is given in pixels.
  - ***[Filter Range]*** ($\Delta\nu$) frequency spread of the Gaussian frequency filter in pixel. The initial $\Delta\nu$ depends on the image dimension but can changed by the user. 
    > **Note:** The algorithm sets the frequency that generates a negative phase map. Because the refractive index of the plasma is less than 1. This is an intrinsic characteristic of plasmas, and it is considered in the calculations of the algorithm.
  - ***[Gaussian Blur]*** ($\sigma_{blur}$) Spread of the bi-dimensional Gaussian image filter. The standard deviation of the Gaussian filter ($\sigma$) defined by the user is equal for all axes. The ***[Gaussian Blur]*** is used to improve the target symmetry.  
  
  - ***[Axisymmetric Orientation]*** Definition of the axis of symmetry (or axisymmetric) to apply the Inverse Abel Transform. The axisymmetric can be horizontal or vertical.
  - ***[Axisymmetric Position]*** Axisymmetric position on the accumulated phase map to apply the Abel inversion.

### LIP Profile
- ***[Stages]:*** Stages frame allows the visualization of each result of the algorithm.
  - ***[Fourier Transform]*** This image is built from the Fourier Transform of the plasma interferogram. From this frequency map (Fig. 2.A), the software selects automatically the frequency that generates a negative phase-shift map. The selected frequency is marked with a red line over a pixel line (or column) identifying the ***[Filter Frequency]*** ($\nu_0$). If the ***[Filter Frequency]***  is equal to zero, the software will set the new valor value automatically.   
  > **Note:** The user can change this ***[Filter Frequency]*** manually.
  
  - ***[Gaussian Filter]*** This image is the Gaussian filter map applied to generate the phase map using the selected frequency (Fig. 2.B).

|<img src = '/Images/Figure3.PNG' width="40%">|
|:--:| 
| *Fig. 3. (a) 2D frequency domain obtained by the interferogram Fourier Transform with the selected frequency to be filtered; (b) Gaussian filter to be applied on the selected frequency.* |

For the next three steps, users have the option of viewing the 2D maps or 1D curves with standard deviation using the ***[Standard Deviation]*** checkbox.
 
  - ***[Acc. Phase-shift]*** Accumulated phase-shift ($\Delta\phi$) of the plasma (in rad) recovered from the interferograms.

|<img src = '/Images/Figure4.PNG' width="80%">|
|:--:| 
|*Fig. 3. Example of: (a) 2D accumulated phase-shift map and (b) 2D standard deviation map; (c) 1D accumulated phase curves and (d) standard     deviation of one curve. All phase values are given in rad.*|   
    
  - ***[Radial Phase-shift]*** Radial phase-shift ($\Delta\phi_r$) map in $rad/\mu m$ obtained after applying an Inverse Abel Transform from Accumulated Phase-shift map ($\Delta\phi$).

|<img src='/Images/Figure5.PNG'>|
|:--:| 
|*Fig. 4. Example of: (a) 2D radial phase-shift map and (b) 2D standard deviation map; (c) and (d) accuracy between 1D radial phase-shift and normalized phase-shift curves. All radial phase values are given in rad/* $\mu m$.|  

  - ***[Density Profile]*** Electron density distribution ($N_e$) of LIP in $cm^{−3}$ built from the radial phase-shift ($\Delta\phi_r$) and ***[Laser Wavelength]*** ($\lambda$).
    
|<img src='/Images/Figure6.PNG'>|
|:--:| 
|*Fig. 5. Example of: (a) 2D plasma density map and (b) 2D standard deviation map; (c) 1D plasma density curves and (d) standard deviation of one density curve. All density values are given in* $cm^{-3}$.|

- ***[1D Profile]*** This button enables 1D frame (*Fig. 6*) with options for the user to visualize the curves of each select stage for different positions on the chosen symmetry axis.
- ***[2D Profile]*** This button enables the visualization of each ***[Stage]*** in 2D images.
- ***[Save Plot]*** This button allows the user to save the visualized plot as an image file (*.png*, *.jpg*, *.bmp*, etc).
- ***[Save Data]*** This button allows the user to save the 2D array that generated the visualized plot as a *.dat* or *.txt* file.
- ***[Colormap dist.]*** With this list box the user can choose between three colormaps distributions: linear (*Fig 7.a*), quadratic (*Fig 7.a*), or cubic(*Fig 7.c*).

|<img src = '/Images/Figure7.PNG' width="100%">|
|:--:| 
|*Fig. 7. Examples with the colormaps distributions: (a) Linear distribution, (b) Quadratic distribution, (c) Cubic Distribution.* |

## How it works
A detailed description of the algorithm will be presented in a future article. However, the summarized data processing by the software algorithm is described by the flowchart shown in *Fig. 8*:

|<img src = '/Images/Figure8.PNG'>|
|:--:| 
| *Fig. 8. Scheme of the algorithm data processing.* |

In the scheme of the data processing algorithm (Fig. 8): $I$ and $I_0$ are the bi-dimensional fringes intensity distributions obtained from the gas and reference interferograms, respectively; the hats denotes the intensity Fourier transform, and $I_C^*$ is the complex conjugate of intensity with frequencies belong to $-v_0$; and, $N_e$ is the plasma electron density determined from the refractive index $n$ and laser wavelength $λ$ defined by user. The equation to calculate $N_e$, (described in detail in [[15]](#reference)) also depends on physical constants defined within the algorithm.
  
## Example
In the Example folder of this repository, the user will find two interferogram images shown in Fig. 9. These images were obtained using a Mach-Zehnder-like interferometer, as discussed in [16].
  
|<img src = '/Example/interferogram (reference).png' width='40%'> <img src = '/Example/interferogram (plasma).png' width='40%'>|
|:--:| 
|*Fig. 9. Examples of Interferogram images: reference image (on the left), and laser-induced plasma image (on the right).*|

The plasma was generated in the atmosphere (~80%) and the interferograms were generated by the second harmonic (395 nm) from ultrashort pulses of a Ti:sapphire multipass CPA system [1]. To characterize the registered plasma the user must select the area around the plasma.
All input parameters used for plasma characterization are shown in Fig. 10.

|<img src = '/Example/MainScreen_Example.png'> |
|:--:| 
|*Fig. 10. Software main screen: input parameters and the result of the example.*|

## Authors
Interferometry Analysis - Gas-Jet software was developed by researchers of the High-Power Ultrashort Pulse Lasers Group from the Center for Lasers and
Applications (CLA) from the Instituto de Pesquisas Energéticas e Nucleares ([IPEN-CNEN](https://www.ipen.br/portal_por/portal/default.php)), and of the Instituto Tecnológico de Aeronáutica ([ITA](http://www.ita.br/)).

* Jhonatha Ricardo dos Santos [![logo_ORCID](/Images/logo_ORCID.png)](https://orcid.org/0000-0001-7877-0580)
* Armando Valter Felicio Zuffi [![logo_ORCID](/Images/logo_ORCID.png)](https://orcid.org/0000-0001-5705-1499)
* Nilson Dias Vieira Junior [![logo_ORCID](/Images/logo_ORCID.png)](https://orcid.org/0000-0003-0092-9357)
* Edison Puig Maldonado [![logo_ORCID](/Images/logo_ORCID.png)](https://orcid.org/0000-0002-9462-8151)
* Ricardo Elgul Samad [![logo_ORCID](/Images/logo_ORCID.png)](https://orcid.org/0000-0001-7762-8961)

## Acknowledgment
Interferogram Analysis – LIP was developed to help with the analyze electron density of plasmas generated by ultrashort laser pulses. This software is part of the work performed by the High-Power Ultrashort Pulse Lasers Group of the CLA/IPEN. This partnership was able due to funding provided by the São Paulo Research Foundation (FAPESP).
The author Jhonatha Ricardo dos Santos also acknowledges the FAPESP for doctoral fellowship 2021/13737-8. 

## License
Interferogram Analysis -LIP is licensed under the [MIT license](/LICENSE).

Copyright (c) 2023 Jhonatha Ricardo dos Santos

## Citation
You can find the DOI for the latest version at [Zenodo].
  
## Reference
- [1] A. V. F. Zuffi, J. R. dos Santos, E. P. Maldonado, N D. Vieira, and R. E. Samad, "Femtosecond laser-plasma dynamics study by a time-resolved Mach–Zehnder-like interferometer," Appl. Opt. 62, C128-C134 (2023) DOI: 10.1364/AO.477395.
- [2] P. Sprangle, B. Hafizi, and J. R. Peñano, “Laser pulse modulation instabilities in plasma channels,” Phys. Rev. E 61, 4381–4393 (2000).DOI: 10.1103/PhysRevE.61.4381.
- [3] G. Costa, M. P. Anania, F. Bisesto, E. Chiadroni, A. Cianchi, A. Curcio,M. Ferrario, F. Filippi, A. Marocchino, F. Mira, R. Pompili, and A. Zigler,“Characterization of self-injected electron beams from LWFA experiments at SPARC_LAB,” Nucl. Instrum. Methods A 909, 118–122 (2018).DOI 10.1016/j.nima.2018.02.008.
- [4] G. S. Settles, Schlieren and shadowgraph techniques: visualizing phenomena in transparent media, in Experimental Fluid Mechanics (Springer, 2001), pp. xviii.
- [5] S. Shiraishi, C. Benedetti, A. J. Gonsalves, K. Nakamura, B. H. Shaw, T. Sokollik, J. van Tilborg, C. G. R. Geddes, C. B. Schroeder, C. Toth, E. Esarey, and W. P. Leemans, “Laser red shifting based characterization of wakefield excitation in a laser-plasma accelerator,” Phys. Plasmas 20, 063103 (2013).DOI 10.1063/1.4810802.
- [6] A. J. Goers, G. A. Hine, L. Feder, B. Miao, F. Salehi, J. K. Wahlstrand, and H. M. Milchberg, “Multi-MeV electron acceleration by Subterawatt laser pulses,” Phys. Rev. Lett. 115, 194802 (2015).DOI 10.1103/PhysRevLett.115.194802.
- [7] F. Brandi and L. A. Gizzi, “Optical diagnostics for density measurement in high-quality laser-plasma electron accelerators,” High Power Laser Sci. Eng. 7, e26 (2019).DOI 10.1017/hpl.2019.11.
- [8] A. K. Arunachalam, “Investigation of laser-plasma interactions at near-critical densities,” Dissertation (University of Jena, 2017)..
- [9] Harris, C.R., Millman, K.J., van der Walt, S.J. et al. Array programming with NumPy. Nature 585, 357–362 (2020). DOI: 10.1038/s41586-020-2649-2.
- [10] Pauli Virtanen, et. al. (2020) SciPy 1.0: Fundamental Algorithms for Scientific Computing in Python. Nature Methods, 17(3), 261-272. DOI: 10.1038/s41592-019-0686-2.
- [11] Gibson, Stephen; Hickstein, Daniel D.; Yurchak, Roman; Ryazanov, Mikhail; Das, Dhrubajyoti; Shih, Gilbert.(2022) PyAbel, PyAbel: v0.9.0, Zenodo, DOI: 10.5281/zenodo.7438595.
- [12] Clark, A. (2015). Pillow (PIL Fork) Documentation. readthedocs. Retrieved from https://buildmedia.readthedocs.org/media/pdf/pillow/latest/pillow.pdf
- [13] Stéfan van der Walt, Johannes L. Schönberger, Juan Nunez-Iglesias, François Boulogne, Joshua D. Warner, Neil Yager, Emmanuelle Gouillart, Tony Yu and the scikit-image contributors. scikit-image: Image processing in Python. PeerJ 2:e453 (2014). DOI: 10.7717/peerj.453
- [14] J. D. Hunter, Matplotlib: A 2D Graphics Environment. Computing in Science & Engineering, 9 (3), 90-95 (2007). [ DOI: 10.1109/MCSE.2007.55] (https://ieeexplore.ieee.org/document/4160265)
- [15] J. T. Verdeyen and J. B. Gerardo, “Application of laser to plasma refractive index determination,” Ann. N.Y. Acad. Sci. 122, 676–684 (1965).
- [16] A. V. F. Zuffi, J. R. dos Santos, E. P. Maldonado, N D. Vieira, and R. E. Samad, "Femtosecond laser-plasma dynamics study by a time-resolved Mach–Zehnder-like interferometer," Appl. Opt. 62, C128-C134 (2023) DOI: 10.1364/AO.477395.

