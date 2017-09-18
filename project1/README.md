# Project 1

#### _CMSC320_

**Posted:** September 14, 2017; 
**Last Updated:** September 14, 2017; 
**Due:** September 29, 2017

You've been hired by a new space weather startup looking to disrupt the space weather reporting business. Your first project is to provide better data about the top 50 solar flares recorded so far than that shown by your competitor [SpaceWeatherLive.com](https://www.spaceweatherlive.com/en/solar-activity/top-50-solar-flares). To do this, they've pointed you to [this messy HTML page](http://cdaw.gsfc.nasa.gov/CME_list/radio/waves_type2.html) from NASA ([available here also](http://www.hcbravo.org/IntroDataSci/misc/waves_type2.html)) where you can get the extra data your startup is going to post in your new spiffy site.

Of course, you don't have access to the raw data for either of these two tables, so as an enterprising data scientist you will scrape this information directly from each HTML page using all the great tools available to you in Python. By the way, you should read up a bit on [Solar Flares](https://en.wikipedia.org/wiki/Solar_flare), [coronal mass ejections](https://www.spaceweatherlive.com/en/help/what-is-a-coronal-mass-ejection-cme), [the solar flare alphabet soup](http://spaceweather.com/glossary/flareclasses.html), [the scary storms of Halloween 2003](http://www.nasa.gov/topics/solarsystem/features/halloween_storms.html), and [sickening solar flares](https://science.nasa.gov/science-news/science-at-nasa/2005/27jan_solarflares).

## Part 1: Data scraping and preparation

### Step 1: Scrape your competitor's data (10 pts)

Use Python to scrape data for the top 50 solar flares shown in [SpaceWeatherLive.com](https://www.spaceweatherlive.com/en/solar-activity/top-50-solar-flares). Steps to do this are:

1.  `pip install` or `conda install` the following Python packages: `beautifulsoup4, requests, pandas, numpy`; these are already in the environment if you are using Docker.
2.  Use `requests` to get (as in, HTTP GET) the URL
3.  Extract the text from the page
4.  Use BeautifulSoup to read and parse the data, either as html or lxml
5.  Use prettify() to view the content and find the appropriate table
6.  Use find() to save the aforementioned table as a variable
7.  Use pandas to read in the HTML file.  HINT make-sure the above data is properly typecast.
8.  Set reasonable names for the table columns, e.g., rank, x_classification, date, region, start_time, maximum_time, end_time, movie. `Pandas.columns` makes this very simple.


The result should be a data frame, with the first few rows as:

```
Dimension: 50 × 8

rank x_class date region start_time max_time end_time movie

1  1 X28.0 2003/11/04 0486  19:29  19:53  20:06 MovieView archive

2  2 X20 2001/04/02 9393  21:32  21:51  22:03 MovieView archive

3  3 X17.2 2003/10/28 0486  09:51  11:10  11:24 MovieView archive

4  4 X17.0 2005/09/07 0808  17:17  17:40  18:03 MovieView archive

5  5 X14.4 2001/04/15 9415  13:19  13:50  13:55 MovieView archive

6  6 X10.0 2003/10/29 0486  20:37  20:49  21:01 MovieView archive

7  7  X9.4 1997/11/06  -  11:49  11:55  12:01 MovieView archive

8  8  X9.0 2006/12/05 0930  10:18  10:35  10:45 MovieView archive

9  9  X8.3 2003/11/02 0486  17:03  17:25  17:39 MovieView archive

10  10  X7.1 2005/01/20 0720  06:36  07:01  07:26 MovieView archive

 ... with 40 more rows
```

### Step 2: Tidy the top 50 solar flare data (10 pts)

Your next step is to make sure this table is usable using pandas:

1.  Drop the last column of the table, since we are not going to use it moving forward.
2.  Use datetime import to combine the date and each of the three time columns into three datetime columns. You will see why this is useful later on.  iterrows() should prove useful here.
3.  Update the values in the dataframe as you do this.  Set_value should prove useful.
4.  Set regions coded as - as missing (NaN). You can use dataframe.replace() here.

The result of this step should be a data frame with the first few rows as:

```
A dataframe: 50 × 6

rank x_class  start_datetime  max_datetime  end_datetime region

1  1 X28.0 2003-11-04 19:29:00 2003-11-04 19:53:00 2003-11-04 20:06:00 0486

2  2 X20 2001-04-02 21:32:00 2001-04-02 21:51:00 2001-04-02 22:03:00 9393

3  3 X17.2 2003-10-28 09:51:00 2003-10-28 11:10:00 2003-10-28 11:24:00 0486

4  4 X17.0 2005-09-07 17:17:00 2005-09-07 17:40:00 2005-09-07 18:03:00 0808

5  5 X14.4 2001-04-15 13:19:00 2001-04-15 13:50:00 2001-04-15 13:55:00 9415

6  6 X10.0 2003-10-29 20:37:00 2003-10-29 20:49:00 2003-10-29 21:01:00 0486

7  7  X9.4 1997-11-06 11:49:00 1997-11-06 11:55:00 1997-11-06 12:01:00 <NA>

8  8  X9.0 2006-12-05 10:18:00 2006-12-05 10:35:00 2006-12-05 10:45:00 0930

9  9  X8.3 2003-11-02 17:03:00 2003-11-02 17:25:00 2003-11-02 17:39:00 0486

10  10  X7.1 2005-01-20 06:36:00 2005-01-20 07:01:00 2005-01-20 07:26:00 0720

 ... with 40 more rows
```

### Step 3: Scrape the NASA data (15 pts)

Next you need to scrape the data in [http://cdaw.gsfc.nasa.gov/CME_list/radio/waves_type2.html](http://cdaw.gsfc.nasa.gov/CME_list/radio/waves_type2.html) ([also available here](http://www.hcbravo.org/IntroDataSci/misc/waves_type2.html)) to get additional data about these solar flares. This table format is described here: [http://cdaw.gsfc.nasa.gov/CME_list/radio/waves_type2_description.htm](http://cdaw.gsfc.nasa.gov/CME_list/radio/waves_type2_description.htm), and here:

```
The Wind/WAVES type II burst catalog: A brief description

URL: [http://cdaw.gsfc.nasa.gov/CME_list/radio/waves_type2.html](http://cdaw.gsfc.nasa.gov/CME_list/radio/waves_type2.html).

This is a catalog of type II bursts observed by the Radio and Plasma Wave (WAVES) experiment on board the Wind spacecraft and the associated coronal mass ejections (CMEs) observed by the Solar and Heliospheric Observatory (SOHO) mission. The type II burst catalog is derived from the Wind/WAVES catalog available at [http://ssed.gsfc.nasa.gov/waves/data_products.html](http://ssed.gsfc.nasa.gov/waves/data_products.html) by adding a few missing events.

The CMEs in this catalog are called radio-loud CMEs because of their ability to produce type II radio bursts. The CME sources are also listed, as derived from the Solar Geophysical Data listing or from inner coronal images such as Yohkoh/SXT and SOHO/EIT. Some solar sources have also been obtained from Solarsoft Latest Events Archive after October 1, 2002: [http://www.lmsal.com/solarsoft/latest_events_archive.html](http://www.lmsal.com/solarsoft/latest_events_archive.html)


Explanation of catalog entries:

Column 1: Starting date of the type II burst (yyyy/mm/dd format)

Column 2: Starting time (UT) of the type II burst (hh:mm format)

Column 3: Ending date of the type II burst (mm/dd format; year in Column 1 applies)

Column 4: Ending time of the Type II burst (hh:mm format)

Column 5: Starting frequency of type II burst (kHz) [1]

Column 6: Ending frequency of type II burst (kHz) [1]

Column 7: Solar source location (Loc) of the associated eruption in heliographic coordinates [2]

Column 8: NOAA active region number (NOAA) [3]

Column 9: Soft X-ray flare importance (Imp) [4]

Column 10: Date of the associated CME (mm/dd format, Year in Column 1 applies) [5]

Column 11: Time of the associated CME (hh:mm format)

Column 12: Central position angle (CPA, degrees) for non-halo CMEs [6]

Column 13: CME width in the sky plane (degrees) [7]

Column 14: CME speed in the sky plane (km/s)

Column 15: Link to the daily proton, height-time, X-ray (PHTX) plots [8]

Notes

[1] ???? indicate that the starting and ending frequencies are not determined.

[2] Heliographic coordinates. S25E16 means the latitude is 25 deg south and 16 deg east (source located in the southeast quadrant of the Sun. N denotes northern latitudes and W denotes western longitudes. Entries like SW90 indicate that the source information is not complete, but we can say that the eruption occurs on the west limb but at southern latitudes; if such entries have a subscript b (e.g., NE90b) it means that the source is behind the particular limb. This information is usually gathered from SOHO/EIT difference images, which show dimming above the limb in question. Completely backside events with no information on the source location are marked as “back”.

[3] If the active region number is not available or if the source region is not an active region, the entry is “—-”. Filament regions are denoted by “FILA” or “DSF” for disappearing solar filament.

[4] Soft X-ray flare size (peak flux in the 1-8 A channel) from GOES. “—-” means the soft X-ray flux is not available.

[5] Lack of SOHO observations are noted as “LASCO DATA GAP”. Other reasons are also noted if there is no CME parameters measured.

[6] The central position angle (CPA) is meaningful only for non-halo CMEs. For halo CMEs, the entry is “Halo”. For halo CMEs, the height-time measurements are made at a position angle where the halo appears to move the fastest. This is known as the measurement position angle (MPA) and can be found in the main catalog ([http://cdaw.gsfc.nasa.gov/CME_List](http://cdaw.gsfc.nasa.gov/CME_List)).

[7] Width = 360 means the CME is a fill halo (see [6]). For some entries, there is a prefix “>”, which means the reported width is a lower limit.

[8] ‘PHTX’ (proton, height-time, X-ray) link to three-day overview plots of solar energetic particle events (protons in the >10, >50 and >100 MeV GOES channels).



Links:

The CMEs and the type II bursts can be viewed together using the c2rdif_waves.html movies linked to the starting frequency (Column 5). The c3rdif_waves.html movies are linked to the ending frequencies (Column 6). The CMEs and the GOES flare light curves for a given type II burst can be viewed from the Javascript movies linked to the CME date (Column 10). The height-time plots (linear and quadratic) of the CMEs are linked to the CME speed (Column 14).

PHTX plots are linked to Column 15.



If you have questions, contact: Nat Gopalswamy ([gopals@ssedmail.gsfc.nasa.gov](mailto:gopals@ssedmail.gsfc.nasa.gov))

This work is supported by NASA’s Virtual Observatories Program
```





#### Tasks

1.  Use BeautifulSoup functions (e.g., find, findAll) and string functions (e.g., split and built-in slicing capabilities) to obtain each row of data as a long string. Create a DataFrame at this point so it’s easier to use melt or wide_to_long for the next few steps.
2.  Use string::split and list comprehensions or similar to separate each line of text into a data row. Choose appropriate names for columns.

The result of this step should be similar to:

```
Dimension: 482 × 14

start_date start_time end_date end_time start_frequency end_frequency flare_location flare_region

* <chr>  <chr>  <chr>  <chr> <chr> <chr>  <chr>  <chr>

1  1997/04/01  14:00  04/01  14:15  8000  4000 S25E16 8026

2  1997/04/07  14:30  04/07  17:30 11000  1000 S28E19 8027

3  1997/05/12  05:15  05/14  16:00 12000  80 N21W08 8038

4  1997/05/21  20:20  05/21  22:00  5000 500 N05W12 8040

5  1997/09/23  21:53  09/23  22:16  6000  2000 S29E25 8088

6  1997/11/03  05:15  11/03  12:00 14000 250 S20W13 8100

7  1997/11/03  10:30  11/03  11:30 14000  5000 S16W21 8100

8  1997/11/04  06:00  11/05  04:30 14000 100 S14W33 8100

9  1997/11/06  12:20  11/07  08:30 14000 100 S18W63 8100

10 1997/11/27  13:30  11/27  14:00 14000  7000 N17E63 8113

... with 472 more rows, and 6 more variables: flare_classification <chr>, cme_date <chr>, cme_time <chr>, cme_angle <chr>, cme_width <chr>, cme_speed <chr>
```




### Step 4: Tidy the NASA the table (15 pts)

Now, we tidy up the NASA table. Here we will code missing observations properly, recode columns that correspond to more than one piece of information, and treat dates and times appropriately.

1.  Recode any missing entries as NaN. Refer to the data description in [http://cdaw.gsfc.nasa.gov/CME_list/radio/waves_type2_description.htm](http://cdaw.gsfc.nasa.gov/CME_list/radio/waves_type2_description.htm)  (and above) to see how missing entries are encoded in each column. Be sure to look carefully at the actual data, as the nasa descriptions might not be completely accurate.
2.  The CPA column (cme_angle) contains angles in degrees for most rows, except for halo flares, which are coded as Halo. Create a new column that indicates if a row corresponds to a halo flare or not, and then replace Halo entries in the cme_angle column as NA.
3.  The width column indicates if the given value is a lower bound. Create a new column that indicates if width is given as a lower bound, and remove any non-numeric part of the width column.
4.  Combine date and time columns for start, end and cme so they can be encoded as datetime objects.



The output of this step should be similar to this:

```
start_datetime  end_datetime start_frequency end_frequency flare_location flare_region importance  cme_datetime  cpa width speed  plot is_halo width_lower_bound

0 1997-04-01 14:00:00 1997-04-01 14:15:00  8000  4000 S25E16 8026 M1.3 1997-04-01 15:18:00 74  79 312  PHTX False False

1 1997-04-07 14:30:00 1997-04-07 17:30:00 11000  1000 S28E19 8027 C6.8 1997-04-07 14:27:00  NaN 360 878  PHTX  True False

2 1997-05-12 05:15:00 1997-05-14 16:00:00 12000  80 N21W08 8038 C1.3 1997-05-12 05:30:00  NaN 360 464  PHTX  True False

3 1997-05-21 20:20:00 1997-05-21 22:00:00  5000 500 N05W12 8040 M1.3 1997-05-21 21:00:00  263 165 296  PHTX False False

4 1997-09-23 21:53:00 1997-09-23 22:16:00  6000  2000 S29E25 8088 C1.4 1997-09-23 22:02:00  133 155 712  PHTX False False

5 1997-11-03 05:15:00 1997-11-03 12:00:00 14000 250 S20W13 8100 C8.6 1997-11-03 05:28:00  240 109 227  PHTX False False
```







## Part 2: Analysis

Now that you have data from both sites, let’s start some analysis.

### Question 1: Replication (10 pts)

Can you replicate the top 50 solar flare table in [SpaceWeatherLive.com](https://www.spaceweatherlive.com/en/solar-activity/top-50-solar-flares) exactly using the data obtained from NASA? That is, if you get the top 50 solar flares from the NASA table based on their classification (e.g., X28 is the highest), do you get data for the same solar flare events?

Include code used to get the top 50 solar flares from the NASA table (be careful when ordering by classification). Write a sentence or two discussing how well you can replicate the SpaceWeatherLive data from the NASA data.

### Question 2: Integration (15 pts)

Write a function that finds the best matching row in the NASA data for each of the top 50 solar flares in the SpaceWeatherLive data. Here, you have to decide for yourself how you determine what is the best matching entry in the NASA data for each of the top 50 solar flares.

In your submission, include an explanation of how you are defining best matching rows across the two datasets in addition to the code used to find the best matches. Finally, use your function to add a new column to the NASA dataset indicating its rank according to SpaceWeatherLive, if it appears in that dataset.

### Question 3: Analysis (10 pts)

Prepare one plot that shows the top 50 solar flares in context with all data available in the NASA dataset. Here are some possibilities (you can do something else)

1.  Plot attributes in the NASA dataset (e.g., starting or ending frequenciues, flare height or width) over time. Use graphical elements (e.g., text or points) to indicate flares in the top 50 classification.
    
2.  Do flares in the top 50 tend to have Halo CMEs? You can make a barplot that compares the number (or proportion) of Halo CMEs in the top 50 flares vs. the dataset as a whole.
    
3.  Do strong flares cluster in time? Plot the number of flares per month over time, add a graphical element to indicate (e.g., text or points) to indicate the number of strong flares (in the top 50) to see if they cluster.
    

## Submission

Prepare an Jupyter Notebook file that includes for each step in Part 1: (a) code to carry out the step discussed, (b) output showing the output of your code, similar to the examples above, and (c) a short prose description of how your code works. For questions 1 and 2 of Part 2, follow the instructions there. For Question 3 of part 2 provide: (a) a short description (2 sentences) of what the intent of your plot is (think in terms of our discussion on how we show variation, co-variation in terms of central trend, spread, skew etc.), (b) code to produce your plot, (c) a short text description of your plot, and (d) a sentence or two of interpretation of your plot (again think of variation, co-variation, etc.).

Submit the resulting .ipynb file to ELMS at: [https://myelms.umd.edu/courses/1227895/assignments/4501288](https://myelms.umd.edu/courses/1227895/assignments/4501288)

### Group work

You are encouraged to work in small groups, but you must prepare your own writeup and submit it. Include the names of the peers who you worked with in the writeup.
