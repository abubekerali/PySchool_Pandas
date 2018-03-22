

```python
#The observed trends are
#1 Students in high spending schools performed low
#2 Students in low budget spending schools performed high
```


```python
#dependencies
import numpy as np
import pandas as pd

```


```python
#Reading csv files in pandas
schools_df= pd.read_csv("C:\\Users\\abu\\PythonData\\PyCitySchools\\raw_data\\schools_complete.csv")
students_df=pd.read_csv("C:\\Users\\abu\\PythonData\\PyCitySchools\\raw_data\\students_complete.csv")
```


```python
#Checking the head and shape of the data frames
students_df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>school</th>
      <th>reading_score</th>
      <th>math_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
    </tr>
  </tbody>
</table>
</div>




```python
schools_df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>name</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
    </tr>
  </tbody>
</table>
</div>




```python
#checking the size of the student data frame
students_df.shape
```




    (39170, 9)




```python

```


```python
#identifying students passing math and reading with passing score of 70
students_df["%Passing Reading"]=students_df["reading_score"]>=70
students_df["%Passing Math"]=students_df["math_score"]>=70
```


```python
#calculating percent passing reading
PassingReading=students_df["%Passing Reading"].sum()/len(students_df["name"])*100
```


```python
#Calculating percent passing math
PassingMath=students_df["%Passing Math"].sum()/len(students_df["name"])*100
```


```python
#calculating overall passing rate
OverallPassing= (students_df["%Passing Math"].sum() + students_df["%Passing Reading"].sum())/2/len(students_df["name"])*100
```


```python
#Storing  all to a dictionary
District_dic= {
                                "Total School":[schools_df["name"].count()],
                                "Total Students":[students_df["name"].count()],
                                "Total Budget":[schools_df["budget"].sum()],
                                "Average Math Score ":[students_df["math_score"].mean()],
                                "Average Reading Score":[students_df["reading_score"].mean()],
                                "%Passing Math":[PassingMath],
                                "%Passing Reading":[PassingReading],
                                "Overall Passing Rate":[OverallPassing]}
```


```python
#converting the dictionary to a data frame
District_Summary=pd.DataFrame(District_dic,columns=pd.Index(["Total School","Total Students","Total Budget","Average Math Score",
                                                             "Average Reading Score","%Passing Math","%Passing Reading","Overall Passing Rate"]))
```


```python
#Snaphsot of the District Summary
District_Summary
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total School</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%Passing Math</th>
      <th>%Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39170</td>
      <td>24649428</td>
      <td>NaN</td>
      <td>81.87784</td>
      <td>74.980853</td>
      <td>85.805463</td>
      <td>80.393158</td>
    </tr>
  </tbody>
</table>
</div>




```python

```


```python
#renaming the name column of schools dataframe to school 
schools_df=schools_df.rename(columns={"name":"school"})
```


```python
#combining the school and the student data frame using merge
District_School= pd.merge(schools_df,students_df,how='inner',on='school')
```


```python
District_School.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>%Passing Reading</th>
      <th>%Passing Math</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>66</td>
      <td>79</td>
      <td>False</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>94</td>
      <td>61</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>90</td>
      <td>60</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>67</td>
      <td>58</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>97</td>
      <td>84</td>
      <td>True</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Grouping the district school data with school name
District_group=District_School.groupby("school").agg({"type":"unique",
                                                                     "name":"count",
                                                                     "budget":"unique",
                                                                     "math_score":"mean",
                                                                     "reading_score":["mean"],
                                                                     "%Passing Reading":["sum"],
                                                                     "%Passing Math":sum })
```


```python
District_group.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>type</th>
      <th>name</th>
      <th>budget</th>
      <th>math_score</th>
      <th>reading_score</th>
      <th>%Passing Reading</th>
      <th>%Passing Math</th>
    </tr>
    <tr>
      <th></th>
      <th>unique</th>
      <th>count</th>
      <th>unique</th>
      <th>mean</th>
      <th>mean</th>
      <th>sum</th>
      <th>sum</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>[District]</td>
      <td>4976</td>
      <td>[3124928]</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>4077.0</td>
      <td>3318.0</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>[Charter]</td>
      <td>1858</td>
      <td>[1081356]</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>1803.0</td>
      <td>1749.0</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>[District]</td>
      <td>2949</td>
      <td>[1884411]</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>2381.0</td>
      <td>1946.0</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>[District]</td>
      <td>2739</td>
      <td>[1763916]</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>2172.0</td>
      <td>1871.0</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>[Charter]</td>
      <td>1468</td>
      <td>[917500]</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>1426.0</td>
      <td>1371.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#removing the groupby aggregate column
