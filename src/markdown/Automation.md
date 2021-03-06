## “Robot” Experiments

Given the complex nature and high cost of flow cytometers, the team began looking into lower-cost solutions to measure cell fluorescence that could be built in-house and integrated with the turbidostat. The end goal of this was to create a device that could be run off of a Raspberry Pi or an Arduino that could simultaneously control the turbidostat (which is already controlled by an Arduino). The "robot" designed below was never fully integrated with the rest of the project hardware, but an optimal design for a fluorescence-measuring design was determined. Therefore, such an integration could be reasonably implemented in the future.

### Initial Experiment

Having determined the utility of an automated sampling system, the team first set out to confirm whether or not cells with different levels of GFP expressed could be visually distinguished when exposed to a ~488 nm light source.  To this end, a light source with a peak emission at 488 nm and a light filter capable of letting green light but not blue light pass through were borrowed from the Reed lab at the University of Waterloo.

Cells (specifically BW 29655 E. coli) expressing GFP under the CcaR promoter were grown to an OD of ~1 under both red and green light in complete M9.  Unfortunately fluorescence measurements were not taken under the flow cytometer, but for the record the cells grown under red light were white while those grown under green light were visibly green.  Upon shining blue light on the cells and taking a picture through the filter, the following image was obtained:

<center><img src="http://2018.igem.org/wiki/images/a/a4/T--Waterloo--July18_initialFluorescenceCheck.jpg" /></center>

The container on the right was the one grown under green light, and it is very clearly distinguishable from the container on the right.

### Box Design and Test with Photodiode

A box was designed using AutoDesk Inventor (a 3D computer aided design software) to hold the components of the measurement system (sample tube, LED, and photodiode/camera) in place for each measurement as well as to block out ambient light.  Using CAD software also allows us to 3D print the final box design if we so desire.  The measurement system involves exciting the GFP with an LED (488 nm) and measuring the emitted fluorescence (510 nm).  To measure the emitted light without measuring the excitation light from the LED, the filter lent to us by the Reed lab was used so that only 510 nm light would be absorbed by the photodiode/camera.  See the first image below for the initial box design, and the second image below for the principle of operation.

<center><img src="http://2018.igem.org/wiki/images/5/53/T--Waterloo--boxDesign.png" /></center>

<center><img src="http://2018.igem.org/wiki/images/1/15/T--Waterloo--boxPrincipleOfOperation.png" /></center>

The design was initially tested using a cardboard box with holes cut in it as shown in the CAD drawing.  We attempted to measurement the fluorescence of a sample tube of cells in the box with a photodiode.  The box prevented ambient light from entering the system and kept the parts in place as desired, however we found we could not get consistent readings when we repeatedly measured the same sample; slight fluctuations in sample and LED position together with possible photodiode malfunction were responsible for these inconsistencies. As a result of this experiment, we decided to try using a camera instead.  We also found that our initial placements of the components was not optimal and concluded that we must perform an experiment to optimize the distances and placements of components without the box.

### Quantification Test with Camera

After the unclear results of the test with the original box apparatus, it was determined that the test using the camera should be repeated in a more quantitative manner. Similarly to the initial experiment, E. coli (this time JT2 containing GFP under the CcaR promoter) were grown up to an OD of ~0.6 in complete M9. Two samples were grown under green light, and two under red light.

The tubes were arranged in a row in front of an iPhone camera, which our light filter in front of it. The tubes were placed in two alternate arrangements, and a movie was taken of the blue LED being passed behind them. See below for one of the movies taken.

<video width="100%" height="480" controls> 
<source src="http://2018.igem.org/wiki/images/9/97/T--Waterloo--August21_robotsExp1.mov" type="video/mp4"> 
</video>

As one can see (especially when looking at the bottoms of the vials), the first and third vials from the right are brighter. These were the vials grown under green light, and hence the cells in them had more GFP. Screenshots of the vials were taken as the light was directly behind them, and the images were cropped down to just the vial (see below for an example of a cropped image).

<center><img src="http://2018.igem.org/wiki/images/1/19/T--Waterloo--August21_Arrangement1Vial1.jpg" /></center>

These images were then fed into a Python program that broke them down into their RGB values. The red, green, and blue values of each pixel in each image were taken and averaged over each image. Since some blue light still made it through the filter, average blue values for the images could not be used to distinguish the samples. Additionally, the green values were informative but still relatively similar in each image, mostly because a strong blue light actually emits some green light as well. However, examining the average red values made the cells grown under red and green light easily distinguishable, with the more fluorescent cells being more “red” (as GFP actually emits some red light).

This led the team to conclude that our approach for distinguishing fluorescent and non-fluorescent cells was valid, and the failure of the initial box design was due to poor placement and control of the position of our photodiode, sample vial, and LED. We thus set out to find the optimal arrangement and implement that in future designs.

For the full code of the Python program, see:
https://github.com/igem-waterloo/uwaterloo-igem-2018/blob/master/models/image/greenFluorescenceQuantifier.py

