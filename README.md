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