District_group.columns=District_group.columns.droplevel(1)
```


```python
#Adding the overall passing column
District_group["Overall Passing Rate"]=(District_group["%Passing Reading"]+District_group["%Passing Math"])/2
```


```python

```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%Passing Reading</th>
      <th>%Passing Math</th>
      <th>Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bailey High School</td>
      <td>[District]</td>
      <td>4976</td>
      <td>[3124928]</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>4077.0</td>
      <td>3318.0</td>
      <td>3697.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cabrera High School</td>
      <td>[Charter]</td>
      <td>1858</td>
      <td>[1081356]</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>1803.0</td>
      <td>1749.0</td>
      <td>1776.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Figueroa High School</td>
      <td>[District]</td>
      <td>2949</td>
      <td>[1884411]</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>2381.0</td>
      <td>1946.0</td>
      <td>2163.5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ford High School</td>
      <td>[District]</td>
      <td>2739</td>
      <td>[1763916]</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>2172.0</td>
      <td>1871.0</td>
      <td>2021.5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>[Charter]</td>
      <td>1468</td>
      <td>[917500]</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>1426.0</td>
      <td>1371.0</td>
      <td>1398.5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Hernandez High School</td>
      <td>[District]</td>
      <td>4635</td>
      <td>[3022020]</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>3748.0</td>
      <td>3094.0</td>
      <td>3421.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Holden High School</td>
      <td>[Charter]</td>
      <td>427</td>
      <td>[248087]</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>411.0</td>
      <td>395.0</td>
      <td>403.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Huang High School</td>
      <td>[District]</td>
      <td>2917</td>
      <td>[1910635]</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>2372.0</td>
      <td>1916.0</td>
      <td>2144.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Johnson High School</td>
      <td>[District]</td>
      <td>4761</td>
      <td>[3094650]</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>3867.0</td>
      <td>3145.0</td>
      <td>3506.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>[Charter]</td>
      <td>962</td>
      <td>[585858]</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>923.0</td>
      <td>910.0</td>
      <td>916.5</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Rodriguez High School</td>
      <td>[District]</td>
      <td>3999</td>
      <td>[2547363]</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>3208.0</td>
      <td>2654.0</td>
      <td>2931.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Shelton High School</td>
      <td>[Charter]</td>
      <td>1761</td>
      <td>[1056600]</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>1688.0</td>
      <td>1653.0</td>
      <td>1670.5</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Thomas High School</td>
      <td>[Charter]</td>
      <td>1635</td>
      <td>[1043130]</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>1591.0</td>
      <td>1525.0</td>
      <td>1558.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Wilson High School</td>
      <td>[Charter]</td>
      <td>2283</td>
      <td>[1319574]</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>2204.0</td>
      <td>2143.0</td>
      <td>2173.5</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Wright High School</td>
      <td>[Charter]</td>
      <td>1800</td>
      <td>[1049400]</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>1739.0</td>
      <td>1680.0</td>
      <td>1709.5</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Calculating the percentage of passing math
District_group["%Passing Math"]=District_group["%Passing Math"]/District_group["name"]*100
```


```python
#Calculating the percentage of passing reading
District_group["%Passing Reading"]=District_group["%Passing Reading"]/District_group["name"]*100
```


```python
#Calculating the percetage of overall passing rate
District_group["Overall Passing Rate"]=District_group["Overall Passing Rate"]
```


```python
#Calculating the buget per student
District_group["Per Student Budget"]=District_group["budget"]/District_group["name"]*100
```


```python
#School Summary
```


