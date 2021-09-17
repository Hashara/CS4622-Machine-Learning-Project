# CS4622-Machine-Learning-Project <a name="TOP"></a>

<!-- ## Table of Contents
1. [Preprocessng Steps](#Preproocessing_steps) -->

# Exploratory data analysis

## Object columns

In this dataset following features' type are object

```
  'date_recorded', 'funder', 'installer', 'wpt_name', 'basin',
  'subvillage', 'region', 'lga', 'ward', 'public_meeting', 'recorded_by',
  'scheme_management', 'scheme_name', 'permit', 'extraction_type',
  'extraction_type_group', 'extraction_type_class', 'management',
  'management_group', 'payment', 'payment_type', 'water_quality',
  'quality_group', 'quantity', 'quantity_group', 'source', 'source_type',
  'source_class', 'waterpoint_type', 'waterpoint_type_group',
  'status_group'
```
## Integer columns

In this dataset following features' type are integer

```
'id', 'gps_height', 'num_private', 'region_code', 'district_code',
       'population', 'construction_year'
```

## Float columns

In this dataset following features' type are float

```
'amount_tsh', 'longitude', 'latitude'
```

## Unique values of object columns

* Some object columns have large number of unique values
```
funder 1898
installer 2146
wpt_name 37400
subvillage 19288
ward 2092
scheme_name 2697
```
* `recorded_by` column only has one unique value
```
GeoData Consultants Ltd    59400
Name: recorded_by, dtype: int64
```

* Some object columns have small number of unique values
```
basin 9
region 21
lga 125
public_meeting 3
recorded_by 1
scheme_management 13
scheme_name 2697
permit 3
extraction_type 18
extraction_type_group 13
extraction_type_class 7
management 12
management_group 5
payment 7
payment_type 7
water_quality 8
quality_group 6
quantity 5
quantity_group 5
source 10
source_type 7
source_class 3
waterpoint_type 7
waterpoint_type_group 6
status_group 3
```

## Columns with missing values
```
 'funder','installer','subvillage','public_meeting','scheme_management','scheme_name','permit'
```

## Dulplicated rows
* There were no duplicated rows in the dataset

## Correlation

![image](https://user-images.githubusercontent.com/47107459/133598073-2fae4b9b-9e4d-49b1-8ece-89d856e0eea2.png)

* public meeting and permit are highly correlated positively.
* longitude and latitude, source_class and waterpoint_type are negatively correlated


## Label

![image](https://user-images.githubusercontent.com/47107459/133430606-1ff90885-af24-47ef-951f-741afe6498c6.png)

```
functional                 32259
non functional             22824
functional needs repair     4317
```
Since `functional needs repair` have small amount of data, we can say that the dataaset is imbalance.

## observation of columns

### extraction_type
![image](https://user-images.githubusercontent.com/47107459/133431668-5440bbe7-150f-45a1-ae72-e11baae3978a.png)

*  some values have small amount of datapoints

### quantity and quantity_group

* quantity and quantity_group are identical

### Payment
![image](https://user-images.githubusercontent.com/47107459/133777947-79fc6493-15c1-4f15-a6f3-86fe28640529.png)

* when `payment` Is `never pay` or `unknown` more possible to be status _type to `non functional` while other categories are more possible to be `functional`.

### Payment_type
![image](https://user-images.githubusercontent.com/47107459/133780037-f4dbe821-5f94-4bcc-94f9-60b67d025245.png)

* when `payment_ type ` Is `never pay` or `unknown` more possible to be status _type to `non functional` while other categories are more possible to be `functional`.

### water_quality
![image](https://user-images.githubusercontent.com/47107459/133780545-7d63209c-4b2d-4f35-a801-af3b2559dd57.png)

*  when  `water_quality` is `soft` more possible to be `status_type` to `functional`, while `water_quality` is `salty` or`unknown`, `status_type` is more possible to be `non functional`.  
* Even though `water_quality` had 8 categories, 4 categories had less than 500 data points.

### quality_group
![image](https://user-images.githubusercontent.com/47107459/133785022-52980d62-a20e-4dd1-bfac-25bd4ca638ad.png)

* In `quality_group`, When `quality_group` is good, more possible to be `status_type` to `functional`.

### quantity
![image](https://user-images.githubusercontent.com/47107459/133788767-85282223-2efd-4266-b864-7ca9e3fdd2bb.png)

* when `quantity` is `dry` more possible to be `non functional` while `enough` is more possible to be `functional`

### quantity_group
![image](https://user-images.githubusercontent.com/47107459/133789085-1f16b781-7c15-4872-b27b-719fb92cb535.png)

* when quantity_group is dry more possible to be non functional while enough is more possible to be functional.

* `quantity_group` and `quantity` have the same values, They are identical. 

### waterpoint_type
![image](https://user-images.githubusercontent.com/47107459/133792519-7f38dd57-1ae8-4db8-a0bd-4f281df4d802.png)


*   In `waterpoint_type`, even though there were 7 categories, 3 categories had comparatively low number of datapoints

* when `waterpoint_type` is `communal standpipe` or `hand_pump` more possible to be `functional` and when  `waterpoint_type` is `communal standpipe multiple ` or `other` more likely to be `non functional`



### construction_year
* construction_year most values are 0

### amount_tsh
* most values are 0

### gps_height
* most values are 0

###  num_private
* most values are 0

### population
* most values are 0


## EDA summary

* Dataset contains object type columns, integer type columns and float type columns
‘funder’, ‘installer’, ‘wpt_name’,’ subvillage’, ‘ward’,’ scheme_name’ contain higher number of unique values
* ‘recorded_by’ column had one unique values
* `basin`, `region`, `public_meeting`, `recorded_by `,`scheme_management, `scheme_name` `permit`,` extraction_type `,` extraction_type_group `,` extraction_type_class `,`management `,` management_group `,`payment`,` payment_type `,`water_quality `,` quality_group `,` quantity`,` quantity_group`,` source`,` source_type `,` source_class `,`waterpoint_type `, `waterpoint_type_group `, `status_group` contain comparatively lower number of unique values
* 'funder', 'installer', 'subvillage', 'public_meeting', 'scheme_management', 'scheme_name', 'permit' had missing values
* There were no duplicate rows in the dataset
* `public_meeting` and `permit` are highly correlated positively.
* `longitude` and `latitude`, `source_class` and `waterpoint_type` are negatively correlated
* The label `status_type` had 3 categories `functional`, `non functional`, and `functional needs repair` which has 32259, 22824 and 4317 data points respectively. Since `functional needs repair` has a small amount of data, we can say that the dataset is imbalanced.
* Even though `extraction_type` had 18 categories, 7 categories had less than 200 data points while others were having a larger amount of data points.
* In `payment`, when `payment` Is `never pay` or `unknown` more possible to be status _type to `non functional` while other categories are more possible to be `functional`.
* In `payment_type`, when `payment_ type ` Is `never pay` or `unknown` more possible to be status _type to `non functional` while other categories are more possible to be `functional`.
* In `water_quality`, when  `water_quality` is `soft` more possible to be `status_type` to `functional`, while `water_quality` is `salty` or`unknown`, `status_type` is more possible to be `non functional`.  
* Even though `water_quality` had 8 categories, 4 categories had less than 500 data points.
* In `quality_group`, When `quality_group` is good, more possible to be `status_type` to `functional`
* In `quantity`, when `quantity` is `dry` more possible to be `non functional` while `enough` is more possible to be `functional`.
* Also in `quantity_group`, when `quantity_group` is `dry` more possible to be `non functional` while `enough` is more possible to be `functional`.
* `quantity_group` and `quantity` have the same values, They are identical. 
* In `source_class`, `unknown` category has a comparatively small number of data points.
* In `waterpoint_type`, even though there were 7 categories, 3 categories had a comparatively low number of datapoints
* In `waterpoint_type`, when `waterpoint_type` is `communal standpipe` or `hand_pump` more possible to be `functional` and when  `waterpoint_type` is `communal standpipe multiple ` or `other` more likely to be `non functional`
* `construction_year`, `amount_tsh`, `gps_height`. `num_private`, `population` had more zero values








# Preprocessing steps

## Missing values

In the dataset following features had missing values

 1. funder
 2. installer
 3. subvillage
 4. public_meeting
 5. scheme_management
 6. scheme_name
 7. permit
 
### Installer 
* Missing values were filled with the median `DWE`

```
df_train_val.installer = df_train_val.installer.fillna('DWE')
df_test_val.installer = df_test_val.installer.fillna('DWE')
```

* Values with value count < 50 were replced by `Other`
```
df_test_val.loc[df_train_val.groupby('installer').installer.transform('count').lt(50), 'installer'] = "Other" 
df_test_val.loc[df_test_val.groupby('installer').installer.transform('count').lt(50), 'installer'] = "Other"
df_train_val.loc[df_train_val.groupby('installer').installer.transform('count').lt(50), 'installer'] = "Other"    
```
### Other missing values

* Other missing values were droped
```
cols_with_missing = [col for col in df_test_val.columns
                     if df_test_val[col].isnull().any()]
df_train_val = df_train_val.drop(columns = cols_with_missing)
df_test_val = df_test_val.drop(columns = cols_with_missing)
```

# Feature Engineering

## Creation of new features by mathematical transformation

### Number of years
* date recorded and construction year were given as the features
* From the date recorded, recorded year was counted
```
df_train_val["year_recorded"] = df_train_val["date_recorded"].astype(str).str[0:4].astype(int)
df_test_val["year_recorded"] = df_test_val["date_recorded"].astype(str).str[0:4].astype(int)
```
* number of year was calculated from year recorded and contruction year 
```
df_train_val["number_of_years"] = df_train_val["year_recorded"] - df_train_val["construction_year"]
df_test_val["number_of_years"] = df_test_val["year_recorded"] - df_test_val["construction_year"]
```
* Some construction year values were 0, Hence some values of number of year > 2000.
  * Calculated the mean of the values which were < 2000
   ```
   x = pd.DataFrame()
  x["number_of_years"] = df_train_val["number_of_years"]
  x = x.drop(x[x.number_of_years > 2000].index)
  x["number_of_years"].describe()
   ```
  * Replcaed all the values that were > 2000 with the mean calculated
  ```
  df_train_val.loc[df_train_val.number_of_years > 2000, 'number_of_years'] = 15
  df_test_val.loc[df_train_val.number_of_years > 2000, 'number_of_years'] = 15
  ```
  

## Zero value features

### Population
* Most of the values are 0
* 0 Values were replcaed by the mean of `346` the population
  ```
  df_train_val.loc[df_train_val.population < 10, 'population'] = 346
  df_test_val.loc[df_test_val.population < 10, 'population'] = 346
  ```
  
### Construction Year
* Most values are 0
* 0 values were replaced by the median. i.e `2000`
```
median = x.construction_year.median()
df_train_val.loc[df_train_val.construction_year == 10, 'construction_year'] = median
df_test_val.loc[df_test_val.construction_year == 10, 'construction_year'] = median
```

### num_private
* Most va;ues are 0
* 0 values were replcaed by the median, `15`
```
median = x.num_private.median()
df_train_val.loc[df_train_val.num_private == 10, 'num_private'] = median
df_test_val.loc[df_test_val.num_private == 10, 'num_private'] = median
```

## Encoding 

### Ordinal Encoding

* Following features were encoded with ordinanl encoding
```
  basin', 'region', 'lga',  'extraction_type_group', 'management_group', 'payment', 'water_quality', 'quality_group', 'quantity_group', 'source',
  'source_class', 'waterpoint_type', 'installer', 'public_meeting','scheme_management', 'permit'
 ```
### One hot encoding
* One hot encoder used as following
```
one_hot_encoder = OneHotEncoder(handle_unknown='ignore', sparse=False)
````

## Principal component analysis

* Before doing PCA dataset was standadized
```
from sklearn.preprocessing import StandardScaler 

scaler = StandardScaler()
scaler.fit(df_train_clean)
df_train_clean_sc = scaler.transform(df_train_clean)  
```

* PCA done for 2 components
```
from sklearn.decomposition import PCA

pca = PCA(n_components = 2)
pca.fit(df_train_clean_sc)

df_train_clean_pc = pca.transform(df_train_clean_sc) 
```
![image](https://user-images.githubusercontent.com/47107459/133815175-b69f900a-b506-4b6f-8438-e4a94a3a4fbb.png)

* Above graph shows that PCA is not useful in this case



# Models

*  Use `GridSearch` for hyper-parameter tunning, for that used `GridSearchCV` library
*  GradientBoost and XgBoost had comparatively high accuracies
*  MLPClassifer had comparatively low accuracy

## Random Forest
```
RandomForestClassifier(n_estimators=50, max_depth=30, 
                                   min_samples_split=2, min_samples_leaf=3, 
                                   random_state=2, min_weight_fraction_leaf=0.0005)
```
## XgBoost
```
XGBClassifier(max_depth=13,n_estimators=200,learning_rate=0.075)
```

## Discion Tree
```
DecisionTreeClassifier( max_leaf_nodes=600)
```

## Gradient Boost
```
GradientBoostingClassifier(n_estimators=200, learning_rate=0.075, max_depth=15,  min_samples_leaf=15, max_features=0.1)
```

## MLP Classifier
```
MLPClassifier(random_state=0, 
                    max_iter=500,learning_rate='adaptive', alpha=0.0001,
                    activation='relu',hidden_layer_sizes=(21,31,10,))
```



*  Since MLP classifer is sensitive to feature scaling,before training to the MLP classifer scaled the data
  * ```
    from sklearn.preprocessing import StandardScaler 

    scaler = StandardScaler()
    scaler.fit(X_train)
    ``` 

# Post processing

## Permutation Importance
```
PermutationImportance(gd, random_state=1).fit(X_test, y_test)
```
* In permutation importance measure how would affect the accuracy of predictions, If randomly shuffle a single column of the validation data, leaving the target and all other columns in place

![image](https://user-images.githubusercontent.com/47107459/133590441-17fab039-0bf5-4acb-96b3-7232b3e011d6.png)

* According to this the most important feature is quantity_group 
## Partial dependence plot

* Some observation of the partial depenace graph are state here
  * Class 0 = functional
  * Class 1 = functional needs repair
  * Class 2 = non functional

![image](https://user-images.githubusercontent.com/47107459/133736088-9cb5f16d-317c-44e9-98c9-0c0aa075867d.png)

*  According to above graphs when changing the `num_private` feature, the impact of the prediction change is neglegible.

![image](https://user-images.githubusercontent.com/47107459/133736498-47e20fd2-755c-40dd-9b5e-04bcedef1b3e.png)

*  According to the above graph, when changing the `construction_year` feature, it will highly affect to the predictions


### SHAP

* Summay plot

![image](https://user-images.githubusercontent.com/47107459/133745049-0c416429-27e5-4989-bf95-75a7fa0b3265.png)
  *   `num_private` has lower impact on the model
  *   `quantity_group` has the higher impact on the model
* Dependence Contribution Plots


  ![image](https://user-images.githubusercontent.com/47107459/133745268-61be90d8-7830-458e-8cef-b26cbe72e90b.png)
  ![image](https://user-images.githubusercontent.com/47107459/133745610-e9c81069-5825-4b5c-9400-b1ac74a3bb5b.png)

