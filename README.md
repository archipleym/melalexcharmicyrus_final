# melalexcharmicyrus_final
Astronomy 596 FDS final project, Spring 2020

Statement of work

We met nearly every other day for at least an hour while working together on the project. I (Melanie) felt our approach to do the same methods on physically different objects worked well, because it was not identical work, but we were able to help each other. We also stuck to our timeline (kind of shocked about that) and accomplished all of our goals!

Melanie - object LSQ12gdj. Used Scalzo et. al 2014 as a starting point for using GPs to extract a bolometric luminosity. In group discussions for a mean model, tried quadratic rise and linear fall, settled on quadratic spline. Working with one band, tried using the peak as a spline point, then shifted it by eye. When first using a GP, copied my HW from when we used george, put in our mean model, and optimized it (referring to previous HW and george tutorials). Shifted everything into pymc3 using pymc3 tutorials, docs, and examples for reference. We had a lot of group coding at this point because it was so finicky. Adopted everything into a hierarchical model then took the median posterior for the lightcurve. Alex definitely did the legwork to get the flux zero points for all of our filters (which apparently existed in superbol) as well as confirming that trapezoidal integration would be fine to do. Bolometric flux, luminosity, and derived temperature this way came out of familiar astrophysical relationships. I was ready to leave it with a constant radius, but Alex convinced me that fitting to a blackbody wouldn't be that hard. It was not hard (copying the superbol code for a blackbody curve), but the resulting shape of the temperature-time plot didn't make a ton of sense to me. At the end, I tried briefly to compare to superbol, but it kept throwing errors, so I did not get a result to compare to. 

Alex - object 2020oi. I downloaded the publically available data from YSE PZ, then ran subtraction photometry on my UCSC account using PhotPipe to augment the full dataset. I started with a quadratic rise and a quadratic decline (connecting the two models at the peak in r-band), but also tried connecting the models at a visual change in decline rate. Initial work fitting on fluxes had massive posterior variance, so I stuck to fitting on magnitudes. I removed the data with magErr < 0.03 and the few data points taken far before the peak. I first created a GP in george using a very similar structure to an earlier problemset - in fact most of my time for this project was devoted to getting a GP working in pymc3. Once in pymc3, it was quick to convert it from one band to a hierarchical model. I initially set only the smoothing parameter in the squared exponential kernel to a random variable for testing, and then switched to fitting for the quadratic parameters. At the end, I added back in the smoothing parameter random variable and another one for the noise level. I played with normal, halfnormal, and skewnormal distributions for these values (I wanted the smoothing parameter to be between ~5 and 50, for example). I then took the median and std of my estimates, and worked out the bolometric lightcurve for the data. I ran superbol to compare the data, and after correcting for a bug found strong agreement. The flux zero points were a bit tricky to sort out, as were the units for the blackbody temperature fits, but after combing through the superbol code we sorted it out. I created the rise template for the slides and we then filled them in together, and we standardized our plots on a set of google slides (with significant prodding from Mel, but I thought aesthetically they turned out great) that I screenshotted and imported to rise. I was very happy with the teamwork in the group - I thought we had a strong balance between individually exploring our objects and group debugging. I have many ideas for ways to improve the results but that's a topic for future work! 

----------------------------------------------
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
