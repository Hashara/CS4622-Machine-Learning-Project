# CS4622-Machine-Learning-Project: Pump it Up: Data Mining the Water Table<a name="TOP"></a>

<!-- ## Table of Contents
1. [Preprocessng Steps](#Preproocessing_steps) -->

### files description
* [All_implementations.ipynb](https://github.com/Hashara/CS4622-Machine-Learning-Project/blob/main/All_implementations.ipynb) - Contains all the implementation done
* [Model with highest accuracy.ipynb](https://github.com/Hashara/CS4622-Machine-Learning-Project/blob/main/Model%20with%20highest%20accuracy.ipynb) - Highest accuracy implementation
* [EDA.ipynb](https://github.com/Hashara/CS4622-Machine-Learning-Project/blob/main/EDA.ipynb) - contains Exploratory data analysis
* [EDA and post processing.md](https://github.com/Hashara/CS4622-Machine-Learning-Project/blob/main/EDA%20and%20post%20processing.md) - description of EDA and post processing
---
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
### scheme_management
* Missing values were filled with the median `VWC`
```
df_train_val.scheme_management = df_train_val.scheme_management.fillna('VWC')
df_test_val.scheme_management = df_test_val.scheme_management.fillna('VWC')
```
* `Trust` and `SWC` was rename to `Other`, becasue thses two did not had many data points
```
df_train_val.loc[df_train_val.scheme_management == 'Trust', 'scheme_management'] = 'Other'
df_test_val.loc[df_test_val.scheme_management == 'Trust', 'scheme_management'] = 'Other'

df_train_val.loc[df_train_val.scheme_management == 'SWC', 'scheme_management'] = 'Other'
df_test_val.loc[df_test_val.scheme_management == 'SWC', 'scheme_management'] = 'Other'
```

### permit
* Binary type column
* Replaced with median, `True`
```
df_train_val.permit = df_train_val.permit.fillna(True)
df_test_val.permit = df_test_val.permit.fillna(True)
```

### public_meeting
* Binary type column
* Replaced with median, `True`
```
df_train_val.public_meeting = df_train_val.permit.fillna(True)
df_test_val.public_meeting = df_test_val.permit.fillna(True)
```

### Other missing values

* Other missing values were droped
```
cols_with_missing = [col for col in df_test_val.columns
                     if df_test_val[col].isnull().any()]
df_train_val = df_train_val.drop(columns = cols_with_missing)
df_test_val = df_test_val.drop(columns = cols_with_missing)
```
## Under smapling and over sampling
* Under sampling with `RandomUnderSampler`
```
undersample = RandomUnderSampler()
X_train_undersample, y_train_undersample = undersample.fit_resample(X_train, y_train)
X_train_undersample = pd.DataFrame(X_train_undersample, columns=X_train.columns)
```
* Over sampling with `SMOTE`
```
oversample = SMOTE("minority")
X_train_oversampled, y_train_oversampled = oversample.fit_resample(X_train, y_train)
X_train_smote = pd.DataFrame(X_train_oversampled, columns=X_train.columns)
```
* Over smapling with `RandomOverSampler`
```
oversample = RandomOverSampler(sampling_strategy='minority')
X_train_oversampled, y_train_oversampled = oversample.fit_resample(X_train, y_train)
X_train_oversampled = pd.DataFrame(X_train_oversampled, columns=X_train.columns))
```
* Both over sampling and under sampling with using `Pipeline`
```
over = SMOTE("minority")
under = RandomUnderSampler()

steps = [('o', over), ('u', under)]
pipeline = Pipeline(steps=steps)

X_train_pip, y_train_pip = pipeline.fit_resample(X_train, y_train)
X_train_pip = pd.DataFrame(X_train_pip, columns=X_train.columns)
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
* Following features, which have binary values, were encoded with onehot encoding
```
'public_meeting', 'permit'
```

## Hyper parameeter tunning
* For hyper parameter tunning `GridSearchCV` was used
```
from sklearn.model_selection import GridSearchCV
def rfmodel(X_train, X_val, y_train, y_val):
    if __name__ == '__main__':
    
        param_grid = {'bootstrap': [True, False],
                    'max_depth': [10, 20, 30, 40, 50, 60, 70, 80, 90, 100, None],
                    'max_features': ['auto', 'sqrt'],
                    'min_samples_leaf': [1, 2, 4],
                    'min_samples_split': [2, 5, 10],
                    'n_estimators': [200, 400, 600, 800, 1000, 1200, 1400, 1600, 1800, 2000]}                   

        estimator = GridSearchCV(estimator=RandomForestClassifier(),
                                 param_grid=param_grid,
                                 n_jobs=-1)

        estimator.fit(X_train, y_train)

        best_params = estimator.best_params_

        print (best_params)
                                 
        validation_accuracy = estimator.score(X_val, y_val)
        print('Validation accuracy: ', validation_accuracy)
```
---

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

---

# Feature selection
* Following features were selected to training the model
```
'gps_height', 'longitude', 'latitude', 'num_private',
'basin', 'region', 'district_code', 'lga', 'population',
'construction_year', 'extraction_type_group', 'management_group',
'payment', 'water_quality',  'quantity_group', 'source',
'waterpoint_type'
```
---


