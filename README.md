# ICE Summer School 2023

## Analysis of dust emission in astronomical sources

In this hands-on session we will learn some basics on how to deal with real astronomical data taken with ALMA. We will produce images of a region that was observed at two frequency bands, and we will derive some physical parameters relevant in the study of the dust such as the dust mass or the spectral index. For this, we have divided this hands-on session in two major parts: (**a**) producing images, and (**b**) analyzing images.

In the following, you will find these sections:

  - [**Basic requirements**](https://github.com/betacygni/ICESummerSchool2023#basic-requirements)
  - [**Producing images with CASA**](https://github.com/betacygni/ICESummerSchool2023#producing-images-with-casa)
  - [**Analyzing images of dust emission**](https://github.com/betacygni/ICESummerSchool2023#analyzing-images-of-dust-emission)
  - [**For experts with CASA and ALMA**](https://github.com/betacygni/ICESummerSchool2023#for-experts-with-casa-and-alma)

[**Starting CASA**](https://github.com/betacygni/ICESummerSchool2023#starting-casa)
  
## Basic requirements

Astronomers use telescopes to observe the sky. However, the data taken by the telescope requires some processing before one obtains a scientific-ready image or datafile. Traditionally, each telescope developes its own software to do this processing. In this hands-on session we will use the [CASA](https://casa.nrao.edu/) software which was developed to be a common processing software for many telescope facilities, in particular, radio telescopes. Among others, CASA can be used to process the data of telescopes such as [ALMA](https://almascience.eso.org/) and [VLA](https://science.nrao.edu/facilities/vla), while other facilities (e.g., [SMA](https://lweb.cfa.harvard.edu/sma/), [NOEMA](https://iram-institute.org/science-portal/noema/), [ATCA](https://www.narrabri.atnf.csiro.au/)) have adapted their procedures to also be compatible with CASA if the user prefers to use this software.

**The first step is to download and install CASA**.
For this, go to the webpage https://casa.nrao.edu/casa_obtaining.shtml and select the file available for your Operative System (Linux or Mac). The current stable version is CASA 6.5, however, other versions can also be downloaded in case you have older operative systems (check the section **Previous CASA releases** within the previous webpage).

Downloading the corresponding CASA file may take some time. The installation should be relatively simple, requiring basically to execute the ```.dmg``` file within a MacOS system or to un-tar the downloaded file within a Linux system. After that, one can define an alias in their ```.bashrc``` or ```.bashr_profile``` files to make the executable ```casa``` available from anywhere.

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

You should see a text similar to this one:

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

![Figure 01](https://github.com/betacygni/ICESummerSchool2023/blob/main/ICESummerSchool2023_01.png)

The left part of the window includes a list of tabs in which information can be modified, while the right panel shows the data to be plotted. The three main tabs that we will use are: **Data**, **Axes** and **Display**. We will now generate a series of plots to have a look at the data:

 - **Plot the distribution of the visibilities in the (u,v) domain (Fourier domain)**
   - Select in the ```Axes``` tab the following options: X-axis: U and Y-axis: V, and click Plot
   - In the ```Display``` tab, select the Colorize option and use colors based on 'baseline'
   - Q.- How are the colors (baselines) distributed in the plot?
   - Q.- Do you see an inner gap at the center of the distribution? Which is its size?
 - **Plot the flux (amplitude) as a function of the baseline**
   - Select in the ```Axes``` tab the following options: X-axis: baseline and Y-axis: Amp
   - Q.- Do you see some coherence in the distribution of points?
   - Q.- There seems to be very bright baselines? Could it be that they are errors and should be excluded?
   - The baselines are just numbered randomly, and one can not have an accurate view of the physical properties of the source.
   - Let's try something different, in the ```Axes``` tab, select: X-axis: UVdist and Y-axis: Amp
   - Q.- Do you see some coherence in the amplitudes with respect to the uv-distance (i.e. baseline distance)?
   - Remember, the Fourier transform of a point-like source has a constant amplitude; while the Fourier transform of a Gaussian source would have a Gaussian distribution of the intensities. Do you think we are observing a point-like source?
 - **Plot the flux (amplitude) as a function of time (observing time)**
   - Select in the ```Axes``` tab the following options: X-axis: time and Y-axis: Amp
   - Q. Is the time that appears in the plot coincident with the information of the listobs file?
 - **Plot the flux (amplitude) as a function of frequency**
   - Select in the ```Axes``` tab the following options: X-axis: frequency and Y-axis: Amp
   - Q. Do the frequencies match with those in the listobs file?

You can explore other plots by selecting different variables or colorizing based on different parameters.

### Creating images

The images can be produced from the visibilities through a process that is traditionally called "CLEANing". The main goal of this process is to remove the instrumental effect of the sparse sampling of the interferometer to try to create a scientific-ready image. This process requires a series of steps that are repeated iteratively until we reach the total number of iterations or a certain threshold determined by the user. Without entering into details, we can directly say that the task (or function) **```tclean```** takes care of this iterative process and CLEANs the data to produce a final image. By typing:

```
inp tclean
```

we obtain a long list of parameters that can be used to improve different aspects of the imaging process. ```tclean``` (or any similar function) is indeed a key function in all softwares dealing with radio-interferometric data. We will use just a limited number of parameters in this hands-on session. These are:
 - ```vis``` : name of the ms file to be processed
 - ```datacolumn``` : to indicate that we want to use the ```data``` column which is the only one available in the files
 - ```imagename``` : name of the output files that will be produced (without extension)
 - ```imsize``` : size in pixels of the image to be produced (in our case 512)
 - ```cell``` : size of each pixel in arcseconds (in our case 0.02 arcsec)
 - ```niter``` : number of iterations to be used
 - ```interactive``` : set to False to speed up the process

We will produce three different images by changing the number of iterations and explore them. For this, we can use:

**Produce a dirty image**:
```
tclean(vis = 'ALMA_1mm.ms',
    datacolumn = 'data',
    imagename = 'ALMA_1mm_continuum_dirty',
    imsize = 512,
    cell = '0.02arcsec',
    niter = 0,
    interactive = False)
```

This will produce a series of files with the same basename ```ALMA_1mm_continuum_dirty``` and different extensions. We can have a look at two of them:
 - ```.image``` : final image
 - ```.residual``` : residual containing flux emission that have not been CLEANed

In order to have a look at the images that have been produced we can use the task (or function) ```viewer``` or ```imview```. Which can be opened by typing:

```
viewer
```

This will open the following windows:

![Figure 02](https://github.com/betacygni/ICESummerSchool2023/blob/main/ICESummerSchool2023_02.png)

We can select the ```.image``` file and open it. This should appear in the main window of the ```viewer``` task, and look like this:

![Figure 03](https://github.com/betacygni/ICESummerSchool2023/blob/main/ICESummerSchool2023_03.png)

By selecting on the "blue folder" icon (top bar) you can inspect more files and select another one to be opened. Select the .residual file and open it. With the "blue arrows" you can switch from one image to the other. Check the name of the file just above the images.

Q.- Do you see some differences between the two images?

The residuals are the same as the "CLEANed" image and this is just because we have set the number of iterations to 0, i.e., we have not CLEANed the data. Therefore, all the flux remains in the residuals. Let's proceed now to do some CLEANing.
 
**Produce a CLEANed iamge**:
```
tclean(vis = 'ALMA_1mm.ms',
    datacolumn = 'data',
    imagename = 'ALMA_1mm_continuum_cleaned',
    imsize = 512,
    cell = '0.02arcsec',
    niter = 1000,
    interactive = False)
```

You can again use the ```viewer``` or ```imview``` task to open the new images that have been created. Check again the ```.image``` and ```.residual``` files.

Q.- Do you see some differences between the residuals and the CLEANed image?
Q.- Do you still see emission remaining in the residuals?

We can CLEAN a bit deeper to remove as much as possible the emission in the residuals. The next step may take a much longer processing time.

**Produce a deeper CLEANed image**:
```
tclean(vis = 'ALMA_1mm.ms',
    datacolumn = 'data',
    imagename = 'ALMA_1mm_continuum_cleaned_deeper',
    imsize = 512,
    cell = '0.02arcsec',
    niter = 1000000,
    interactive = False)
```

As in the two previous cases, inspect with the ```viewer``` (or ```imview```) task the newly generated images (the ```.image``` and ```.residual``` files).

Q.- How much emission is remaining in the residuals?


We have now produced an image for the 1mm file. Let's produce now similar images for the 3mm. For this, create the three images (**dirty**, **cleaned** and **cleaned_deeper**) for the ```ALMA_3mm.ms``` file. Inspect also with ```viewer``` the produced images.


## Analyzing images of dust emission

Once the images have been produced, we can use them to calculate fluxes, masses and other physical parameters. The images produces with CASA can be exported into FITS format using the task ```exportfits``` and be analyzed in other softwares (e.g., within python or ds9). For simplicity we will use some functionalities within CASA for the analysis proposed in this hands-on session.

### Calculating fluxes

We will start by calculating the fluxes of the main sources detected with ALMA. We can do this using the ```viewer``` (or ```imview```) function. Once the image is loaded within the viewer, you can zoom-in and define a polygon corresponding to the size-area of the source. All this can be done by using the icons available in the top bar of the viewer window.

 - The lenses (to the right in the panel list) can be used to zoom in and out the image. You can zoom-in into the source to better see the structure and morphology.
 - The icons with an ```R``` inside a square, a circle and an irregular polygon can be used to define different regions (or areas) within the map. You can define a polygon that matches the emission of the source. To define the polygon, you can click at the positions of the vertices within the map. The polygon can be closed by double-clicking with the mouse on the image.
 - Once the polygon is defined you will see a series of new tabs to the right of the image. These tabs include information regarding ```Properties```, ```Statistics```, ```Fit```, ```File``` and ```Histogram```. With the polygon defined, go to the tab of Statistics. There you will find information on the ```FluxDensity``` in units of Jy. This is the flux of your source
 - Obtain the flux of the source at both 1mm and 3mm

You can also calcualte the fluxes for the ```_dirty``` and ```_cleaned``` images and compare with the ```_cleaned_deeper```.
Q.- Do you see differences? Why do you think the fluxes are the same or not the same?

### Derive dust masses

Once we have the flux at 1mm and 3mm, we can proceed to calculate the mass of dust in this source. For this, we can use some of the expressions that were introduced in the lectures. As a reminder, assuming that the emission is optically thin and comes from dust, we have:

$M_\mathrm{d} = \frac{S_\nu D^2}{B_\nu(T_\mathrm{d}) \kappa_\nu}$

where $M_\mathrm{d}$ is the mass of dust; $S_\nu$ is the flux density at frequency $\nu$; $D$ is the distance to the source; $B_\nu(T_\mathrm{d})$ is the black-body function at the dust temperature $T_\mathrm{d}$; and $\kappa_\nu$ is the dust opacity coefficient. The black-body or Planck function is defined as: $B_\nu(T_\mathrm{d})=\frac{2h\nu^3}{c^2}\frac{1}{e^{h\nu/kT_\mathrm{d}}-1}$, with $h$ the Planck constant, $c$ the speed of light, and $k$ the Boltzman constant. The dust opacity can be obtained from softwares like ```optool``` as we saw in the first hands-on session, or can be directly derived from tabulated values such as those presented in [Ossenkopf and Henning (1994)](https://ui.adsabs.harvard.edu/abs/1994A%26A...291..943O/abstract)

The equation above can be written, after replacing some numeric numbers, as:

$`
\left[\frac{M_\mathrm{d+g}}{M_\odot}\right] = 0.00325
\left[\frac{\nu}{\mathrm{THz}}\right]^{-3}
\left[\frac{\kappa_\nu}{\mathrm{cm}^2\mathrm{g}^{-1}}\right]^{-1}
\left[\exp\left(0.048\frac{\left[\nu/\mathrm{GHz}\right]}{\left[T_\mathrm{d}/\mathrm{K}\right]}\right)-1\right]
\left[\frac{S_\nu}{\mathrm{Jy}}\right]
\left[\frac{D}{\mathrm{kpc}}\right]^{2}
`$

The different parameters to be used are:
 - Flux density $S_\nu$ : use the fluxes determined at 1mm and at 3mm
 - Frequency $\nu$ : use the frequencies for 1mm and 3mm that were found in the listobs files at the beginning of the session
 - Distance to the source $D$ : G31.41+0.31 is located at a distance of 3.75 kpc

We still need to find some values for the temperature and dust opacity.

For the dust temperature, we do not have (in this dataset) any direct measurement of the temperature. Based on the informations that we have seen in the lectures, dense cores can have a temperature of about 20 K. Let's use this value.

For the dust opacity, we can make use of the values from [Ossenkopf and Henning (1994)](https://ui.adsabs.harvard.edu/abs/1994A%26A...291..943O/abstract). Check their Table 1, for which a fraction is shown in the image below. The last entry in the table corresponds to 1.3 mm (a wavelength similar to the one that we observed with ALMA). We have different options of opacities depending if we want to consider normal ```MRN grains```, ```MRN grains with thin ice mantles``` or ```MRN with thick ice mantles```. Within each of these groups we can use values for more diffuse gas, or for densities of $10^6$ cm$^{-3}$ and $10^8$ cm$^{-3}$.

![Figure 04](https://github.com/betacygni/ICESummerSchool2023/blob/main/ICESummerSchool2023_04.png)

**Questions**
- Calculate the dust mass assuming 20 K, and dust opacities corresponding to dust grains of the different populations at a density of $10^6$ cm$^{-3}$.
- Assuming that the dust-to-gas mass ratio is 0.01, you can calculate the total mass of dust plus gas by multiplying the dust mass by 100. Is this a massive dense core?
- This object is in reality a hot core with a temperature of 200 K. Which is the mass that you derive if use the correct temperature? What would be the error if we use a wrong temperature of only 20 K?

### Calculate spectral indices

The spectral index $\alpha$ is defined as

$S_\nu \propto \nu^\alpha$

This value can be determined if we have observations at two different wavelengths (or frequencies). This is the case for our data!

We can determine the spectral index by using the fluxes that we derived for 1mm and 3mm in the previous section of this hands-on exercise. Making use of the frequencies that we found in the listobs files, you can now determine what is the spectral index of G31.41+0.31

**Questions**
 - What is the spectral index $\alpha$ for the dust emission between 1 and 3mm for G31.41+0.31?
 - The spectral index for the dust is usually $\alpha = \beta+2$, with $\beta$ ranging between 0 and 2. What is the value of $\beta$ that we obtain?

### Spectral index maps

Complementary to the average spectral index that we have defined for the whole dense core in G31.41+0.31, we can also generate spectral index maps using the two images that we produced at 1mm and 3mm. For this, we will use the task within CASA named ```immath```. Type within CASA the following:

```
inp immath
```

to list all the parameters of this task. In this case we will use the two images and calculate the spectral index by doing:

```
immath(imagename = ['ALMA_3mm_continuum_cleaned_deeper.image', 'ALMA_1mm_continuum_cleaned_deeper.image'],
    mode = 'spix',
    outfile = 'ALMA_spindex.image')
```

The new image ```ALMA_spindex.image``` can be opened using the ```viewer``` (or ```imview```) in a similar way to what was done with the previous images.

**Questions**
 - What is the typical value of the spectral index across the G31.41+0.31 region?
 - Do you see spatial variations? If so, what do you think is the physical origin of these variations?

## For experts with CASA and ALMA

If you have been using CASA and ALMA in the past, you may want to try new things in addition to what has been introduced before. We list here some possible aspects to explore:

 - The 1mm and 3mm datafiles contain observations at four different frequencies each one. Create an image of each frequency sub-band individually (e.g., by using the parameter ```spw``` within ```tclean```. Do you see differences between the different individual images?
 - Once you have all the individual images at the multiple frequencies (i.e., a total of 8, four in the 1mm band and four in the 3mm band), you can calculate the spectral index using all of them, or between pairs of images. How does the spectral index for the images in-band (i.e., for the four images within the 1mm file or the 3mm file) compare with the general spectral index derived using the two main images at 1mm and 3mm?
 - You can also derive the mass from the flux measured in the 3mm image. How does the mass at 3mm compares with that at 1mm? What can you conclude regarding the dust properties based on these results?





