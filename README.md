# Final-Project-Statistical-Modelling-with-Python

## Project / Goals
In this project I set out to **compare the “point-of-interest” (POI) coverage** provided by CityBikes, Foursquare and Yelp around a set of bike‐share stations, and then to **build simple models** to see whether POI density helps explain or predict the number of free bikes at each station. My main questions were:
- Which API provides the richest, most reliable POI data in my area?
- Is there a measurable relationship between how “busy” an area looks (POIs) and actual bike availability?
- Can a regression or classification model turn POI counts into a quick availability forecast?

## Process
1. **Data collection & integration**  
   - Pulled live station status (free_bikes, empty_slots, location) from the CityBikes API.  
   - Queried Foursquare and Yelp for nearby POIs (restaurants), limiting to 5 per station within 1 km.  
   - Parsed and flattened each JSON response into a unified DataFrame, then joined back to station data.

2. **Exploratory analysis & modeling**  
   - Visualized POI counts vs. free bike counts to check for obvious trends.  
   - Built an **OLS regression** (statsmodels) to quantify the linear relationship.  
   - Turned free bikes into a binary “high vs. low availability” flag (above/below median) and fit a **logistic regression** (scikit-learn).  
   - Evaluated model performance with R², p-values, classification report, and ROC–AUC.

## Results
- **API coverage:** Foursquare returned more consistently geolocated and formatted POI records than Yelp in this region, though both suffered occasional rate‐limit errors.  
- **Regression:** POI count explained only ~0.4 % of variance in free bikes (R² ≈ 0.004), and the negative slope (–0.83 bikes per POI) was _not_ statistically significant (p ≈ 0.33).  
- **Classification:** A single‐feature logistic model achieved ~54 % accuracy, ROC–AUC ≈ 0.51—barely better than random. Precision/recall for “low availability” were undefined, since the model never predicted that class.

## Challenges
- **Rate‐limit throttling** on both Foursquare and Yelp required adding delays and retry logic.  
- **Inconsistent JSON schemas:** location fields and nested keys changed slightly between APIs, so parsing had to be robust to missing data.  
- **Small sample size** (≈260 stations) and single POI category (restaurants) limited the signal.  
- **Temporal dynamics:** Station availability fluctuates by hour & day, but my snapshot approach ignored time series effects.

## Future Goals
- **Add more features**: include empty_slots, station capacity, day-of-week, weather, and transit stops.  
- **Geospatial modeling**: use KDE or spatial autocorrelation (e.g. Moran’s I) rather than raw counts.  
- **Advanced ML**: try tree-based or neural models to capture non-linear patterns.  
- **Larger dataset**: collect multiple snapshots throughout the day to build a time-aware model.  
- **Expand POI scope**: query additional categories (cafés, bars, transit hubs) or use OpenStreetMap for fuller coverage.
