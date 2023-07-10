# ICE Summer School 2023

## Analysis of dust emission in astronomical sources

In this hands-on session we will learn some basics on how to deal with real astronomical data taken with ALMA. We will produce images of a region that was observed at two frequency bands, and we will derive some physical parameters relevant in the study of the dust emission such as the dust mass and the spectral index. For this, we have divided this hands-on session in two different parts: (**a**) producing images, and (**b**) analyzing images.

### Basic requirements

Astronomers use telescopes to observe the sky. However, the data taken by the telescope requires some processing before one obtains a scientific-ready image or datafile. Traditionally, each telescope developes its own software to do this processing. In this hands-on session we will use the [CASA](https://casa.nrao.edu/) software which was developed to be a common processing software for many telescope facilities, in particular, radio telescopes. Among others, CASA can be used to process the data of telescopes such as [ALMA](https://almascience.eso.org/) and [VLA](https://science.nrao.edu/facilities/vla), and other facilities (e.g., [SMA](https://lweb.cfa.harvard.edu/sma/), [NOEMA](https://iram-institute.org/science-portal/noema/), [ATCA](https://www.narrabri.atnf.csiro.au/)) have adapted their procedures to also be compatible with CASA if the user prefers to use this software.

**The first step is to download and install CASA**
For this, go to the webpage https://casa.nrao.edu/casa_obtaining.shtml and select the file that fits your Operative System (Linux or Mac). The current stable version is CASA 6.5, however, other versions can also be downloaded in case you have older operative systems (check the section **Previous CASA releases** within the previous webpage)

Downloading the corresponding CASA file may take some time. The installation should be relatively simple, requiring basically to un-compress the downloaded file and set an alias to the executable ```casa``` file.

The next requirement consists of the data taken with ALMA to be analyzed. Due to the data volume, we will distribute these files via USB stick. We will have access to two different files:

```
ALMA_1mm.ms
ALMA_3mm.ms
```

These two files have the extension ```.ms``` which refers to *measurement set* and contains the so-called visibilities observed by an interferometer. These visibilities correspond to the Fourier transform of the real sky emission of a source in the sky after being sampled and observed by ALMA.

## Producing images with CASA

Before producing the images, we will have a look at the data to have a better idea of what was observed.

### Starting CASA

You can start ```casa``` by opening a terminal and typing:

```
> casa
```

You should see a text similar to this one (if you are using the version CASA 6.2.0):

```
optional configuration file config.py not found, continuing CASA startup without it
IPython 7.15.0 -- An enhanced Interactive Python.

Using matplotlib backend: agg
Telemetry initialized. Telemetry will send anonymized usage statistics to NRAO.
You can disable telemetry by adding the following line to the config.py file in your rcdir (e.g. ~/.casa/config.py):
telemetry_enabled = False
--> CrashReporter initialized.
CASA 6.2.0.124 -- Common Astronomy Software Applications [6.2.0.124]
```

At the same time you will get an additional window ```Log Messages``` where you will constantly get information of the functions (or tasks) that you execute.

### Inspecting the data

Once ```casa``` is started, we can have a look at the two data files. For this, we will use the task **```listobs```**

```
listobs(vis = 'ALMA_1mm.ms',
    listfile = 'ALMA_1mm_listobs.txt')
```