```python
#Renaming the columns of the school Summary
District_group.rename(columns={"type":"School Type","name":"Total Students","budget":"Total School Budget","math_score":"Average Math Score","reading_score":"Average Reading Score","%Passing Reading":"%Passing Reading ","%Passing Math":"%Passing Math","Overall Passing Rate":"Overall Passing Rate"})
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%Passing Reading</th>
      <th>%Passing Math</th>
      <th>Overall Passing Rate</th>
      <th>Per Student Budget</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>[District]</td>
      <td>4976</td>
      <td>[3124928]</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>81.933280</td>
      <td>66.680064</td>
      <td>74.306672</td>
      <td>[62800.0]</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>[Charter]</td>
      <td>1858</td>
      <td>[1081356]</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>97.039828</td>
      <td>94.133477</td>
      <td>95.586652</td>
      <td>[58200.0]</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>[District]</td>
      <td>2949</td>
      <td>[1884411]</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>80.739234</td>
      <td>65.988471</td>
      <td>73.363852</td>
      <td>[63900.0]</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>[District]</td>
      <td>2739</td>
      <td>[1763916]</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>79.299014</td>
      <td>68.309602</td>
      <td>73.804308</td>
      <td>[64400.0]</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>[Charter]</td>
      <td>1468</td>
      <td>[917500]</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>97.138965</td>
      <td>93.392371</td>
      <td>95.265668</td>
      <td>[62500.0]</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>[District]</td>
      <td>4635</td>
      <td>[3022020]</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>80.862999</td>
      <td>66.752967</td>
      <td>73.807983</td>
      <td>[65200.0]</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>[Charter]</td>
      <td>427</td>
      <td>[248087]</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>96.252927</td>
      <td>92.505855</td>
      <td>94.379391</td>
      <td>[58100.0]</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>[District]</td>
      <td>2917</td>
      <td>[1910635]</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>81.316421</td>
      <td>65.683922</td>
      <td>73.500171</td>
      <td>[65500.0]</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>[District]</td>
      <td>4761</td>
      <td>[3094650]</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>81.222432</td>
      <td>66.057551</td>
      <td>73.639992</td>
      <td>[65000.0]</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>[Charter]</td>
      <td>962</td>
      <td>[585858]</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>95.945946</td>
      <td>94.594595</td>
      <td>95.270270</td>
      <td>[60900.0]</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>[District]</td>
      <td>3999</td>
      <td>[2547363]</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>80.220055</td>
      <td>66.366592</td>
      <td>73.293323</td>
      <td>[63700.0]</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>[Charter]</td>
      <td>1761</td>
      <td>[1056600]</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>95.854628</td>
      <td>93.867121</td>
      <td>94.860875</td>
      <td>[60000.0]</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>[Charter]</td>
      <td>1635</td>
      <td>[1043130]</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>97.308869</td>
      <td>93.272171</td>
      <td>95.290520</td>
      <td>[63800.0]</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>[Charter]</td>
      <td>2283</td>
      <td>[1319574]</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>96.539641</td>
      <td>93.867718</td>
      <td>95.203679</td>
      <td>[57800.0]</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>[Charter]</td>
      <td>1800</td>
      <td>[1049400]</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>96.611111</td>
      <td>93.333333</td>
      <td>94.972222</td>
      <td>[58300.0]</td>
    </tr>
  </tbody>
</table>
</div>




```python


```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>name</th>
      <th>budget</th>
      <th>math_score</th>
      <th>reading_score</th>
      <th>%Passing Reading</th>
      <th>%Passing Math</th>
      <th>Overall Passing Rate</th>
      <th>Per Student Budget</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>[District]</td>
      <td>4976</td>
      <td>[3124928]</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>81.933280</td>
      <td>66.680064</td>
      <td>74.306672</td>
      <td>[62800.0]</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>[Charter]</td>
      <td>1858</td>
      <td>[1081356]</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>97.039828</td>
      <td>94.133477</td>
      <td>95.586652</td>
      <td>[58200.0]</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>[District]</td>
      <td>2949</td>
      <td>[1884411]</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>80.739234</td>
      <td>65.988471</td>
      <td>73.363852</td>
      <td>[63900.0]</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>[District]</td>
      <td>2739</td>
      <td>[1763916]</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>79.299014</td>
      <td>68.309602</td>
      <td>73.804308</td>
      <td>[64400.0]</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>[Charter]</td>
      <td>1468</td>
      <td>[917500]</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>97.138965</td>
      <td>93.392371</td>
      <td>95.265668</td>
      <td>[62500.0]</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Idetifying the top performing schools by overall Passing rate
Top_Performing_School=District_group.sort_values(["Overall Passing Rate"], ascending=False)
```


