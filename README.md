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

Once ```casa``` is started, we can have a look at the two data files. For this, we will use the task **```listobs```**. All the tasks (or functions) in casa can be explored by typing ```inp TASKNAME```. In our current case, we can type

```
inp listobs
```

This will list all the possible parameters that can be modified before executing the task. The most important one is the ```vis``` parameter that indicates the name of the measurement set file (or visibility file) to be used. You can also specify the parameter ```listfile``` to save the output in an specific text file.

Create two text files with the observation information of the two ```.ms``` files by doing:

```
listobs(vis = 'ALMA_1mm.ms',
    listfile = 'ALMA_1mm_listobs.txt')

listobs(vis = 'ALMA_3mm.ms',
    listfile = 'ALMA_3mm_listobs.txt')
```

**Questions:**
 - What is the name of the source that has been observed? (tip: search for FieldName)
 - Is it the same source in the two files (i.e. 1mm and 3mm)?
 - What are the coordinates of the source in the sky? (tip: search for RA and Decl, i.e. Right Ascension and Declination)
 - Are the coordinates the same for the two observed files?
 - Which frequencies have been observed in the 1mm observations? And in the 3mm observations? (tip: search for RestFreq)
- How many antennas were used in each one of the observations? (tip: search for the Antennas information)
 

Following that, we can have a look at how the antennas are distributed. For this, we can use the task **```plotants```**. You can explore the parameters by typing:

```
inp plotants
```

As in the previous case, the main parameter to consider is ```vis``` where the name of the measurement set can be specified. Furthermore, the output of the image can be saved in an image file using the parameter ```figfile```. Explore the distribution of the antennas by doing:

```
plotants(vis = 'ALMA_1mm.ms',
    figfile = 'ALMA_1mm_plotants.png')

plotants(vis = 'ALMA_3mm.ms',
    figfile = 'ALMA_3mm_plotants.png')
```

If the ```figfile``` is left empy, one can have access to an interactive plot where you can zoom in and out.

Finally, we can explore the distribution of the data by using the task **```plotms```**. This task is quite powerful and provides several possibilities to plot and explore the data. We will use just some of them to have a quick look at the observed files. By typing:

```
inp plotms
```

we can see a long list of parameters to be used when plotting the data. However, we only need to specify one of them (the ```vis``` parameter) since all the others can be modified in a GUI interface available for plotms. Let's have a look at it by doing:

```
plotms(vis = 'ALMA_1mm.ms')
```

You should see the following window:


The left part of the window includes a list of tabs in which information can be modified, while the right panel shows the data to be plotted. The three main tabs that we will use are: **Data**, **Axes** and **Display**. We will now generate a series of plots to have a look at the data:

 - **Plot the distribution of the visibilities in the (u,v) domain (Fourier domain)**
   - Select in the ```Axes``` tab the following options: X-axis: U and Y-axis: V, and click Plot
   - In the ```Display``` tab, select the Colorize option and use colors based on 'baseline'
   - Q.- How are the colors (baselines) distributed in the plot?
   - Q.- Do you see an inner gap at the center of the distribution? Which is its size?
 - **Plot the flux (amplitude) as a function of the baseline**
   Select in the ```Axes``` tab the following options: X-axis: baseline and Y-axis: Amp
   Q.- Do you see some coherence in the distribution of points?
   The baselines are just numbered randomly, and one can not have an accurate view of the physical properties of the source.
   Let's try something different
   In the ```Axes``` tab, select: X-axis: UVdist and Y-axis: Amp
   Q.- Do you see some coherence in the amplitudes with respect to the uv-distance (i.e. baseline distance)?
   Remember, the Fourier transform of a point-like source has a constant amplitude; while the Fourier transform of a Gaussian source would have a Gaussian distribution of the intensities. Do you think we are observing a point-like source?






