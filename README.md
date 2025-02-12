# E-Commerce Data Cleaning 
This is to showcase an outlined data cleaning process using both Python and Power Query.

Dataset: 
A dirty E-commerce data gotten from [kaggle](https://www.kaggle.com/datasets/oleksiimartusiuk/e-commerce-data-shein). It's a folder of 21 category files holding 80,000+ online products altogether. 

Loading Data:
Python: Each of the 21 csv files in the dataset folder was loaded individually as a dataframe and stored in a list using the pandas library.
Excel: Each of the 21 csv files in the dataset folder was loaded as sheets into a single workbook using Virtual Basic script and the micro tool.

Data Assessment:
Exploratory Data Analysis (EDA) was done to assess the data for its completeness, consistency, accuracy, uniqueness, and relevance.
Python tools: pandas EDA tools.
Power Query: the filter, column quality, and column profile tools were used.
The following quality issues were discovered:
Accuracy Issues
Improper naming of the columns - the names goods-title-link - jump, rank-title, rank-sub, and goods-title-link do not properly communicate the values of the columns. 
Fields were found to be in the wrong data types

Consistency Issue
Unnecessary displacement and repetition of columns - the goods-title-link appeared to contain values displaced from goods-title-link - jump.
In the rank-sub column, certain errors and choice of words like "Towel" & "Towels" and "Leisure holiday" & "Leisure Vacation" seemed to cause unnecessary spread of categories. 

Relevance Issues
Some columns: [list the columns], were flagged as unrelated to the context of analysis.

Completeness Issue
Quite a number of missing values were notice across all the fields in the dataset.

Uniqueness Issue
Some records in the dataset were found to be repeated

Data Cleaning:
To achieve a clean dataset, the identified quality issues were targeted and fixed in the following steps.

Fixing Accuracy
The columns were renamed appropriately, using the .remane() tool in python and by simply editing the headers in Power Query. 
"goods-title-link - jump" changed to "product name"
"rank-title" changed to "rank"
"rank-sub" changed to "subcategory"
"selling_proposition" changed to "selling proposition"
"color-count" changed to "color count"

Special fields required to be integers (whole number) and floats (decimal number) were fixed using the .astype() in python while Power Query's automatic data type detection was only inspected for its accuracy.
While doing this, attention was drawn to a value in price field which contains arbitrary "," in-between the figures. This was replaced with whitespace which was then removed using .str.replace() followed by .strip().

Fixing Consistency
The displaced column was collapsed into the original one using the .fillna() and .drop() tools concurrently in python and the merge tool in Power Query.
The inconsistency errors in the subcategory field was corrected using the ".str.replace()" tool in python and simple the "replace values" tool in Power Query.

Fixing Relevance 
The irrelevant fields were removed using the ".drop()" tool in python and the "remove columns" tool in Power Query.

Fixing Completeness
The missing values were replaced accordingly using the .fillna() tool in python and the "replace values" in Power Query while the values that could not be randomly fixed were dropped given that they were found to be insignificant junk (less than 1%) of the whole dataset.
Missing rank were filled with "No Rank"
Missing subcategory were filled with "No Subcategory"
Missing discount were filled with "0"
Missing selling proposition were filled with "Not Specified"
Missing color count were filled with "1"
Missing product name and price were removed.

Fixing Uniqueness 
Duplicates covering all fields were first removed using .drop_duplicates in python and "remove duplicated rows" in Power Query.
To further narrow this down, duplicates covering products name to discount were removed.

Further cleaning
The dataset was stripped of whitsepaces and unnecessary characters using .strip() tool in python and trim and clean tools in Power Query. 
The discount column was stripped of "-" and "%" while the column was renamed to "discount (%)" for the purpose of standardization. 
While checking numerical summary of the dataset at this stage of cleaning, attention was drawn to an error posing to be an outlier in the price field. This was filtered out for the purpose of accuracy.