```python
Top_Performing_School.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>name</th>
      <th>budget</th>
      <th>math_score</th>
      <th>reading_score</th>
      <th>%Passing Reading</th>
      <th>%Passing Math</th>
      <th>Overall Passing Rate</th>
      <th>Per Student Budget</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Cabrera High School</th>
      <td>[Charter]</td>
      <td>1858</td>
      <td>[1081356]</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>97.039828</td>
      <td>94.133477</td>
      <td>95.586652</td>
      <td>[58200.0]</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>[Charter]</td>
      <td>1635</td>
      <td>[1043130]</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>97.308869</td>
      <td>93.272171</td>
      <td>95.290520</td>
      <td>[63800.0]</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>[Charter]</td>
      <td>962</td>
      <td>[585858]</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>95.945946</td>
      <td>94.594595</td>
      <td>95.270270</td>
      <td>[60900.0]</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>[Charter]</td>
      <td>1468</td>
      <td>[917500]</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>97.138965</td>
      <td>93.392371</td>
      <td>95.265668</td>
      <td>[62500.0]</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>[Charter]</td>
      <td>2283</td>
      <td>[1319574]</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>96.539641</td>
      <td>93.867718</td>
      <td>95.203679</td>
      <td>[57800.0]</td>
    </tr>
  </tbody>
</table>
</div>




```python
#identifying the low performing schools by overall passing rate
low_performing_school=District_group.sort_values(["Overall Passing Rate"], ascending=True)
```


```python
low_performing_school.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>name</th>
      <th>budget</th>
      <th>math_score</th>
      <th>reading_score</th>
      <th>%Passing Reading</th>
      <th>%Passing Math</th>
      <th>Overall Passing Rate</th>
      <th>Per Student Budget</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rodriguez High School</th>
      <td>[District]</td>
      <td>3999</td>
      <td>[2547363]</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>80.220055</td>
      <td>66.366592</td>
      <td>73.293323</td>
      <td>[63700.0]</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>[District]</td>
      <td>2949</td>
      <td>[1884411]</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>80.739234</td>
      <td>65.988471</td>
      <td>73.363852</td>
      <td>[63900.0]</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>[District]</td>
      <td>2917</td>
      <td>[1910635]</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>81.316421</td>
      <td>65.683922</td>
      <td>73.500171</td>
      <td>[65500.0]</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>[District]</td>
      <td>4761</td>
      <td>[3094650]</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>81.222432</td>
      <td>66.057551</td>
      <td>73.639992</td>
      <td>[65000.0]</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>[District]</td>
      <td>2739</td>
      <td>[1763916]</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>79.299014</td>
      <td>68.309602</td>
      <td>73.804308</td>
      <td>[64400.0]</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Math Score by grade
```


```python
#grouping the district school by school and grade
Math_Score_Per_Grade=District_School.groupby(['school',"grade"])["math_score"].mean()
```


```python
Math_Score_Per_Grade
```




    school                 grade
    Bailey High School     10th     76.996772
                           11th     77.515588
                           12th     76.492218
                           9th      77.083676
    Cabrera High School    10th     83.154506
                           11th     82.765560
                           12th     83.277487
                           9th      83.094697
    Figueroa High School   10th     76.539974
                           11th     76.884344
                           12th     77.151369
                           9th      76.403037
    Ford High School       10th     77.672316
                           11th     76.918058
                           12th     76.179963
                           9th      77.361345
    Griffin High School    10th     84.229064
                           11th     83.842105
                           12th     83.356164
                           9th      82.044010
    Hernandez High School  10th     77.337408
                           11th     77.136029
                           12th     77.186567
                           9th      77.438495
    Holden High School     10th     83.429825
                           11th     85.000000
                           12th     82.855422
                           9th      83.787402
    Huang High School      10th     75.908735
                           11th     76.446602
                           12th     77.225641
                           9th      77.027251
    Johnson High School    10th     76.691117
                           11th     77.491653
                           12th     76.863248
                           9th      77.187857
    Pena High School       10th     83.372000
                           11th     84.328125
                           12th     84.121547
                           9th      83.625455
    Rodriguez High School  10th     76.612500
                           11th     76.395626
                           12th     77.690748
                           9th      76.859966
    Shelton High School    10th     82.917411
                           11th     83.383495
                           12th     83.778976
                           9th      83.420755
    Thomas High School     10th     83.087886
                           11th     83.498795
                           12th     83.497041
                           9th      83.590022
    Wilson High School     10th     83.724422
                           11th     83.195326
                           12th     83.035794
                           9th      83.085578
    Wright High School     10th     84.010288
                           11th     83.836782
                           12th     83.644986
                           9th      83.264706
    Name: math_score, dtype: float64




```python
#converting the groupby to data frame
Math_Score_Per_Grade_df=pd.DataFrame(Math_Score_Per_Grade)
```


```python
Math_Score_Per_Grade_df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>math_score</th>
    </tr>
    <tr>
      <th>school</th>
      <th>grade</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="4" valign="top">Bailey High School</th>
      <th>10th</th>
      <td>76.996772</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>77.515588</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>77.083676</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Cabrera High School</th>
      <th>10th</th>
      <td>83.154506</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>82.765560</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.094697</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Figueroa High School</th>
      <th>10th</th>
      <td>76.539974</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>76.884344</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>76.403037</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Ford High School</th>
      <th>10th</th>
      <td>77.672316</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>76.918058</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>77.361345</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Griffin High School</th>
      <th>10th</th>
      <td>84.229064</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.842105</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>82.044010</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Hernandez High School</th>
      <th>10th</th>
      <td>77.337408</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>77.136029</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>77.438495</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Holden High School</th>
      <th>10th</th>
      <td>83.429825</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>85.000000</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.787402</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Huang High School</th>
      <th>10th</th>
      <td>75.908735</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>76.446602</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>77.027251</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Johnson High School</th>
      <th>10th</th>
      <td>76.691117</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>77.491653</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>77.187857</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Pena High School</th>
      <th>10th</th>
      <td>83.372000</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>84.328125</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.625455</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Rodriguez High School</th>
      <th>10th</th>
      <td>76.612500</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>76.395626</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>76.859966</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Shelton High School</th>
      <th>10th</th>
      <td>82.917411</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.383495</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.420755</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Thomas High School</th>
      <th>10th</th>
      <td>83.087886</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.498795</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.497041</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.590022</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Wilson High School</th>
      <th>10th</th>
      <td>83.724422</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.195326</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.085578</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Wright High School</th>
      <th>10th</th>
      <td>84.010288</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.836782</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.644986</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.264706</td>
    </tr>
  </tbody>
