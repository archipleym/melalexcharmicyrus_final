# melalexcharmicyrus_final
Astronomy 596 FDS final project, Spring 2020

What we sent to Gautham:
Goal is to obtain a bolometric lightcurve for supernova 2020oi from individual lightcurves
Steps:
(1) Create a hierarchical model of a black body whose parameters vary with both wavelength and time (using pymc?).
(2) Each lightcurve observed at a single wavelength range is a cross-section of this full 3D surface.
(3) Once we know how the parameters vary with lightcurve, we can integrate over all wavelengths (including the ones we didn’t observe at, using a GP to interpolate).
(4) Get the full bolometric lightcurve.
(5) Estimate the temperature, again assuming blackbody.

His response:
your GP is really multiple GPS (either 1 per pass and - what I think you mean by single wavelength) or a 2D surface across wavelength and time.
If you go with 1D over time then you need the black body part to extrapolate to different wavelength ranges.
If you use a 2D GP then you don’t need a BB at all since the GP defines the model at other wavelengths.

Minimally, compare your output against SuperBol, whatever you elect to do.
It’s also helpful to have some other literature objects other than 2020oi to make sure what you are doing recovers what others found for existing sources.
I would look at Scalzo et al 2014 or 2018 and compare your code on one of their objects that is not 2020oi to their code.
If you pick four objects including 2020oi, each of you can work on one.

Particularly if these are four different classes of SNe then you can also see if this black body assumption holds for other classes.
Particularly since you are going the more stats route, and operating on a small sized dataset, draw physical conclusions about the objects you are studying.
And for SNe you can also substitute kilonovae - there should be a metric ton of data on GW170817A that Villar et al 2018/9?  compiled.
(Also, re. 5, there is no one black body temperature for all time with the SNe/KNe - it’s getting hotter as it rises to maximum and then cools).

So if you do something like a hierarchical model in [1] your parameters are more like a peak temperature and some functional form for how the temperature changes away from weak.
Maybe on the rise it’s parabolic or something, and linear on the decline so you might image two timescale parameters like rise rate and decline rate or something.
