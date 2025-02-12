# E-Commerce Data Cleaning 
This is to showcase an outlined data cleaning process using both Python and Power Query.

## Dataset: 
A dirty E-commerce data gotten from [kaggle](https://www.kaggle.com/datasets/oleksiimartusiuk/e-commerce-data-shein). It's a folder of 21 category files holding 80,000+ online products altogether.

## Platforms Used:
* Python
* Excel
* Power BI

### NB: Only Power Query shots will be provided here as all reference to python should be checked here. Most issues were detected in python.

## Loading Data:
* Python: Each of the 21 csv files in the dataset folder was loaded individually as a dataframe and stored in a list using the pandas library.
* Excel: Each of the 21 csv files in the dataset folder was loaded as sheets into a single workbook using Virtual Basic script and the micro tool.

VB Script

![image](https://github.com/user-attachments/assets/c76a1517-7f7c-4e73-b6ea-f53adc528417)

The script was saved and then ran over all files in the dataset through micro tool as provided in the following shots.

![image](https://github.com/user-attachments/assets/fca874ac-42b8-459a-b9ea-5cfca2800fdc)
![image](https://github.com/user-attachments/assets/ebf4299a-4665-4289-9697-c4048f826f4c)
![image](https://github.com/user-attachments/assets/51d1e6dc-0cac-44be-b86c-84a9a46722fe)
![image](https://github.com/user-attachments/assets/bc9cd25c-107e-44a5-a231-86b4502cebf1)

This workbook was saved as [allecommerce](https://github.com/MrLIT97/E-Commerce-Data-Cleaning/blob/main/allecommerce.xlsx) and its sheets were loaded into a Power BI for Power Query transformation. Power BI's Power Query was preferred for noticable flexibility (e.g the "enable load" feature)

![image](https://github.com/user-attachments/assets/8c589c40-353d-4713-a351-d41b201cee10)
![image](https://github.com/user-attachments/assets/bcc58ff5-bcfe-425a-87ca-3d3470242bf4)
![image](https://github.com/user-attachments/assets/de3d5db5-b01e-40c4-a0ef-a0ed47d26791)

### Concatenation of all data into a single file:
* Power Query: All 21 sheets imported were concatenated using the "Append Queries" tool. "Append Queries as New" was used to retain original files for reference.
  
![image](https://github.com/user-attachments/assets/59123ea5-6c73-4df8-bbfd-9bfec00407f6)

* Python: All 21 dataframes were concatenated using the ".concated" tool.
  
## Data Assessment:
Exploratory Data Analysis (EDA) was done to assess the data for its completeness, consistency, accuracy, uniqueness, and relevance.
* Python tools: pandas EDA tools.
* Power Query: the filter, column quality, and column profile tools were used.

### Quality Issues identified:
**1. Accuracy Issues**
- Improper naming of the columns - the names "goods-title-link--jump", "rank-title", "rank-sub", and "goods-title-link" do not properly communicate the values of the columns.
  
![image](https://github.com/user-attachments/assets/df08cde3-2580-4a56-8fc4-04d7d1f35e8d)

- Fields were found to be in the wrong data types.

**2. Consistency Issue**
- Unnecessary displacement and repetition of columns - the "goods-title-link" appeared to contain values displaced from "goods-title-link--jump".
![image](https://github.com/user-attachments/assets/749d9b01-77ba-4883-aacf-ddaefe203af6)
![image](https://github.com/user-attachments/assets/9d905443-3c76-46bf-99d4-f02e39e45985)

- In the rank-sub column, certain errors and choice of words like **"Towel" & "Towels" and "Leisure holiday" & "Leisure Vacation"** seemed to cause unnecessary spread of categories. 

**3. Relevance Issues**
- Some columns: "goods-title-link--jump href", "blackfridaybelts-bg src", "blackfridaybelts-content", and "product-locatelabels-img src", were flagged as unrelated with larger percentage of their values missing.
![image](https://github.com/user-attachments/assets/4daf429e-51c2-460c-b93b-5f6bf78c7dae)
![image](https://github.com/user-attachments/assets/f5e9f5ac-c51f-4bdb-9306-666f35e1b6ee)

**4. Completeness Issue**
- Quite a number of missing values were notice across all the fields in the dataset.

**5. Uniqueness Issue**
- Significant traces of duplicates was found in the dataset.
    A total of 6600 duplicates were discovered.

## Data Cleaning:
To achieve a clean dataset, the identified quality issues were targeted and fixed in the following steps.

**1. Fixing Accuracy**
- The columns were renamed appropriately, using the ".remane()" tool in python and by simply editing the headers in Power Query. 
* "goods-title-link - jump" changed to "product name"
* "rank-title" changed to "rank"
* "rank-sub" changed to "subcategory"
* "selling_proposition" changed to "selling proposition"
* "color-count" changed to "color count"
  
![image](https://github.com/user-attachments/assets/8b32e2d2-e878-49aa-a9a2-c6eccc548ad7)
![image](https://github.com/user-attachments/assets/54c70d2e-d760-4e73-829d-21380726f130)


- Special fields required to be integers (whole number) and floats (decimal number) were fixed using the ".astype()" in python while Power Query's "automatic data type detection" was only inspected for its accuracy.
* While doing this, attention was drawn to a value in price field which contains arbitrary "," in-between the figures. This was replaced with whitespace which was then removed using ".str.replace()" followed by .strip().

**2. Fixing Consistency**
- The displaced column was collapsed into the original one using the .fillna() and .drop() tools concurrently in python and the merge tool in Power Query.
  
![image](https://github.com/user-attachments/assets/7ce182bf-ceb0-49a9-9067-15e41cfbe5a6)

- The inconsistency errors in the subcategory field was corrected using the ".str.replace()" tool in python and simple the "replace values" tool in Power Query.
  
![image](https://github.com/user-attachments/assets/92bab775-a64a-43f1-b3f3-e0779d8254f5)

**3. Fixing Relevance**
- The irrelevant fields were removed using the ".drop()" tool in python and the "remove columns" tool in Power Query.
  
![image](https://github.com/user-attachments/assets/18c19e95-8fe4-4130-bcd8-5df1d792a616)

**4. Fixing Completeness**
- The missing values were replaced accordingly using the .fillna() tool in python and the "replace values" in Power Query while the values that could not be randomly fixed were dropped given that they were found to be insignificant junk (less than 1%) of the whole dataset.
* Missing rank were filled with "No Rank"
* Missing subcategory were filled with "No Subcategory"
* Missing discount were filled with "0"
* Missing selling proposition were filled with "Not Specified"
* Missing color count were filled with "1"
* Missing product name and price were removed.
  
![image](https://github.com/user-attachments/assets/515e3eb4-5d14-45fa-991e-45dcf3fa80ee)

**5. Fixing Uniqueness**
- Duplicates covering all fields were first removed using .drop_duplicates in python and "remove duplicated rows" in Power Query.

Row counts before removing duplicates

![image](https://github.com/user-attachments/assets/0adea6a3-e4d0-4a18-a585-194cd09471ae)

Removing duplicates

![image](https://github.com/user-attachments/assets/c8880dcf-f3a5-4563-9a93-825ede97c204)

Row counts after removing duplicates

![image](https://github.com/user-attachments/assets/4960d4ed-4797-49ed-b008-5b939ec3b2d6)

- To further narrow this down, duplicates covering "product name" to "price" were removed.
  
![image](https://github.com/user-attachments/assets/6452fad5-c560-4f43-9a5d-d46249bb7fc5)

Row counts afters removing narrowed duplicates
![image](https://github.com/user-attachments/assets/b9c8a747-6484-45a4-949c-4b478a2ccd71)

### Further cleaning
- The dataset was stripped of whitsepaces and unnecessary characters using ".strip()" tool in python and "trim" and "clean" tools in Power Query.
  
![image](https://github.com/user-attachments/assets/4d7a528b-023c-4110-aff9-5fe2e387a3f5)

- The discount column was stripped of "-" and "%" while the column was renamed to "discount (%)" for the purpose of standardization. The "-" and "%" have automatically been handled by Power Query, it only required setting "discount column" to "Percentage" data type to achieve same result.
  
![image](https://github.com/user-attachments/assets/6e285406-d178-4c9c-b9b6-ed00062356af)


- While checking numerical summary of the dataset at this stage of cleaning, attention was drawn to an error value of "888888.0" posing to be an outlier in the price field (It was easily noticeable in python, and this guided inspection in Power Query). This was filtered out for the purpose of accuracy.

To view this in Power Query, the data was filtered to show records with price > 1000

![image](https://github.com/user-attachments/assets/7992e044-4532-420d-871d-d682ba99e0a1)
![image](https://github.com/user-attachments/assets/42bf1192-7b08-4dc9-9044-9ab8a0d1a305)

This was then filtered out by retaining only records with prive value not equal to "888888"

![image](https://github.com/user-attachments/assets/a266ae95-fd28-44dd-a9ea-5626f81bee9d)

Row counts after cleaning

![image](https://github.com/user-attachments/assets/eba379b2-6b4b-4fd7-8e80-cf70fd7a369b)

To ensure that only the concatenated data named "All Products" is loaded from Power Query, the "Enable load" feature was unchecked for all the base files and checked for only "All Products".

![image](https://github.com/user-attachments/assets/1860e1c0-9c96-4842-a7fa-e26f03f1fb97)
![image](https://github.com/user-attachments/assets/10ac1e9f-c77a-4842-957c-418e9b0559ab)

To break the dataset free from Power BI, the table was copied from the "Data View" and then pasted in a blank workbook in Excel.

![image](https://github.com/user-attachments/assets/acc37e05-b0c5-4c30-be72-67fe9918e97c)

![image](https://github.com/user-attachments/assets/73d3c556-4c1b-4353-91fb-33990ebd4ceb)

This clean data was then saved as xlsx in excel while the equivalent clean data in python was save as csv, which is available [here](https://github.com/MrLIT97/E-Commerce-Data-Cleaning/blob/main/allecommerce_clean.csv).
