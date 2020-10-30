# capstone_1


## Coffee Quality
* https://www.kaggle.com/volpatto/coffee-quality-database-from-cqi?select=arabica_data_cleaned.csv
* Arabica and Robusta data (origin, bitterness, sweetness, color, overall quality)
* Compare each bean type for what makes a good cup of coffee, and then merged data to explore and compare/contrast
* Are their any shared characteristics that make a quality cup? Which countries/farms have the highest quality? 

# Global Quality of Arabica Coffee Samples
## According to trained reviewers from the Coffee Quality Institute

The main data was pulled from the Coffee Quality Institute's review pages in January of 2018. The raw data contains reviews of 1312 Arabica and 28 Robusta coffee beans. 

Due to the dearth of Robusta reviews, this analysis will focus solely on the Arabica species of coffee (although further research in the future would certainly include finding more data on Robusta for a more "robust" data exploration).

The intent behind the analysis was simple: as a personal fan of coffee, I wanted to know what it is that makes a good cup of coffee? The best cup of coffee? The worst? And also, to discover more about the origination of the coffee that many drink every day.

Each element in this data set is a 2 kilogram sample that was sent by the coffee owner (often the farmer or farm, but not always) to the Coffee Quality Insitute for evaluation. The dataset contains the quality metrics by which the coffee was evaluated and resulting scores, metadata about the bean, and metadata on the originating farm. As mentioned, before cleaning there was 1312 rows of data and 44 columns.

Additionally, to aid in the analysis by hemisphere, I used a second data set on country information including only country name, latitude, and longitude (of roughly the respective country center). This [Kaggle Data](https://www.kaggle.com/eidanch/counties-geographic-coordinates) was originally pulled from a Google open dataset.

In initially looking at the data, a few columns were irrelevant (too many null values, no explanatory power, redundant), but of the useful columns they were surprisingly whole. There were a few misplaced decimals and spelling errors to correct, but very few null values that were dropped.

I considered the columns in roughly two groups: quality measures and bean metadata (including origin/farm)).

### The quality measures:

* Aroma                  
* Flavor                 
* Aftertaste             
* Acidity                
* Body                   
* Balance             
* Uniformity            
* Clean.Cup              
* Sweetness             
* Cupper.Points      
* Total.Cup.Points       
* Moisture                
* Color
* Quakers
* Category.One.Defects  
* Category.Two.Defects   

### Bean Metadata:

* Owner
* Country of Origin
* Farm Name
* Lot Number
* Mill
* Company
* Altitude
* Region
* Processing Method
* Species (Arabica / Robusta)

Start off exploratory data analysis by looking at the usual suspects, df.shape, df.info, df.describe, to get a feel for the columns and data. At this point I start removing any excess columns (for example there was an unnamed column, and also a column that was just the lower case version of another column). It also starts becoming apparent which columns are going to be central to the analysis.



After that I start plotting the distribution of samples according to a number of factors. First thought, let's take a look at the number of samples by country of origin.




Now that we can see how the samples are distributed, I'd like to see how each of these countries fairs by total points, or total cup points.




In addition to species, the samples are also broken down further by variety. Let's see how the distribution of varieties looks amongst the samples.



And just like with the countries of origin, let's see the breakdown of the total point spread by variety.



Now to turn more towards the numerical side, I plotted a correlation heatmap to get a better idea about these measures.


Looking at rubric by which coffee is scored, obvious choices like acidity, aftertaste, body or mouthfeel. Moisture and uniformity I thought were initially good to include but ultimately not as almost all scores in these categories were 10s. Sweetness would also seem important, but again almost all scored 10 so not so much explanatory power. Including defects to check for negative correlation with total points.



# Research on what these defects mean



Narrow down to 'Acidity', 'Aftertaste', 'Aroma', 'Balance', 'Category.One.Defects', 'Category.Two.Defects','Body', 'Flavor', as well as the primary column by which the coffee is ultimately scored, the 'Total.Cup.Points.'




Total Points or Total Cup Points are the primary attribute by which we'll guage the quality of a sample of coffee, so briefly want to look at the distribution of Total Cup Points.




Plotting the distribution of Total Cup Points reveals that it is not exactly normally distributed. It displays excess kurtosis, so would be called a leptokurtic distribution. This refers to the "peak" in the middle and fatter tails as compared to the normal distribution.






# CONFIDENCE INTERVAL ABOUT POPULATION MEAN OF COFFEE





Show globe graphs here. Coffee grown all over the world, what separates coffee from one region from another?
Start with cutting world in two using other CSV of country data: Northern and Southern Hemispheres
For the sake of simplicity (while I acknowledge that some countries are on both hemispheres), separated
by latitudes supplied in the CSV, which came from Google's data (show what these latitudes plot?)



In looking for plotting geographical data from the global perspective, first encountered basemap (which
is apparently deprecated and would not work with currently supported Pandas or Matplotlib), so switched to 
Cartopy package. Using a shapefile and the .reader method to extract and plot the countries of origin.








# Test if coffee from northern and southern hemispheres significantally different
# Null is that the quality of coffee is the same regardless of whether it comes from the Northern or Southern hemispheres
# Alternative is that it matters which hemisphere the coffee comes from


Unfortunately did not explicitly have data regarding in which hemispheres the countries lay, so that was when I imported the second dataset and ultimately merged.





# Mexico by far has supplied the most samples, does this mean Mexican coffee is better or worse than rest of world?
# In other words, could coffee from Mexico be representative of the global quality of coffee?
# Null is Mexican coffee is no different
# Alternative is that it is









# Is coffee grown around 2000 meters statistically significantly better? One tail test
# H0 = There is no difference in quality between samples from around 2000 m and other altitudes
# Ha = Coffee grown at around 2000 meters is statistically significantly better






# poisson distribution? Hypothesis test? Log likelihood
# Use mean and std of total.cup.points to plot pdf or PMF if normal. Hypothesis test if normal?
# Run CLT or boostrap from point stats (plot compared to normal dist with same mean, std)
# What would regression do? goal of regression?





As a sort of summary of the findings, I created a three-dimensional scatter plot showing the relationship between altitude, latitude, and total points.




Finally, I wanted to perform a linear regression on the chosen metrics to see if I could create a fit model to explain the Total Cup Points. I went with the Ordinary Least Squares regression as at least intuitively the simplest choice given the number of parameters.



After the first OLS I decided to try and see if I could improve by removing the variable that seemed to have the least power, and thus retried the OLS but replaced category two defects with mean altitude. However, after plotting the results on top of each other, along with the actual distribution of scores, it does not appear to have meaningfully changed the estimations. Not surprised by that given the extremely high F stat and R2, so may be better suited for a different type of regression. 

If I were to further research, I'd like to find Robusta coffee bean data to add, as well as try predictive regression as these factors are probably highly correlated with each other. I'd also like to delve into the time series data later on for factors like weather, time of year, and harvest years.
