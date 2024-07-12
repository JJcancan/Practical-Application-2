# Practical-Application-2
**Business Understanding**
The goal is to provide your dealership with better information on how to optomize pricing for car sales based on attributes of the product. This will enable you to better list and sell inventory. To do so, a predictive model using industry used car data will be preprocessed and with conducted feature engineering, this can provide a predictive model.

**Data Understanding**
- Data comprises of 426880 total rows and 18 total columns.
- ![image](https://github.com/user-attachments/assets/34d09ce7-8a34-4bd3-9bf8-78f0e3695d5c)
- Size has more than 50% of the its data missing.
- Location data (region, state) may not be useful information as your dealership is in one state.
- Vehicle ID and VIN may also not be useful as these attributes have nothing to do with the price of the car
- Model offers 29649 unique entries which may not be useful due to the variety and specificity of the data.
- **Useful Categorical Features**
  - **Manufacturer**
    - ![image](https://github.com/user-attachments/assets/1a437dae-c545-4435-9dbd-c7cf32fcf29a)
    - 42 Unique Manufacturers
  - **Condition**
    - ![image](https://github.com/user-attachments/assets/8294662d-8522-4c83-9c82-11d95003f925)
    - 6 Unique Conditions
    - Highest price average comes from fair condition cars
    - Most cars are in good condition
  - **Cylinders**
    - ![image](https://github.com/user-attachments/assets/a1beba41-1c2b-401d-a4a0-a7f437d827cd)
    - Highest price average came from 8 cylinder cars
    - Most cars have a 6, 4, 8 cylinder enginer
  - **Fuel**
    - ![image](https://github.com/user-attachments/assets/263b4a4e-ee31-4413-bb22-6094b96367ec)
    - Highest average price are for diesel fuel cars
    - Most used cars have a gas fuel type
  - **title_status**
    - ![image](https://github.com/user-attachments/assets/80ef9a31-cdeb-43dd-b33c-b64b974b4a49)
    - Highest average price and most frequent title statuses are clean titles
  - **Drive**
    -  ![image](https://github.com/user-attachments/assets/1f9e4199-a42a-4aa8-bcf5-b6e5c6183cc5)
    -  Highest average price and most frequent drive train are four wheel drive vehicles
  - **Type**
    - ![image](https://github.com/user-attachments/assets/d108bdfa-cb5a-49db-9da9-cce46595ce9b)
    - Pickup trucks had highest average price but a majority of data were SUVs and Sedans
  - **Paint Color**
    - ![image](https://github.com/user-attachments/assets/91ca571e-9226-405d-8f11-5c967cf2a04b)
    - Highest average price car were green cars but were fairly low in unique counts (some of these cars may be luxury cars)
- **Numerical Features**
  - Year
    - ![image](https://github.com/user-attachments/assets/916e0c97-c72e-44b3-b4a9-7c3c70b09fd8)
    - Majority of data is from 1990 onwards
  - Odometer
    - ![image](https://github.com/user-attachments/assets/6bbeaeb2-8030-42b6-b53f-3f31974f8682)
    - Large outliers shown
    - Majority of data less than 300K miles, also odometer readings at 0 miles are not realistic and will be removed as well
  - Price
    - Target Variable
    - Outliers and unrealistic values as car prices range from 0 to 3,736,928,711 USD
    - Majority of data is between 1000 and 150K USD
    - ![image](https://github.com/user-attachments/assets/e22686dc-2651-4d15-a329-a99307c2aa77)
    - Data seems to be right skewed transforming the target variable to a log scale may help the distribution
- **Conclusion**
  - Some categorical features contain numerous low freq populated categories or the category has low variety (i.e. fuel where most are gas cars)
    - We can consider dropping some of these features but should first only remove attributes that we assume do not drive price and features that have insufficient data
  -	I start with removing the features that may have useful info related to price like region, Id, state
  -	Size has more than 50% of its data as missing, this should be considered to drop
  -	Model, as stated previously, should be dropped as well

**Data Preparation**-
- Data Cleaning
  - 'region', 'id', 'state', 'VIN', 'model', 'size' columns were dropped due to usefulness
  -	Missing values were dropped after the columns above were dropped
-	Data Filtering
  -	After dropping missing values and columns mentioned above data was filtered to remove outliers in numeric variables
  -	Price filtered from 1K-150KUSD
  -	Odometer filtered from 1-300K miles
  -	Year was filtered from anything 1990 and onward
- Conclusion
  - Data was cut down to 117169 rows and 12 columns
  - Category columns will have to be encoded using a one hot encoder before being inputted into the model
 
**Modelling**
- Preprocessing Data
  - As mentioned above a one hot encoder is used to represent categorical variables as numerical ones.
  - A standard scaler helps to shift and scale odometer and year inputs as these distributions seemed to be skewed.
-	Cross validation
  -	RidgeCV with various alphas were used to find optimal alpha value to feed into regression models
    -	An alpha of 0.1 was found to be the best option for our model
-	Regression Models
  -	Ridge and LASSO regression models were used to construct our prediction model
  -	A root mean squared error (RMSE) and the coefficient of determination (R2) are used to evaluate our models.
    -	RMSE tells us the average difference between the actual and expected results
    -	R2 explains the proportion of variance in the dependent variable that can be determined by the independent variable.
  -	LASSO Results
    -	Lasso: Test RMSE = 9672.7710, R2 = 0.4628
    -	Lasso: Train RMSE = 9761.4691, R2 = 0.4604
  -	Ridge Results
    -	Ridge: Test RMSE = 6339.6408, R2 = 0.7693
    -	Ridge: Train RMSE = 6359.9943, R2 = 0.7710
-	Conclusion
  -	The Ridge model with preprocessing and cross validation applied served the best result
  -	The model explained ~77% of the variance in the dataset
  -	The training RMSE was about 6,300 off from target and could be better if given data had missing values filled
  -	The training RMSE and test RMSE were fairly close to each other meaning that we did not run into overfitting or underfitting

**Deployment**
- Business Goal
  - We wanted to predict used car prices using attributes that are common knowledge for your used car dealership.
  -	This could provide better inventory management and more accurate selling prices
-	Methods
  -	After exploring and cleaning the dataset provided, Ridge and Lasso regression models were evaluated
  -	It was found that Ridge regression was the best fit for this application
-	Results
  - ![image](https://github.com/user-attachments/assets/e991b378-8528-4dee-beba-c13dd177a500)
  - Top coefficient values are depicted some key notes as year increases or the more recent a car model the likely higher the price
  - Aston martins, Teslas, Porsches, and Ferraris have a higher price which is understandable knowing the price of their fleet of models. But as a used car dealership these may be a hard sell.
  - Saturns, Mercury, and Fiats seem to not be great makes to keep in stock
  - Odometer increases, the price seems to decrease.
  - The model resulted in a R-squared score of 77% which means our model can explain ~77% of the variability in data.
- Recommendations 
  -	Certain manufacturers do not do well in price advised to keep Mercurys, Saturns, Fiats, and Chryslers off inventory
  -	Most cars should be within the last couple years the more recent the car the higher the price.






  

 