</table>
</div>




```python
Math_Score_Per_Grade_df.columns
```




    Index(['math_score'], dtype='object')




```python
#Reading Score Per Grade
```


```python
#grouping the district school by school and grade
Reading_Score_Per_Grade=District_School.groupby(["school","grade"])["reading_score"].mean()
```


```python
#changing the groupby series to data frame
Reading_Score_Per_Grade_df=pd.DataFrame(Reading_Score_Per_Grade)
```


```python
Reading_Score_Per_Grade_df
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>reading_score</th>
    </tr>
    <tr>
      <th>school</th>
      <th>grade</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="4" valign="top">Bailey High School</th>
      <th>10th</th>
      <td>80.907183</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>80.945643</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>80.912451</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>81.303155</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Cabrera High School</th>
      <th>10th</th>
      <td>84.253219</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.788382</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>84.287958</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.676136</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Figueroa High School</th>
      <th>10th</th>
      <td>81.408912</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>80.640339</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>81.384863</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>81.198598</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Ford High School</th>
      <th>10th</th>
      <td>81.262712</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>80.403642</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>80.662338</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>80.632653</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Griffin High School</th>
      <th>10th</th>
      <td>83.706897</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>84.288089</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>84.013699</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.369193</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Hernandez High School</th>
      <th>10th</th>
      <td>80.660147</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>81.396140</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>80.857143</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>80.866860</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Holden High School</th>
      <th>10th</th>
      <td>83.324561</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.815534</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>84.698795</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.677165</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Huang High School</th>
      <th>10th</th>
      <td>81.512386</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>81.417476</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>80.305983</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>81.290284</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Johnson High School</th>
      <th>10th</th>
      <td>80.773431</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>80.616027</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>81.227564</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>81.260714</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Pena High School</th>
      <th>10th</th>
      <td>83.612000</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>84.335938</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>84.591160</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.807273</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Rodriguez High School</th>
      <th>10th</th>
      <td>80.629808</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>80.864811</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>80.376426</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>80.993127</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Shelton High School</th>
      <th>10th</th>
      <td>83.441964</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>84.373786</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>82.781671</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>84.122642</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Thomas High School</th>
      <th>10th</th>
      <td>84.254157</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.585542</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.831361</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.728850</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Wilson High School</th>
      <th>10th</th>
      <td>84.021452</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.764608</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>84.317673</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.939778</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Wright High School</th>
      <th>10th</th>
      <td>83.812757</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>84.156322</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>84.073171</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.833333</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Score by School Spending
```


```python

```
#identifying the minimum and maximum bdget per student
District_group["Per Student Budget"].max()

```python
District_group["Per Student Budget"].min()
```




    array([ 57800.])




```python
#creating bins to place the values
bins=[56500,58000,59500,61000,62500,64000,65500]
#creating group labels
group_labels=["56500 to 58000","58000 to 59500","59500 to 61000","61000 to 62500","62500 to 64000","64000 to 65500"]
```