For the full experimental protocol, see the online lab book (pages 80 and 81).

### Optimization Test with Camera

Following the successful fluorescence-quantization experiment, the team determined that it was necessary to optimize the positions of both the camera and the blue LED relative to the sample. To do this, an experiment was designed according to the following diagram:

<center><img src="http://2018.igem.org/wiki/images/4/41/T--Waterloo--Sept24_setupArrangementDiagram.png" /></center>

The green squares represent positions for the camera, the red squares represent positions for the blue LED, and the white circle represents the sample. Every combination of camera and LED position was tried.

For the samples, new cells were used. DH5α containing a GFP-expressing cassette (registry part I20270) in pSB1C3 was used as a “fluorescent” strain (called strain 346) and empty JT2 was used as a “non-fluorescent” strain. Both cultures were grown up from frozen stock overnight in LB, then the cells were moved to complete M9 for ~3 hours of growth. After the end of this growth, the fluorescence values of the JT2 strain and strain 346 were taken using a flow cytometer in the Ingalls Lab. Under a laser intensity of 10 mW and using a sample size 10,000 cells, the fluorescence in arbitrary units was 2375 for strain 346 and 19 for JT2.

From a previous experiment (see online lab book page 111), we have reason to expect that the OD to cells per mL relationship is similar for empty JT2 and strain 346. After 3 hours of growth in M9, JT2 was at an OD of ~0.15 and 346 was at an OD of ~0.3. Strain 346 was diluted down to an OD of ~0.15 with PBS, and then two 25 mL samples were prepared of each strain. Two ~50/50 mixtures of the strains were also prepared.

After the combination of camera and LED positions were tried and photos of the samples taken through the light filter, several things became apparent.
The readings from all camera positions were less consistent between duplicates when the LED was 30 cm away. This was most likely because small changes in sample position or LED orientation resulted in large changes in how the light bounced off the sample tube. Therefore, it is better to have the LED 10 cm away from the sample.
When the camera was 5 cm away, it could not focus properly. Therefore, it is better to have the camera 20 cm away.
When the LED, sample, and camera are in a line, the light coming from the LED can saturate the image; it goes completely white regardless of the sample type, making it hard to distinguish fluorescent and non-fluorescent cells. Therefore it is best to have the path from the sample to the LED and the path from the sample to the camera be perpendicular.

From the above observations, it became clear that the best setup was to have the LED 10 cm away from the sample, have the camera 20 cm from the sample, and have the LED-sample-camera path to be in an L shape. For that particular set up, the results below were obtained after analysis using the Python program for getting average RGB values.

<center><img src="http://2018.igem.org/wiki/images/c/c9/T--Waterloo--Sept24_resultsTableForGoodPosition.png" /></center>

The fluorescence values are a simple average between the values for the duplicates. As one can see, strain 346 is clearly more fluorescent than JT2, and the mixture is of an intermediate fluorescence. This confirms that the system can be used to measure fluorescence and distinguish between different mixtures of fluorescent and non-fluorescent cells. The mixture should have been ~50% fluorescent cells, and by linear interpolation from the data obtained one can get that the mixture is 58% fluorescent cells (from the calculation (100%)*(94.7-71.85)/(111.25-71.85)).

### Practical Use During the Project

The “robot” used to measure fluorescence, now no longer housed in a box, was used to attempt to estimate the ratios of different populations in a co-culture. The sample tube contained both DH5α containing GFP and empty DH5α.

The setup as a whole consisted of a camera with our filter in front of it, a sample tube 15 cm away from this camera, and our blue LED (which was 10 cm away from our sample). This is a diagram representative of the setup:

<center><img src="http://2018.igem.org/wiki/images/f/f8/T--Waterloo--robotAttempt.jpg" /></center>

The actual image of the sample on the camera appeared as follows:

<center><img src="http://2018.igem.org/wiki/images/e/e0/T--Waterloo--robotAttemptImage.jpg" /></center>

During the experiment, we intended to use a flow cytometer to confirm the population ratios, but unfortunately it broke the day before the experiment was set to run. Accordingly, we do not have confirmation that the robot correctly estimated the population ratios. However, the experiment was done in duplicate and the population were monitored over two hours, and the duplicates did at least have very similar average RGB values. This means that the machine was at least precise, even if we cannot speak to its accuracy.

### Second Design and Future Experiments

The team managed to successfully attach a camera to a Raspberry Pi, and have the Pi control and take pictures with the camera. This could eventually be used as a replacement to the phone camera.

In a theoretical future design, the robot could be set up within the turbidostat's incubator, with the turbidostat's intermediate waste vials in the place of the current sample vials seen above. In this way, the robot could measure the fluorescence of the cells flushed out of the turbidostat every time it dilutes a cell culture. It may even be possible to attach an additional vacuum line to the intermediate waste vial. This could be used to clear out the liquid of the intermediate waste vial in an automated function.
