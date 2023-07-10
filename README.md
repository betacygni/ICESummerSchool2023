# ICE Summer School 2023

## Analysis of dust emission in astronomical sources

In this hands-on session we will learn some basics on how to deal with real astronomical data taken with ALMA. We will produce images of a region that was observed at two frequency bands, and we will derive some physical parameters relevant in the study of the dust emission such as the dust mass and the spectral index. For this, we have divided this hands-on session in two different parts: (**a**) producing images, and (**b**) analyzing images.

### Basic requirements

Astronomers use telescopes to observe the sky. However, the data taken by the telescope requires some processing before one obtains a scientific-ready image or datafile. Traditionally, each telescope developes its own software to do this processing. In this hands-on session we will use the [CASA](https://casa.nrao.edu/) software which was developed to be a common processing software for many telescope facilities, in particular, radio telescopes. Among others, CASA can be used to process the data of telescopes such as [ALMA](https://almascience.eso.org/) and [VLA](https://science.nrao.edu/facilities/vla), and other facilities (e.g., [SMA](https://lweb.cfa.harvard.edu/sma/), [NOEMA](https://iram-institute.org/science-portal/noema/), [ATCA](https://www.narrabri.atnf.csiro.au/)) have adapted their procedures to also be compatible with CASA if the user prefers to use this software.

**The first step is to download and install CASA**
For this, go to the webpage: https://casa.nrao.edu/casadocs/casa-5.5.0/introduction/obtaining-and-installing
to the webpage https://casa.nrao.edu/casa_obtaining.shtml and select the file that fits your Operative System (Linux or Mac). The current stable version is CASA 6.5, however, other versions can also be downloaded in case you have older operative systems (check the section **Previous CASA releases** within the previous webpage)

Downloading the corresponding CASA file may take some time. The installation should be relatively simple, requiring basically to un-compress the downloaded file and set an alias to the executable ```casa``` file.



