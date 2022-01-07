# Exploring popular Airbnb bookings for Cape Town





## 1. Business Understanding (The Problem )

Cape Town in South Africa is a popular tourist hotspot for the locals and international travelers. Thsi creates high demand for lodgins around holidays, and high demands create steeper prices for travelers because Airbnb uses a dynamical price allocation model, so in this analysis we will attempt to answer the following questions:

* 1. How do different seasons of the year Influence bookings and prices?
* 2. What is the most popular area for bookings?
* 3. Can we build a reliable price prediction model?


## 2. Data Understanding

In order to be able the questions raised the following analysis will be based on Airbnb data for Cape Town. This data ranges from the year 2012 until 2021. In order to be able to get a full extent of the trends this dataset provides us; locations booked, the price paid, the length of the stay and also stats about stay reviews. This is a good point to start the analysis. 

The dataset has 16,891 observations and 18 columns, 2 columns do not have any entries and will not provide any value for this analysis. 





## 3. Data Preparation

### 3.1 Installations

For the analysis here is a list of required Python packages. 

* ```pip install  folium == 0.12.1 ```
* ```pip install  scikit-learn == 0.24.1  ```
* ```pip install  pandas == 1.2.4 ```
* ```pip install  seaborn == 0.11.1 ```

### 3.2 Dealing with missing data

From the figure below can note that 2 columns do not have any entries and will dropped from the dataframe. And 2 columns have 4771 missing entries.

![Missing data](https://user-images.githubusercontent.com/12718924/148544951-c5b59b0a-cd21-48e3-a476-a52345ca3f23.png)


### 3.3 Data Inspection

Lokking a e first entries of the data, we can visually inspect the columns names and the data types under each columns, there is a method (df.dtypes) can can be called to achive this but this way also provides examples of first few entries. It can be observed that the dataset has a mixture of numerical and categorical values that will aid in ansering the business questions posed at the begining of the analysis. 

![data head](https://user-images.githubusercontent.com/12718924/148545719-a325ead7-283f-4df0-b387-3acea5a9b1c3.png)




## 4. Data Modelling

### 4.1 Seasonal Influence

In order to be bale to visulaize the bookings by season, first we need to extract the month from the booking date, this will be achieved by wrting a function that splits the date in the formmat "2020-02-05" by hyphens and olny returning item on the second index (1 - in Python). With this results now we can pass it in the second function that returns what season that particular month belongs to in South Africa. 

Here is the convention for South African seasons:

- Autumn: March-May
- Winter: June - August
- Spring: September - November
- Summer: December - February 

And now plotting the resulting Seasons agains price paid per night we note that the average booking price starts with Spring with the lowest price of R 1665 and goes up to R2295 in Summer per night.

![Average Price](https://user-images.githubusercontent.com/12718924/148546992-6315b91c-1ae3-4fa7-9b8e-50f36e33d0ab.png)

Investigating percentage price increase using the base price of R1665/Night in Spring. In the graph below it is clear that if you were to book your holiday in Spring for Summer you will save up to 40%. The price increase in Summer is due to the increased demand for accommodation as more people are on holiday over the festive season and most of them have been paid bonus resulting in more readily available holiday spend.

### 4.2 Popular Area in Cape Town


In order to be able to answer this questio we will use the neighbohood column which is a  categorical variable that contain Cape Town's ward numbers. For a start we will aggregate the neighborhood using a groupby method. This will return a count of the instance each ward occured in the dataset, this will allow a percentage calculation that will be basis for judging which area is most, the one with the hights percentage occurance in the dataset. 

![pop ward](https://user-images.githubusercontent.com/12718924/148547901-601cc059-c157-446e-9083-8d172d93e6b0.png)

From the dataset, Ward 115 had almost 20% of the booking followed by ward 54. If you look at the areas that fall under Ward 115 it all makes sense. The vibrant Waterfront is under ward 115; Green point the popular beach that is available to the public, Cape Town city center the hub for business and the newly developed business hub Woodstock to name a few. The visualization below shows ward 115 on the map and the length of the stay, red indicating longer stays and yellow indicating shorter stays.

### 4.3 Building a reliable price prediction model

#### 4.3.1 Feature Engineering

First all categorical variables will be dropped except neighborhood, becuase this variable can be encoded using a label encoder that will return a numerical value that can be used to train a model. 

``` # Create a subset dataset of integers and floats
model_data = airbnb_listing[['minimum_nights', 'number_of_reviews', 'reviews_per_month',
                              'calculated_host_listings_count', 'availability_365', 'number_of_reviews_ltm', 
                             'price', 'year', 'month','ward_code' ]]
                             
                             
# Create a dataframe of dependant variables
X_variables = model_data.drop(['price'], axis=1)

# This is our models target variable
target_variable = model_data['price']
```
#### 4.3.2 Model Training

Out of the 12k observation that remained after dropping the missing entries 80% (9K observations) trained the model and 20% (2K) was used to evaluation. The following was the coefficients of the model. 

![coefficient](https://user-images.githubusercontent.com/12718924/148549519-b46099cf-fac8-46a2-86a3-0ee546b1e1da.png)

Looking at the coefficient table for the variables, it tells apart from the obvious and increase in 1 unit of minimum_nights increase the price by 6 units. The interesting is a unit increase in reviews_per_month has a 33 unit decrease in price.

### 4.3.3 Model Evaluation


![Model_eval](https://user-images.githubusercontent.com/12718924/148549807-ef6c4542-6510-4e3e-97ed-04987bd9ab9e.png)


## 5. Summary 

1. Booking your stay in Summer will cost 40% more than if booked in Spring. 
2. Ward 115 that lies on the coast line is the most popular area in the dataset
3. Using both Mean Square Error (8400982.84) and Root Mean Square Error (2898.44), it can be noted that both these metrics have very high values whereas lower values are desired.

## 6. File in this repo

-  listings.csv: Cape Town Airbnb data
-  capetown_outpy.html: Heatmap plotted using folium to show concentrations of bookings
-  cape_town_airbnb_assignment.ipynb: Code used for the Analysis
-  Image Folder: Contains images on this analysis

##  7. Acknowledgements

* Jeff Heaton Regression Notebook - https://github.com/jeffheaton/t81_558_deep_learning/blob/master/t81_558_class_04_3_regression.ipynb
* StatsSA 2011 Census - https://wazimap.co.za/profiles/ward-19100115-city-of-cape-town-ward-115-19100115/
* Link to the blog post https://www.linkedin.com/pulse/exploring-popular-airbnb-bookings-cape-town-tshepo-moagi
* Data Source, Cape Town Airbnb: http://insideairbnb.com/get-the-data.html
