# Handling Mistakes and Format Problems

In a [previous tutorial](https://github.com/AEEldin/Pandas-read_CSV), we learned how to use pandas' read_csv() function to load and read the full csv holding your dataset or parts of the file (by rows or columns). In this tutorial, we will discuss using Python's Pandas library to handle mistakes and change the format data columns as essential techniques in cleaning your dataset.


### Part 1: Loading and read your dataset
Assume you have your data stored in a .csv file somewhere on your computer; the first step is to load and read such data. For these tutorials, we will generate data using https://www.mockaroo.com or https://cobbl.io and test the different functions offered by the libraries we imported. Our dataset is named employeeInfo.csv and stores randomly generate information about employees (e.g., names, addresses, salaries, IDs, emails)

```
import pandas as pd

data = pd.read_csv('employeeInfo.csv')    # use pandas library to read the dataset
print(data.info())                        # Print statistical information about the dataset (e.g., the columns names and their data types)
print(data.head())                        # Print the first 5 records of your dataset along with the header
```

### Part 2: Fixing Wrong Data Points
While reading data from your dataset, you may spot some wrong data, which are data points that have some mistakes (e.g., typo, incorrect number) or are not as expected. It is important to note here that wrong data is different from empty data and is also different from correct data in the wrong format. We should find the wrong data points and replace them with the correct values. Assume we have information (e.g., names, ages, emails) about staff members; let's display some info about the dataset and display the first five rows.

```
import pandas as pd

data = pd.read_csv('staffInfo.csv', header=0)   # let's load the dataset

print(data.info())     # let's print statistical info about the dataset
print(data.head())     # let's print the header along with the first 5 rows
```


### Replacing specific rows or data records
If you find that the employee on row 3 has an incorrect email address, you can the "loc" function of pandas with the row number and the column name to directly set the updated value (note: rows start from 0, not 1; so row 3 is the 4th record in your dataset).

```
data.loc[3,'email'] = 'harrison@gmail.com'
print(data.head())
```

### Looping on the dataset data records, and place some if conditions

```
for x in data.index: 
  if data.loc[x, "age"] <0:                     # negative value for age
    print("Wrong age on row:", x)
    data.loc[x, "age"] = 30                     # you may replace the wrong value with 30


  if not data.loc[x,"email"].endswith('.edu'):  # if the email does not end with "edu"
    print("Wrong email on row:", x)
    newEmail = str(data.loc[x, "email"]) + ".edu"
    data.loc[x, "email"] = newEmail             # you may append .edu to the email


  if data.loc[x, "age"] >130:   
    print("Wrong age on row:", x)
    data.drop(x, inplace = True)                # you may remove the full row

  
print(data.head())
```


### Part 3: Adjusting the Format of Columns

The info() function shows the datatype of each column; however, you can change the datatype and the format of some columns


```
data = data.infer_objects()                                                 # Converts object types to possible types
print(data.info())
```

Changing the type of one or more columns, with the option of considering errors
```
data = data.astype({"Salary": float, "ID": int})                     # Change the type of one or multiple columns
data = data.astype({"Salary": float, "ID": int}, errors='ignore')    # Change the type, and ignore erros
data = data.astype({"Salary": float, "ID": int}, errors='raise')     # Change the type, and raise erros
print(data.info())
```

Using the to_numeric and to_datetime to convert the type of certain columns

```
data['Salary'] =pd.to_numeric(data['Salary'])                           # Converts one or more columns to numeric type
print(data.info())
                                               
data['Start Date'] = pd.to_datetime(data['Start Date'])                 # Convert one or more columns to datetime type
print(data['Start Date'])
```

For the to_datetime, you can use the strftime() function to specify the format of the datetime data type
```
data['Start Date']=data['Start Date'].dt.strftime('%d-%m-%Y')      
data['Start Date']=data['Start Date'].dt.strftime('%Y/%m/%d')      
data['Start Date']=data['Start Date'].dt.strftime('%y%m%d')
print(data.head())
```

