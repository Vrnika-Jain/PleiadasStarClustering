# PleiadasStarClustering
Pleiadas star cluster is popularly known as Seven Sisters. 

Its attributes-->

1. Absolute Magnitude and Distance
Absolute magnitude (M) is a measure of the luminosity of a celestial object, on an inverse logarithmic astronomical magnitude scale. Calculate the Absolute magnitude of the stars in your cluster.
m − M = 5log(d) − 5
Note that d = 1/ω, where d is distance measured in parsecs and ω is parallax measured in arcseconds. Our data column, plx, is parallax measured in milliarcseconds, so ω = plx /1000 and d = 1000/plx. Make a density plot of the distances to the stars in your cluster using Seaborn. Exclude the outliers that maybe present in your data and then find the mean distance to your cluster.

2. Luminosity and Temperature
Calculate the Luminosity of stars in your cluster using M. Compare the luminosity that you have derived with the actual luminosity of the stars using lum_val. Do the same for effective Temperature and Radius of the stars in your cluster and compare with actual values of temperature (teff_val) and radius (radius_val).
M = 4.77 − 2.5log(L/Lo)

TK = 5601/(color + 9.4)<sup>2/3</sup>

Color is represented by bp_rp. Don’t forget to drop Null values in your data which could lead to possible errors.

3. Position in the Nigth Sky
Define brightness of the stars to be difference of max(gmag) and gmag.
Plot the position of the stars in your cluster using Right Ascension and Declination coordinates setting size of a star equal to its brightness.

4. Hertzsprung-Russell Diagram
Make the Absolute magnitude versus color plot to get the HR Diagram for the given cluster. We will then calculate the turnoff mass of the stars to determine the age of the cluster. Turnoff point in a HR diagram is the point from which the stars start deviating from the main sequence locus. It marks the age of the stars just about to escape the main sequence phase and thus this age is used to designate cluster age. We encourage you to find how to estimate the age once you have the turnoff point.