```python
#slicing based on per student budget
pd.cut(District_group["Per Student Budget"],bins,labels=group_labels).head()

```




    school
    Bailey High School      62500 to 64000
    Cabrera High School     58000 to 59500
    Figueroa High School    62500 to 64000
    Ford High School        64000 to 65500
    Griffin High School     61000 to 62500
    Name: Per Student Budget, dtype: category
    Categories (6, object): [56500 to 58000 < 58000 to 59500 < 59500 to 61000 < 61000 to 62500 < 62500 to 64000 < 64000 to 65500]




```python
District_group["Per Student Budget Group"]=pd.cut(District_group["Per Student Budget"],bins,labels=group_labels)
```


```python
District_group.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>name</th>
      <th>budget</th>
      <th>math_score</th>
      <th>reading_score</th>
      <th>%Passing Reading</th>
      <th>%Passing Math</th>
      <th>Overall Passing Rate</th>
      <th>Per Student Budget</th>
      <th>Per Student Budget Group</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>[District]</td>
      <td>4976</td>
      <td>[3124928]</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>81.933280</td>
      <td>66.680064</td>
      <td>74.306672</td>
      <td>[62800.0]</td>
      <td>62500 to 64000</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>[Charter]</td>
      <td>1858</td>
      <td>[1081356]</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>97.039828</td>
      <td>94.133477</td>
      <td>95.586652</td>
      <td>[58200.0]</td>
      <td>58000 to 59500</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>[District]</td>
      <td>2949</td>
      <td>[1884411]</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>80.739234</td>
      <td>65.988471</td>
      <td>73.363852</td>
      <td>[63900.0]</td>
      <td>62500 to 64000</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>[District]</td>
      <td>2739</td>
      <td>[1763916]</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>79.299014</td>
      <td>68.309602</td>
      <td>73.804308</td>
      <td>[64400.0]</td>
      <td>64000 to 65500</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>[Charter]</td>
      <td>1468</td>
      <td>[917500]</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>97.138965</td>
      <td>93.392371</td>
      <td>95.265668</td>
      <td>[62500.0]</td>
      <td>61000 to 62500</td>
    </tr>
  </tbody>
</table>
</div>




```python

Per_Student_Budget_group=District_group[["math_score","reading_score","%Passing Math","%Passing Reading","Overall Passing Rate","Per Student Budget"]]
```


```python
Per_Student_Budget_group.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>math_score</th>
      <th>reading_score</th>
      <th>%Passing Math</th>
      <th>%Passing Reading</th>
      <th>Overall Passing Rate</th>
      <th>Per Student Budget</th>
      <th>Per Student Budget Group</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>66.680064</td>
      <td>81.933280</td>
      <td>74.306672</td>
      <td>[62800.0]</td>
      <td>62500 to 64000</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>95.586652</td>
      <td>[58200.0]</td>
      <td>58000 to 59500</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
      <td>[63900.0]</td>
      <td>62500 to 64000</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>73.804308</td>
      <td>[64400.0]</td>
      <td>64000 to 65500</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
      <td>[62500.0]</td>
      <td>61000 to 62500</td>
    </tr>
  </tbody>
</table>
</div>




```python
#grouping by perstudent budget group
Scores_by_school_spending =Per_Student_Budget_group.groupby(["Per Student Budget Group"])
```


```python

```


```python
Scores_by_school_spending.mean()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>math_score</th>
      <th>reading_score</th>
      <th>%Passing Math</th>
      <th>%Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Per Student Budget Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>56500 to 58000</th>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
    </tr>
    <tr>
      <th>58000 to 59500</th>
      <td>83.515798</td>
      <td>83.915256</td>
      <td>93.324222</td>
      <td>96.634622</td>
      <td>94.979422</td>
    </tr>
    <tr>
      <th>59500 to 61000</th>
      <td>83.599686</td>
      <td>83.885211</td>
      <td>94.230858</td>
      <td>95.900287</td>
      <td>95.065572</td>
    </tr>
    <tr>
      <th>61000 to 62500</th>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
    </tr>
    <tr>
      <th>62500 to 64000</th>
      <td>78.505315</td>
      <td>81.696400</td>
      <td>73.076824</td>
      <td>85.050359</td>
      <td>79.063592</td>
    </tr>
    <tr>
      <th>64000 to 65500</th>
      <td>77.023555</td>
      <td>80.957446</td>
      <td>66.701010</td>
      <td>80.675217</td>
      <td>73.688113</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Scores by School type

```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Selecting the desired columns
School_Type=District_group[["math_score","reading_score","%Passing Math","%Passing Reading","Overall Passing Rate","type"]]

```


```python

