<p style="text-align:center">
    <a href="https://skills.network" target="_blank">
    <img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/assets/logos/SN_web_lightmode.png" width="200" alt="Skills Network Logo">
    </a>
</p>

<h1 align=center><font size = 5>Assignment: SQL Notebook for Peer Assignment</font></h1>

Estimated time needed: **60** minutes.

## Introduction
Using this Python notebook you will:

1.  Understand the Spacex DataSet
2.  Load the dataset  into the corresponding table in a Db2 database
3.  Execute SQL queries to answer assignment questions 


## Overview of the DataSet

SpaceX has gained worldwide attention for a series of historic milestones. 

It is the only private company ever to return a spacecraft from low-earth orbit, which it first accomplished in December 2010.
SpaceX advertises Falcon 9 rocket launches on its website with a cost of 62 million dollars wheras other providers cost upward of 165 million dollars each, much of the savings is because Space X can reuse the first stage. 


Therefore if we can determine if the first stage will land, we can determine the cost of a launch. 

This information can be used if an alternate company wants to bid against SpaceX for a rocket launch.

This dataset includes a record for each payload carried during a SpaceX mission into outer space.


### Download the datasets

This assignment requires you to load the spacex dataset.

In many cases the dataset to be analyzed is available as a .CSV (comma separated values) file, perhaps on the internet. Click on the link below to download and save the dataset (.CSV file):

 <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/labs/module_2/data/Spacex.csv" target="_blank">Spacex DataSet</a>




