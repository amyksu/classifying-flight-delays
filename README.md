# Rain or Shine: Will your flight be delayed?

A study led by researchers at UC Berkeley estimated that the cost of domestic flight delays puts a $32.9 billion dent into the US economy and about half of that cost is borne by airline passengers. This is a huge problem that affects so many people and having experienced my fair share of delays myself, I wanted to see if there was a way to predict and answer the question, “Will my flight be delayed or not?” 

For my project, I determined an arrival delay of over 30 minutes as the threshold of a flight delay, not including cancellations. 

## Data
To answer this question, I gathered all available flight data for 2019, which at the time was only January and February, from the Bureau of Transportation Statistics, or BTS. The total amount of flights over this time was over 1.1 million flights. The data from the BTS contained all information including, scheduled vs actual arrival and departure time, destination, origin, and airline amongst other information. 

I, then, gathered daily weather information for these two months from DarkSky API. Information from DarkSky included temperature, precipitation type, and amount, and wind speed.  

## EDA
Before tackling my problem, I did some EDA on the data I collected. First, because there were so many flights in my dataset, I wanted to reduce the number by focusing on the top 20 busiest airports. Within the 20 busiest airports, here are the average delay in minutes. 

![Top Airports](https://paper-attachments.dropbox.com/s_F39AC199D2D18BA4069336B797942B2CEBEAE5FD6F8D29B75873518EB6459AC8_1563843094722_image.png)


In addition, I thought it would be interesting to see which airlines had the most delays out of the top 20 busiest airlines. Below, are these 20 airlines and the percent of flights delayed more than 30 minutes.  

![Top Airlines](https://paper-attachments.dropbox.com/s_F39AC199D2D18BA4069336B797942B2CEBEAE5FD6F8D29B75873518EB6459AC8_1563922744434_image.png)

## Feature Engineering and Metrics Selection

Using the data I obtained from the Bureau of Transportation Statistics, I created two new features using the tail number, or the license plate number equivalent, of the aircraft: 

   1. the average delay of the aircraft 
   2. the previous delay time 

For my project, I attempted to maximize the F1 score because both recall and precision were important. Recall is important in this context because if a passenger expects a flight to be on time, but the flight is actually delayed, the passenger could be annoyed that the flight is delayed. On the other hand, precision is important because if a passenger expects the flight to be delayed, goes to the airport late to plan for that and misses their flight, the passenger will also be peeved. 


## Modeling 

Using a subset of my data, I tried a few models such as logistic regression, random forest classifier, and gradient boosting, to name a few. 

![Models](https://paper-attachments.dropbox.com/s_F39AC199D2D18BA4069336B797942B2CEBEAE5FD6F8D29B75873518EB6459AC8_1563920068235_image.png)


As I mentioned in the previous section, because I was focusing on maximizing my F1 score, I landed on gradient boosting using XGBoost because I obtained the highest F1 score. 

Because my classes (delayed flights versus non-delayed flights) were imbalanced, I tried various methods to account for this including undersampling and oversampling using the imblearn library. However, in the end, I increased the scale positive weight paramter to 5 which I determiend by taking the negative (non-delayed flights) and dividing it by the positive value (delayed flights). In the end, I achieved an accuracy score of 0.902 and an F1 score of 0.714.


## Results and Conclusion

Though the results from a gradient boosting model is somewhat of a black box, I used the SHAP library to get some interpretability from the most important features in my model. 


![Results](https://paper-attachments.dropbox.com/s_F39AC199D2D18BA4069336B797942B2CEBEAE5FD6F8D29B75873518EB6459AC8_1563921052381_image.png)


On the left, the most common factors in delays were:

   1. Flying out of Laguardia airport - flights both departing and arriving to the airport were often delayed 
   2. Precipitation of any type - rain or snow - also related to delays
   3. High wind speeds 
   4. A previous delay will also cause a delay
   5. Night flights also tend to be more likely to be delayed 

On the right, these were the most common factors in flights that were less likely to be delayed: 

   1. Delta airlines had the least amount of delays in both my EDA and my model 
   2. Tuesday flights also tend to be less likely to be delayed
   3. Early morning flights also are less likely to be delayed
   4. Hartsfield-Jackson Atlanta Airport and Denver Airport are also the best airports to fly out of. 

## Future Work 
In the future, there are a couple of things that I would like to add to my project. Firstly, the data that I obtained was all domestic data. In the future, it would be cool if I could gather some international flight information and incorporate that into my modeling. In addition, the weather data that I gathered was only daily due to API limitations from the API I used. In the future, I would like to gather more granular data such as hourly weather data to better account for weather in my model. 