Scores_by_School_Type=School_Type.groupby("type")
```


```python
Scores_by_School_Type.mean()
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-148-34bfc686c3ae> in <module>()
    ----> 1 Scores_by_School_Type.count()
    

    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\groupby.py in count(self)
       3877 
       3878         data, _ = self._get_data_to_aggregate()
    -> 3879         ids, _, ngroups = self.grouper.group_info
       3880         mask = ids != -1
       3881 
    

    pandas\src\properties.pyx in pandas.lib.cache_readonly.__get__ (pandas\lib.c:45588)()
    

    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\groupby.py in group_info(self)
       1678     @cache_readonly
       1679     def group_info(self):
    -> 1680         comp_ids, obs_group_ids = self._get_compressed_labels()
       1681 
       1682         ngroups = len(obs_group_ids)
    

    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\groupby.py in _get_compressed_labels(self)
       1685 
       1686     def _get_compressed_labels(self):
    -> 1687         all_labels = [ping.labels for ping in self.groupings]
       1688         if len(all_labels) > 1:
       1689             group_index = get_group_index(all_labels, self.shape,
    

    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\groupby.py in <listcomp>(.0)
       1685 
       1686     def _get_compressed_labels(self):
    -> 1687         all_labels = [ping.labels for ping in self.groupings]
       1688         if len(all_labels) > 1:
       1689             group_index = get_group_index(all_labels, self.shape,
    

    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\groupby.py in labels(self)
       2316     def labels(self):
       2317         if self._labels is None:
    -> 2318             self._make_labels()
       2319         return self._labels
       2320 
    

    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\groupby.py in _make_labels(self)
       2327     def _make_labels(self):
       2328         if self._labels is None or self._group_index is None:
    -> 2329             labels, uniques = algos.factorize(self.grouper, sort=self.sort)
       2330             uniques = Index(uniques, name=self.name)
       2331             self._labels = labels
    

    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\algorithms.py in factorize(values, sort, order, na_sentinel, size_hint)
        311     table = hash_klass(size_hint or len(vals))
        312     uniques = vec_klass()
    --> 313     labels = table.get_labels(vals, uniques, 0, na_sentinel, True)
        314 
        315     labels = _ensure_platform_int(labels)
    

    pandas\src\hashtable_class_helper.pxi in pandas.hashtable.PyObjectHashTable.get_labels (pandas\hashtable.c:15447)()
    

    TypeError: unhashable type: 'numpy.ndarray'



```python
#Scores by School Size
```


```python
School_Size=District_School.groupby("school").agg({"size":"unique",
                                                                     "name":"count",
                                                                     "budget":"unique",
                                                                     "math_score":"mean",
                                                                     "reading_score":["mean"],
                                                                     "%Passing Reading":["sum"],
                                                                     "%Passing Math":sum })
```


```python
School_Size.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>size</th>
      <th>name</th>
      <th>budget</th>
      <th>math_score</th>
      <th>reading_score</th>
      <th>%Passing Reading</th>
      <th>%Passing Math</th>
    </tr>
    <tr>
      <th></th>
      <th>unique</th>
      <th>count</th>
      <th>unique</th>
      <th>mean</th>
      <th>mean</th>
      <th>sum</th>
      <th>sum</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>[4976]</td>
      <td>4976</td>
      <td>[3124928]</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>4077.0</td>
      <td>3318.0</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>[1858]</td>
      <td>1858</td>
      <td>[1081356]</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>1803.0</td>
      <td>1749.0</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>[2949]</td>
      <td>2949</td>
      <td>[1884411]</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>2381.0</td>
      <td>1946.0</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>[2739]</td>
      <td>2739</td>
      <td>[1763916]</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>2172.0</td>
      <td>1871.0</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>[1468]</td>
      <td>1468</td>
      <td>[917500]</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>1426.0</td>
      <td>1371.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#removing the groupby aggregate column
School_Size.columns=School_Size.columns.droplevel(1)
```


```python
#Adding the overall passing column
School_Size["Overall Passing Rate"]=(School_Size["%Passing Reading"]+School_Size["%Passing Math"])/2
```


```python
#Calculating the percentage of passing math
School_Size["%Passing Math"]=School_Size["%Passing Math"]/School_Size["name"]*100
```


```python
#Calculating the percentage of passing reading
School_Size["%Passing Reading"]=School_Size["%Passing Reading"]/School_Size["name"]*100
```


```python
SchoSol_Size.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>size</th>
      <th>name</th>
      <th>budget</th>
      <th>math_score</th>
      <th>reading_score</th>
      <th>%Passing Reading</th>
      <th>%Passing Math</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>[4976]</td>
      <td>4976</td>
      <td>[3124928]</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>81.933280</td>
      <td>66.680064</td>
      <td>74.306672</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>[1858]</td>
      <td>1858</td>
      <td>[1081356]</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>97.039828</td>
      <td>94.133477</td>
      <td>95.586652</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>[2949]</td>
      <td>2949</td>
      <td>[1884411]</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>80.739234</td>
      <td>65.988471</td>
      <td>73.363852</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>[2739]</td>
      <td>2739</td>
      <td>[1763916]</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>79.299014</td>
      <td>68.309602</td>
      <td>73.804308</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>[1468]</td>
      <td>1468</td>
      <td>[917500]</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>97.138965</td>
      <td>93.392371</td>
      <td>95.265668</td>
    </tr>
  </tbody>
</table>
</div>




```python
#identifying the min and max of school size
School_Size["size"].max()
```




    array([4976], dtype=int64)




```python
School_Size["size"].min()
```




    array([427], dtype=int64)




```python
#creating bins to place the values
bins=[0,1000,2000,5000]
#creating group labels
group_labels=["Small=<1000","Medium=1000-2000","Large=2000-5000"]
```


```python
#Selecting desired columns
School_Size_Cut=School_Size[["math_score","reading_score","%Passing Math","%Passing Reading","Overall Passing Rate","size"]]
```


```python
#slicing based on per student budget
pd.cut(School_Size["size"],bins,labels=group_labels).head(10)
```




    school
    Bailey High School        Large=2000-5000
    Cabrera High School      Medium=1000-2000
    Figueroa High School      Large=2000-5000
    Ford High School          Large=2000-5000
    Griffin High School      Medium=1000-2000
    Hernandez High School     Large=2000-5000
    Holden High School            Small=<1000
    Huang High School         Large=2000-5000
    Johnson High School       Large=2000-5000
    Pena High School              Small=<1000
    Name: size, dtype: category
    Categories (3, object): [Small=<1000 < Medium=1000-2000 < Large=2000-5000]




```python
School_Size["size group"]=pd.cut(School_Size["size"],bins,labels=group_labels)
```


```python
#grouping by school size
School_Size_group=School_Size.groupby("size")
```


```python
School_Size_group.mean()
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-193-2fc68dd66237> in <module>()
    ----> 1 School_Size_group.count()
    

    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\groupby.py in count(self)
       3877 
       3878         data, _ = self._get_data_to_aggregate()
    -> 3879         ids, _, ngroups = self.grouper.group_info
       3880         mask = ids != -1
       3881 
    

    pandas\src\properties.pyx in pandas.lib.cache_readonly.__get__ (pandas\lib.c:45588)()
    

    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\groupby.py in group_info(self)
       1678     @cache_readonly
       1679     def group_info(self):
    -> 1680         comp_ids, obs_group_ids = self._get_compressed_labels()
       1681 
       1682         ngroups = len(obs_group_ids)
    

    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\groupby.py in _get_compressed_labels(self)
       1685 
       1686     def _get_compressed_labels(self):
    -> 1687         all_labels = [ping.labels for ping in self.groupings]
       1688         if len(all_labels) > 1:
       1689             group_index = get_group_index(all_labels, self.shape,
    

    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\groupby.py in <listcomp>(.0)
       1685 
       1686     def _get_compressed_labels(self):
    -> 1687         all_labels = [ping.labels for ping in self.groupings]
       1688         if len(all_labels) > 1:
       1689             group_index = get_group_index(all_labels, self.shape,
    

    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\groupby.py in labels(self)
       2316     def labels(self):
       2317         if self._labels is None:
    -> 2318             self._make_labels()
       2319         return self._labels
       2320 
    

    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\groupby.py in _make_labels(self)
       2327     def _make_labels(self):
       2328         if self._labels is None or self._group_index is None:
    -> 2329             labels, uniques = algos.factorize(self.grouper, sort=self.sort)
       2330             uniques = Index(uniques, name=self.name)
       2331             self._labels = labels
    

    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\algorithms.py in factorize(values, sort, order, na_sentinel, size_hint)
        311     table = hash_klass(size_hint or len(vals))
        312     uniques = vec_klass()
    --> 313     labels = table.get_labels(vals, uniques, 0, na_sentinel, True)
        314 
        315     labels = _ensure_platform_int(labels)
    

    pandas\src\hashtable_class_helper.pxi in pandas.hashtable.PyObjectHashTable.get_labels (pandas\hashtable.c:15447)()
    

    TypeError: unhashable type: 'numpy.ndarray'



```python


```
