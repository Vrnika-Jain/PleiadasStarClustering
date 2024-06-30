# PleiadasStarClustering
Pleiadas star cluster is popularly known as Seven Sisters. This document provides a detailed step-by-step guide to retrieve data from the Gaia mission for the Pleiades Cluster and save it into a CSV file.


## Table of Contents
1. Its attributes
2. Install Required Packages
3. Import Necessary Packages
4. Define Timer Functions
5. Investigate Available Gaia Tables
6. Sample Query
7. Convert Data to Pandas DataFrame
8. Querying Gaia for Pleiades Cluster Data
9. Convert and Save Pleiades Data
10. Output


## Its attributes-->
1. *Absolute Magnitude and Distance :*
Absolute magnitude (M) is a measure of the luminosity of a celestial object, on an inverse logarithmic astronomical magnitude scale. Calculate the Absolute magnitude of the stars in your cluster.
m − M = 5log(d) − 5
Note that d = 1/ω, where d is distance measured in parsecs and ω is parallax measured in arcseconds. Our data column, plx, is parallax measured in milliarcseconds, so ω = plx /1000 and d = 1000/plx. Make a density plot of the distances to the stars in your cluster using Seaborn. Exclude the outliers that maybe present in your data and then find the mean distance to your cluster.

2. *Luminosity and Temperature :*
Calculate the Luminosity of stars in your cluster using M. Compare the luminosity that you have derived with the actual luminosity of the stars using lum_val. Do the same for effective Temperature and Radius of the stars in your cluster and compare with actual values of temperature (teff_val) and radius (radius_val). i.e. 
M = 4.77 − 2.5log(L/Lo)
and 
TK = 5601/(color + 9.4)<sup>2/3</sup>

Color is represented by bp_rp. Don’t forget to drop Null values in your data which could lead to possible errors.

3. *Position in the Nigth Sky :*
Define brightness of the stars to be difference of max(gmag) and gmag.
Plot the position of the stars in your cluster using Right Ascension and Declination coordinates setting size of a star equal to its brightness.

4. *Hertzsprung-Russell Diagram :*
Make the Absolute magnitude versus color plot to get the HR Diagram for the given cluster. We will then calculate the turnoff mass of the stars to determine the age of the cluster. Turnoff point in a HR diagram is the point from which the stars start deviating from the main sequence locus. It marks the age of the stars just about to escape the main sequence phase and thus this age is used to designate cluster age. We encourage you to find how to estimate the age once you have the turnoff point.


## Install Required Packages
Import the required data science packages
```
import pandas as pd
import numpy as np
from astroquery.gaia import Gaia
from datetime import datetime

print('Modules imported!')
```


## Define Timer Functions
These functions help in timing the execution of the code.
```
def timer_start():
    global start_time
    start_time = datetime.now()

def timer_stop():
    time_elapsed = datetime.now() - start_time
    da, remainder = divmod(time_elapsed.total_seconds(), 24*3600)
    hrs, remainder = divmod(remainder, 3600)
    mins, secs = divmod(remainder, 60)
    if da:
        print(f'{int(da)} days {int(hrs)} hours {int(mins)} minutes {int(secs)} seconds elapsed')
    elif hrs:
        print(f'{int(hrs)} hours {int(mins)} minutes {int(secs)} seconds elapsed')
    elif mins:
        print(f'{int(mins)} minutes {int(secs)} seconds elapsed')
    elif secs >= 1.0:
        print(f'{int(secs)} seconds elapsed')
    else:
        print(f'{secs:.2} seconds elapsed')

print('Timer functions loaded!')
```


## Investigate Available Gaia Tables
Load and look at the available Gaia tables.
```
timer_start()
tables = Gaia.load_tables(only_names=False)
timer_stop()

# Print the name and description of a specific table
i = 93
print(tables[i].get_qualified_name())
print(tables[i].description)
```


## Sample Query
Build and run a sample query to retrieve data from the Gaia source table.
```
myquery = 'SELECT TOP 20 * FROM gaiadr2.gaia_source'

# Run the query and store the results
timer_start()
job = Gaia.launch_job(myquery, dump_to_file=False)
timer_stop()

# Print the job results
print(job)
```


## Convert Data to Pandas DataFrame
Convert the retrieved data into a pandas DataFrame and display the first few rows.
```
# Convert to pandas dataframe
sample_df = (job.get_results()).to_pandas()

# Check that we got a pandas dataframe
type(sample_df)

# Take a look at the first 5 rows
sample_df.head()
```


## Querying Gaia for Pleiades Cluster Data
Query data around the Pleiades Cluster coordinates.
```
from astropy.coordinates import SkyCoord

# Get coordinates for Pleiades
pleiades_coord = SkyCoord.from_name("Pleiades")
print(pleiades_coord)

# Define the query
query = """
SELECT phot_g_mean_mag as gmag, ra, dec, parallax as plx, bp_rp, lum_val, teff_val, radius_val
FROM gaiadr2.gaia_source
WHERE CONTAINS(POINT('ICRS', ra, dec), CIRCLE('ICRS', 56.75, 24.11667, 1.833)) = 1
AND parallax IS NOT NULL AND abs(parallax) > 0
AND parallax_over_error > 10
AND abs(pmra_error / pmra) < 0.10
AND abs(pmdec_error / pmdec) < 0.10
AND pmra IS NOT NULL AND abs(pmra) > 0
AND pmdec IS NOT NULL AND abs(pmdec) > 0
AND pmra BETWEEN 15 AND 25
AND pmdec BETWEEN -55 AND -40;
"""

# Run the query and store the results
timer_start()
job = Gaia.launch_job(query, dump_to_file=False)
timer_stop()

# Print the job results
print(job)
```


## Convert and Save Pleiades Data
Convert the Pleiades data into a pandas DataFrame and save it as a CSV file.
```
# Convert to pandas dataframe
df = (job.get_results()).to_pandas()

# Print column names
for col in df.columns:
    print(col)

# Display the first few rows of the dataframe
df.head()

# Print the number of rows in the dataframe
print(len(df))

# Save the dataframe to a CSV file
df.to_csv("Pleiades_Cluster.csv", index=False)
```


## Output
Your Pleiades Cluster data will be saved in a CSV file named Pleiades_Cluster.csv.
