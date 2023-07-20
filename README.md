<p align="center">
<br/>
<img src="https://i.imgur.com/bOnVI78.jpg" height="65%" width="65%" alt="blueberries"/>
<br />

# Blueberry Yield Prediction

*Prediction of wild blueberry yield based on several factors, including plant spatial arrangement, outcrossing and self-pollination, bee species compositions and weather conditions. This project was completed as participation in a 2023 Kaggle competition.*

## 1. Data

The data we are using is publicly available and can be found on Kaggle under Wild Blueberry Yield Prediction Dataset. The dataset is a collection of numerical features that describe a crop of wild blueberries. The features include numerical descriptors about bumblee population, air temperature, rain conditions, and plant spatial arrangement. The full data can be found here:

> * [Wild Blueberry Yield Prediction Dataset](https://www.kaggle.com/datasets/shashwatwork/wild-blueberry-yield-prediction-dataset)

## 2. Data Cleaning and EDA

Given that this data is used for a competition, the data is very clean already. Furthermore, all of the features are of appropriate types and there are no missing values. The only question marks with the data are with a few outliers. The biggest of outliers being in the 'honeybee' column. There are also less extreme outliers in the 'bumbles', 'andrena', 'osmia', 'RainingDays', 'AverageRainingDays', 'fruitset', 'fruitmass' and 'seeds'. We know that the 'honeybee' column represents the honeybee density in the field. However, we do not know enough about the data to know if the outliers are sensible or not. For now, we'll leave them in but maybe remove them later in the modeling phase. Also, for the other (less extreme) outliers, we will leave them for now as they don't seem to be as a result of data errors. Boxplots of the features can be seen below and the outliers in the honeybee columns can be clearly seen. 

<p align="center">
<br/>
<img src="https://i.imgur.com/MGgYnGO.png" height="50%" width="50%" alt="boxplot"/>
<br />

Next, we explored the features and plotted a correlation plot. We can see that several variables are highly (and negatively) correlated with yield. Moreover, 'fruitset', 'fruitmass', and 'seeds' are highly correlated with yield. This is not a huge surpise, as these are the features that are directly correlated with blueberry yield. However, there are a few other features that are strongly negatively correlated with yield. These features include 'clonesize', 'RainingDays', and 'AverageRainingDays'.

<p align="center">
<br/>
<img src="https://i.imgur.com/4nh0gNP.png" height="60%" width="60%" alt="heatmap"/>
<br />

Next, we plotted a pairplots of the features, colored by blueberry yield to further investigate characteristics and the distribution of yield within each variable. We can see that there are some trends in the yield distributions. In the histograms (plotted on the diagonal), we can see for most features, some sort of separation in the yield values. For example, there tends to be less yield with lower fruitset values. Or on the other hand, a higher yield with a lower clonesize.

Also, we added two additional variables that can be seen in these plots: seed_fruitset and bees. The first variable combines fruitset and seeds since they are important to yield. The second feature combines all of the "bee" features into one. 

<p align="center">
<br/>
<img src="https://i.imgur.com/Bnujlkd.png" height="80%" width="80%" alt="pairplot"/>
<br />

## 3. Preprocessing

In the preprocessing stage, we prepared three different datasets for modeling. The first dataset is the full data, therefore there is nothing to do here besides splitting into training and testing sets. For the second dataset, we hand selected features based on their correlation in the plots above. The features we selected were: fruitset, fruitmass, seeds, clonsesize, raining days, average raining days, seed fruitset, and bees. Furthermore, the first two datasets were properly scaled using sklearn StandardScaler(). Lastly, we performed PCA on the data and created a new dataset with the first 6 principal components that explained approximately 99% of the total variance. 

## 4. Modeling

Now for the modeling phase! We have 3 sets of data to test and compare. I am going to train/test a Random Forest, XGBoost, CatBoost, and LightGBM Regressors. For each data and regressor combination, we will perform parameter tuning and generate the training set mean absolute error (MAE). Because this project was done for a Kaggle competition, we were not provided with testing labels. Therefore, the predictions of what we think are the best models were submitted to the Kaggle competition to compute the testing set MAE.

The top 4 model's modeling results can be found in the Table below.

<p align="center">
<br/>
<img src="https://i.imgur.com/d8VrNiS.png" height="40%" width="40%" alt="results"/>
<br />

**Discussion and Conclusion**:

The method that Kaggle uses for their competitions is that they generate a testing score on some of the data during the competition, and then they create a final score on the remaining data at the deadline.

Given that all training errors are less than the testing errors, there has been some overfitting in the models. This can especially be seen with the LightGBM model with the filtered data as this model has the lowest training MAE but the highest testing MAE of the sample.

Of the four regression models tested in this project, it is clear from the results that CatBoost and LightGBM were most succesful in their models. Furthermore, both the full data and the filtered data performed similarly where the full data had a very slight edge in MAE. However, the time to train and test models with the full data is slightly longer given the number of features; therefore, the marginal decrease in MAE may be defeated by the time it takes for the full data. Also, the PCA models never competed with the other data sets. It was a good idea in theory, however, the MAE were never good enough to move forward with.

The final testing MAE for the best model was 334.9 which used the CatBoost model with the full dataset. Therefore, our testing results did improve by a few points when trained on their final testing set. 

Overall, this was an enjoyable competition with very little separating the highest performers. My model came within 7.5 points with the winner and I am satisfied with finishing in the top 25%. In future work, I believe that parameter tuning and feature engineering is what can improve models the most in competitions like this. Therefore, I would spend more time with these tasks to achieve the best score that I can. 