```python
!pip install sqlalchemy==1.3.9

```

    Collecting sqlalchemy==1.3.9
      Downloading SQLAlchemy-1.3.9.tar.gz (6.0 MB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m6.0/6.0 MB[0m [31m76.2 MB/s[0m eta [36m0:00:00[0m:00:01[0m00:01[0m
    [?25h  Preparing metadata (setup.py) ... [?25ldone
    [?25hBuilding wheels for collected packages: sqlalchemy
      Building wheel for sqlalchemy (setup.py) ... [?25ldone
    [?25h  Created wheel for sqlalchemy: filename=SQLAlchemy-1.3.9-cp37-cp37m-linux_x86_64.whl size=1159121 sha256=fd1701ce790ac7ef1ba485ab145fd9daa701b0ce6beb0c71c7075aca97fd66f9
      Stored in directory: /home/jupyterlab/.cache/pip/wheels/03/71/13/010faf12246f72dc76b4150e6e599d13a85b4435e06fb9e51f
    Successfully built sqlalchemy
    Installing collected packages: sqlalchemy
      Attempting uninstall: sqlalchemy
        Found existing installation: SQLAlchemy 1.3.24
        Uninstalling SQLAlchemy-1.3.24:
          Successfully uninstalled SQLAlchemy-1.3.24
    Successfully installed sqlalchemy-1.3.9


### Connect to the database

Let us first load the SQL extension and establish a connection with the database



```python
#Please uncomment and execute the code below if you are working locally.

#!pip install ipython-sql
```


```python
%load_ext sql
```


```python
import csv, sqlite3

con = sqlite3.connect("my_data1.db")
cur = con.cursor()
```


```python
!pip install -q pandas==1.1.5
```


```python
%sql sqlite:///my_data1.db
```




    'Connected: @my_data1.db'




```python
import pandas as pd
df = pd.read_csv("https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/labs/module_2/data/Spacex.csv")
df.to_sql("SPACEXTBL", con, if_exists='replace', index=False,method="multi")
```

    /home/jupyterlab/conda/envs/python/lib/python3.7/site-packages/pandas/core/generic.py:2882: UserWarning: The spaces in these column names will not be changed. In pandas versions < 0.14, spaces were converted to underscores.
      both result in 0.1234 being formatted as 0.12.


**Note:This below code is added to remove blank rows from table**


**Note:This below code is added to remove blank rows from table**



```python
%sql create table SPACEXTABLE as select * from SPACEXTBL where Date is not null
```

     * sqlite:///my_data1.db
    Done.





    []



## Tasks

Now write and execute SQL queries to solve the assignment tasks.

**Note: If the column names are in mixed case enclose it in double quotes
   For Example "Landing_Outcome"**

### Task 1




##### Display the names of the unique launch sites  in the space mission



```python
%sql SELECT DISTINCT LAUNCH_SITE FROM SPACEXTBL
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>Launch_Site</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>CCAFS LC-40</td>
        </tr>
        <tr>
            <td>VAFB SLC-4E</td>
        </tr>
        <tr>
            <td>KSC LC-39A</td>
        </tr>
        <tr>
            <td>CCAFS SLC-40</td>
        </tr>
    </tbody>
</table>




### Task 2


#####  Display 5 records where launch sites begin with the string 'CCA' 



```python
%sql SELECT * FROM SPACEXTBL WHERE LAUNCH_SITE LIKE 'CCA%' LIMIT 5
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>Date</th>
            <th>Time (UTC)</th>
            <th>Booster_Version</th>
            <th>Launch_Site</th>
            <th>Payload</th>
            <th>PAYLOAD_MASS__KG_</th>
            <th>Orbit</th>
            <th>Customer</th>
            <th>Mission_Outcome</th>
            <th>Landing_Outcome</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2010-06-04</td>
            <td>18:45:00</td>
            <td>F9 v1.0  B0003</td>
            <td>CCAFS LC-40</td>
            <td>Dragon Spacecraft Qualification Unit</td>
            <td>0</td>
            <td>LEO</td>
            <td>SpaceX</td>
            <td>Success</td>
            <td>Failure (parachute)</td>
        </tr>
        <tr>
            <td>2010-12-08</td>
            <td>15:43:00</td>
            <td>F9 v1.0  B0004</td>
            <td>CCAFS LC-40</td>
            <td>Dragon demo flight C1, two CubeSats, barrel of Brouere cheese</td>
            <td>0</td>
            <td>LEO (ISS)</td>
            <td>NASA (COTS) NRO</td>
            <td>Success</td>
            <td>Failure (parachute)</td>
        </tr>
        <tr>
            <td>2012-05-22</td>
            <td>7:44:00</td>
            <td>F9 v1.0  B0005</td>
            <td>CCAFS LC-40</td>
            <td>Dragon demo flight C2</td>
            <td>525</td>
            <td>LEO (ISS)</td>
            <td>NASA (COTS)</td>
            <td>Success</td>
            <td>No attempt</td>
        </tr>
        <tr>
            <td>2012-10-08</td>
            <td>0:35:00</td>
            <td>F9 v1.0  B0006</td>
            <td>CCAFS LC-40</td>
            <td>SpaceX CRS-1</td>
            <td>500</td>
            <td>LEO (ISS)</td>
            <td>NASA (CRS)</td>
            <td>Success</td>
            <td>No attempt</td>
        </tr>
        <tr>
            <td>2013-03-01</td>
            <td>15:10:00</td>
            <td>F9 v1.0  B0007</td>
            <td>CCAFS LC-40</td>
            <td>SpaceX CRS-2</td>
            <td>677</td>
            <td>LEO (ISS)</td>
            <td>NASA (CRS)</td>
            <td>Success</td>
            <td>No attempt</td>
        </tr>
    </tbody>
</table>



### Task 3




##### Display the total payload mass carried by boosters launched by NASA (CRS)



```python
%sql SELECT SUM(PAYLOAD_MASS__KG_) FROM SPACEXTBL WHERE CUSTOMER='NASA (CRS)'
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>SUM(PAYLOAD_MASS__KG_)</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>45596</td>
        </tr>
    </tbody>
</table>



### Task 4




##### Display average payload mass carried by booster version F9 v1.1



```python
%sql SELECT AVG(PAYLOAD_MASS__KG_) FROM SPACEXTBL WHERE BOOSTER_VERSION='F9 v1.1'
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>AVG(PAYLOAD_MASS__KG_)</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2928.4</td>
        </tr>
    </tbody>
</table>



### Task 5

##### List the date when the first succesful landing outcome in ground pad was acheived.


_Hint:Use min function_ 



```python
%sql SELECT min(DATE) FROM SPACEXTBL WHERE LANDING_OUTCOME='Success (ground pad)'
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>min(DATE)</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2015-12-22</td>
        </tr>
    </tbody>
</table>



### Task 6

##### List the names of the boosters which have success in drone ship and have payload mass greater than 4000 but less than 6000



```python
%sql SELECT BOOSTER_VERSION FROM SPACEXTBL WHERE PAYLOAD_MASS__KG_ between 4000 and 6000 AND LANDING_OUTCOME='Success (drone ship)'
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>Booster_Version</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>F9 FT B1022</td>
        </tr>
        <tr>
            <td>F9 FT B1026</td>
        </tr>
        <tr>
            <td>F9 FT  B1021.2</td>
        </tr>
        <tr>
            <td>F9 FT  B1031.2</td>
        </tr>
    </tbody>
</table>



### Task 7




##### List the total number of successful and failure mission outcomes



```python
%sql SELECT MISSION_OUTCOME, COUNT(*) FROM SPACEXTBL GROUP BY MISSION_OUTCOME
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>Mission_Outcome</th>
            <th>COUNT(*)</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Failure (in flight)</td>
            <td>1</td>
        </tr>
        <tr>
            <td>Success</td>
            <td>98</td>
        </tr>
        <tr>
            <td>Success </td>
            <td>1</td>
        </tr>
        <tr>
            <td>Success (payload status unclear)</td>
            <td>1</td>
        </tr>
    </tbody>
</table>



### Task 8



##### List the   names of the booster_versions which have carried the maximum payload mass. Use a subquery



```python
%sql SELECT BOOSTER_VERSION FROM SPACEXTBL WHERE PAYLOAD_MASS__KG_ = (SELECT MAX(PAYLOAD_MASS__KG_) FROM SPACEXTBL)
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>Booster_Version</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>F9 B5 B1048.4</td>
        </tr>
        <tr>
            <td>F9 B5 B1049.4</td>
        </tr>
        <tr>
            <td>F9 B5 B1051.3</td>
        </tr>
        <tr>
            <td>F9 B5 B1056.4</td>
        </tr>
        <tr>
            <td>F9 B5 B1048.5</td>
        </tr>
        <tr>
            <td>F9 B5 B1051.4</td>
        </tr>
        <tr>
            <td>F9 B5 B1049.5</td>
        </tr>
        <tr>
            <td>F9 B5 B1060.2 </td>
        </tr>
        <tr>
            <td>F9 B5 B1058.3 </td>
        </tr>
        <tr>
            <td>F9 B5 B1051.6</td>
        </tr>
        <tr>
            <td>F9 B5 B1060.3</td>
        </tr>
        <tr>
            <td>F9 B5 B1049.7 </td>
        </tr>
    </tbody>
</table>



### Task 9


##### List the records which will display the month names, failure landing_outcomes in drone ship ,booster versions, launch_site for the months in year 2015.

**Note: SQLLite does not support monthnames. So you need to use  substr(Date, 6,2) as month to get the months and substr(Date,0,5)='2015' for year.**



```python
%sql SELECT SUBSTR(DATE,6,2) AS MONTH, LANDING_OUTCOME AS LANDING_OUTCOME,  BOOSTER_VERSION AS BOOSTER_VERSION,  LAUNCH_SITE AS LAUNCH_SITE  FROM SPACEXTBL WHERE LANDING_OUTCOME = 'Failure (drone ship)' AND "DATE" LIKE '%2015%'
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>MONTH</th>
            <th>LANDING_OUTCOME</th>
            <th>BOOSTER_VERSION</th>
            <th>LAUNCH_SITE</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>01</td>
            <td>Failure (drone ship)</td>
            <td>F9 v1.1 B1012</td>
            <td>CCAFS LC-40</td>
        </tr>
        <tr>
            <td>04</td>
            <td>Failure (drone ship)</td>
            <td>F9 v1.1 B1015</td>
            <td>CCAFS LC-40</td>
        </tr>
    </tbody>
</table>



### Task 10




##### Rank the count of landing outcomes (such as Failure (drone ship) or Success (ground pad)) between the date 2010-06-04 and 2017-03-20, in descending order.



```python
%sql SELECT LANDING_OUTCOME, COUNT(*) as COUNT FROM SPACEXTBL WHERE "DATE" BETWEEN '2010-06-04' and '2017-03-20' GROUP BY LANDING_OUTCOME ORDER BY COUNT
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>Landing_Outcome</th>
            <th>COUNT</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Precluded (drone ship)</td>
            <td>1</td>
        </tr>
        <tr>
            <td>Failure (parachute)</td>
            <td>2</td>
        </tr>
        <tr>
            <td>Uncontrolled (ocean)</td>
            <td>2</td>
        </tr>
        <tr>
            <td>Controlled (ocean)</td>
            <td>3</td>
        </tr>
        <tr>
            <td>Success (ground pad)</td>
            <td>3</td>
        </tr>
        <tr>
            <td>Failure (drone ship)</td>
            <td>5</td>
        </tr>
        <tr>
            <td>Success (drone ship)</td>
            <td>5</td>
        </tr>
        <tr>
            <td>No attempt</td>
            <td>10</td>
        </tr>
    </tbody>
</table>



### Reference Links

* <a href ="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Labs_Coursera_V5/labs/Lab%20-%20String%20Patterns%20-%20Sorting%20-%20Grouping/instructional-labs.md.html?origin=www.coursera.org">Hands-on Lab : String Patterns, Sorting and Grouping</a>  

*  <a  href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Labs_Coursera_V5/labs/Lab%20-%20Built-in%20functions%20/Hands-on_Lab__Built-in_Functions.md.html?origin=www.coursera.org">Hands-on Lab: Built-in functions</a>

*  <a  href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Labs_Coursera_V5/labs/Lab%20-%20Sub-queries%20and%20Nested%20SELECTs%20/instructional-labs.md.html?origin=www.coursera.org">Hands-on Lab : Sub-queries and Nested SELECT Statements</a>

*   <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Module%205/DB0201EN-Week3-1-3-SQLmagic.ipynb">Hands-on Tutorial: Accessing Databases with SQL magic</a>

*  <a href= "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Module%205/DB0201EN-Week3-1-4-Analyzing.ipynb">Hands-on Lab: Analyzing a real World Data Set</a>




## Author(s)

<h4> Lakshmi Holla </h4>


## Other Contributors

<h4> Rav Ahuja </h4>


## Change log
| Date | Version | Changed by | Change Description |
|------|--------|--------|---------|
| 2021-07-09 | 0.2 |Lakshmi Holla | Changes made in magic sql|
| 2021-05-20 | 0.1 |Lakshmi Holla | Created Initial Version |


## <h3 align="center"> © IBM Corporation 2021. All rights reserved. <h3/>

