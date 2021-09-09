# CS4622-Machine-Learning-Project <a name="TOP"></a>

<!-- ## Table of Contents
1. [Preprocessng Steps](#Preproocessing_steps) -->


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

## New features

# Number of years
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
* 
