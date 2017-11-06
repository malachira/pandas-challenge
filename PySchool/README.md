

```python
import os
import pandas as pd
```


```python
#create filepath
school_filepath = os.path.join("raw_data","schools_complete.csv")
student_filepath = os.path.join("raw_data","students_complete.csv")
```


```python
#read csv to data-frames
school_df = pd.read_csv(school_filepath)
student_df = pd.read_csv(student_filepath)
```


```python
#use groupby on student_df to get the avg reading/math score for each school
stu_df1 = student_df.groupby("school")["reading_score","math_score"].mean()
stu_df1
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>reading_score</th>
      <th>math_score</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>81.033963</td>
      <td>77.048432</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.975780</td>
      <td>83.061895</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.158020</td>
      <td>76.711767</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>80.746258</td>
      <td>77.102592</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.816757</td>
      <td>83.351499</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.934412</td>
      <td>77.289752</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.814988</td>
      <td>83.803279</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.182722</td>
      <td>76.629414</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>80.966394</td>
      <td>77.072464</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>84.044699</td>
      <td>83.839917</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.744686</td>
      <td>76.842711</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.725724</td>
      <td>83.359455</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.848930</td>
      <td>83.418349</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.989488</td>
      <td>83.274201</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.955000</td>
      <td>83.682222</td>
    </tr>
  </tbody>
</table>
</div>




```python
#use column filter on student_df to find the % of students passing math & reading(assuume 60 is pass)
math_pass_df = student_df.loc[student_df["math_score"] > 60, :]
reading_pass_df = student_df.loc[student_df["reading_score"] > 60, :]
```


```python
#use groupby on each of the df to get the total passing students per school
math_pass_count = pd.DataFrame(math_pass_df.groupby("school")["name"].count())
#rename "name" column 
math_pass_count = math_pass_count.rename(columns = {"name":"total math pass"})

reading_pass_count = pd.DataFrame(reading_pass_df.groupby("school")["name"].count())
#rename "name" column 
reading_pass_count = reading_pass_count.rename(columns = {"name":"total reading pass"})

```


```python
#Use df.join() to join these two df to stu_df1
stu_df2 = stu_df1.join(math_pass_count, how = "outer")
stu_df3 = stu_df2.join(reading_pass_count, how = "outer") 
stu_df3.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>total math pass</th>
      <th>total reading pass</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>81.033963</td>
      <td>77.048432</td>
      <td>4351</td>
      <td>4976</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>1858</td>
      <td>1858</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>2549</td>
      <td>2949</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>80.746258</td>
      <td>77.102592</td>
      <td>2389</td>
      <td>2739</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1468</td>
      <td>1468</td>
    </tr>
  </tbody>
</table>
</div>




```python
#make the index of stu_df4(school) into a column
stu_df3.reset_index(level=0, inplace=True)
stu_df3.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>total math pass</th>
      <th>total reading pass</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bailey High School</td>
      <td>81.033963</td>
      <td>77.048432</td>
      <td>4351</td>
      <td>4976</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cabrera High School</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>1858</td>
      <td>1858</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Figueroa High School</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>2549</td>
      <td>2949</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ford High School</td>
      <td>80.746258</td>
      <td>77.102592</td>
      <td>2389</td>
      <td>2739</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1468</td>
      <td>1468</td>
    </tr>
  </tbody>
</table>
</div>




```python
#rename "name" column in school_df to "school" to facilitate merging
school_df = school_df.rename(columns = {"name":"school"})
school_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
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
#merge school_df & stu_df4
merge_df = pd.merge(school_df, stu_df3, on = "school", how = "outer")
merge_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>total math pass</th>
      <th>total reading pass</th>
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
      <td>81.182722</td>
      <td>76.629414</td>
      <td>2533</td>
      <td>2917</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>2549</td>
      <td>2949</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>1761</td>
      <td>1761</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>4007</td>
      <td>4635</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1468</td>
      <td>1468</td>
    </tr>
  </tbody>
</table>
</div>




```python
#calculate the pass % per school for reading & math
merge_df["% passing math"] = (merge_df["total math pass"]/merge_df["size"])*100
merge_df["% passing reading"] = (merge_df["total reading pass"]/merge_df["size"])*100
merge_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>total math pass</th>
      <th>total reading pass</th>
      <th>% passing math</th>
      <th>% passing reading</th>
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
      <td>81.182722</td>
      <td>76.629414</td>
      <td>2533</td>
      <td>2917</td>
      <td>86.835790</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>2549</td>
      <td>2949</td>
      <td>86.436080</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>1761</td>
      <td>1761</td>
      <td>100.000000</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>4007</td>
      <td>4635</td>
      <td>86.450917</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1468</td>
      <td>1468</td>
      <td>100.000000</td>
      <td>100.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#create a df with only information about district schools
district_df = merge_df.loc[merge_df["type"] == "District", : ]
```


```python
#get data from district_df 
total_district_schools = district_df["school"].count()
total_district_students = district_df["size"].sum()
total_district_budget = district_df["budget"].sum()
avg_math_score = district_df["math_score"].mean()
avg_reading_score = district_df["reading_score"].mean()
prcnt_passing_math = district_df["% passing math"].mean()
prcnt_passing_reading = district_df["% passing reading"].mean()
overall_passing_rate = (prcnt_passing_math + prcnt_passing_reading)/2
```


```python
#create a data frame from the district data
district_summary = pd.DataFrame({"Overall Passing Rate" : [overall_passing_rate],
                                 "% Passing Reading" : [prcnt_passing_reading],
                                 "% Passing Math": [prcnt_passing_math],
                                 "Average Reading Score" : [avg_reading_score],
                                 "Average Math Score" : [avg_math_score],
                                 "Total Budget" :[total_district_budget],
                                 "Total Students" : [total_district_students],
                                 "Total Schools" : [total_district_schools],
                                })
district_summary
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>Overall Passing Rate</th>
      <th>Total Budget</th>
      <th>Total Schools</th>
      <th>Total Students</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>86.790742</td>
      <td>100.0</td>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>93.395371</td>
      <td>17347923</td>
      <td>7</td>
      <td>26976</td>
    </tr>
  </tbody>
</table>
</div>




```python
#arrange items in the correct order
district_summary.loc[:,["Total Schools","Total Students","Total Budget","Average Math Score","Average Reading Score",\
                       "% Passing Math","% Passing Reading","Overall Passing Rate"]]
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Schools</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7</td>
      <td>26976</td>
      <td>17347923</td>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>86.790742</td>
      <td>100.0</td>
      <td>93.395371</td>
    </tr>
  </tbody>
</table>
</div>




```python
merge_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>total math pass</th>
      <th>total reading pass</th>
      <th>% passing math</th>
      <th>% passing reading</th>
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
      <td>81.182722</td>
      <td>76.629414</td>
      <td>2533</td>
      <td>2917</td>
      <td>86.835790</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>2549</td>
      <td>2949</td>
      <td>86.436080</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>1761</td>
      <td>1761</td>
      <td>100.000000</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>4007</td>
      <td>4635</td>
      <td>86.450917</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1468</td>
      <td>1468</td>
      <td>100.000000</td>
      <td>100.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Copy merge_df to school_sumry_df, set school name as index & delete unwanted columsn
school_sumry_df = merge_df.copy()
school_sumry_df.set_index("school", inplace=True)
del school_sumry_df["School ID"]
school_sumry_df = school_sumry_df.rename(columns = {"size":"Total students"})
school_sumry_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>Total students</th>
      <th>budget</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>total math pass</th>
      <th>total reading pass</th>
      <th>% passing math</th>
      <th>% passing reading</th>
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
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>2533</td>
      <td>2917</td>
      <td>86.835790</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>2549</td>
      <td>2949</td>
      <td>86.436080</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>1761</td>
      <td>1761</td>
      <td>100.000000</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>4007</td>
      <td>4635</td>
      <td>86.450917</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1468</td>
      <td>1468</td>
      <td>100.000000</td>
      <td>100.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#add column for per student budget & overall passing rate
school_sumry_df["Per student budget"] = school_sumry_df["budget"]/school_sumry_df["Total students"]
school_sumry_df["% Overall Passing Rate"] = (school_sumry_df["% passing math"] + school_sumry_df["% passing reading"])/2
school_sumry_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>Total students</th>
      <th>budget</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>total math pass</th>
      <th>total reading pass</th>
      <th>% passing math</th>
      <th>% passing reading</th>
      <th>Per student budget</th>
      <th>% Overall Passing Rate</th>
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
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>2533</td>
      <td>2917</td>
      <td>86.835790</td>
      <td>100.0</td>
      <td>655.0</td>
      <td>93.417895</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>2549</td>
      <td>2949</td>
      <td>86.436080</td>
      <td>100.0</td>
      <td>639.0</td>
      <td>93.218040</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>1761</td>
      <td>1761</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>600.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>4007</td>
      <td>4635</td>
      <td>86.450917</td>
      <td>100.0</td>
      <td>652.0</td>
      <td>93.225458</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1468</td>
      <td>1468</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>625.0</td>
      <td>100.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#some more renaming
school_sumry_df = school_sumry_df.rename(columns = {"type":"School type"})
school_sumry_df = school_sumry_df.rename(columns = {"budget":"total budget"})
school_sumry_df = school_sumry_df.rename(columns = {"math_score":"Avg math score"})
school_sumry_df = school_sumry_df.rename(columns = {"reading_score":"Avg reading score"})
school_sumry_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School type</th>
      <th>Total students</th>
      <th>total budget</th>
      <th>Avg reading score</th>
      <th>Avg math score</th>
      <th>total math pass</th>
      <th>total reading pass</th>
      <th>% passing math</th>
      <th>% passing reading</th>
      <th>Per student budget</th>
      <th>% Overall Passing Rate</th>
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
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>2533</td>
      <td>2917</td>
      <td>86.835790</td>
      <td>100.0</td>
      <td>655.0</td>
      <td>93.417895</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>2549</td>
      <td>2949</td>
      <td>86.436080</td>
      <td>100.0</td>
      <td>639.0</td>
      <td>93.218040</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>1761</td>
      <td>1761</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>600.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>4007</td>
      <td>4635</td>
      <td>86.450917</td>
      <td>100.0</td>
      <td>652.0</td>
      <td>93.225458</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1468</td>
      <td>1468</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>625.0</td>
      <td>100.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#sort school_sumry_df in descending order to get the schools with 5 highest passing rate
school_highest_df = school_sumry_df.sort_values("% Overall Passing Rate",ascending=False)
school_highest_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School type</th>
      <th>Total students</th>
      <th>total budget</th>
      <th>Avg reading score</th>
      <th>Avg math score</th>
      <th>total math pass</th>
      <th>total reading pass</th>
      <th>% passing math</th>
      <th>% passing reading</th>
      <th>Per student budget</th>
      <th>% Overall Passing Rate</th>
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
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>1761</td>
      <td>1761</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>600.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1468</td>
      <td>1468</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>625.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>83.989488</td>
      <td>83.274201</td>
      <td>2283</td>
      <td>2283</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>578.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>1858</td>
      <td>1858</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>582.0</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>248087</td>
      <td>83.814988</td>
      <td>83.803279</td>
      <td>427</td>
      <td>427</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>581.0</td>
      <td>100.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#sort school_sumry_df in descending order to get the schools with 5 highest passing rate
school_lowest_df = school_sumry_df.sort_values("% Overall Passing Rate")
school_lowest_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School type</th>
      <th>Total students</th>
      <th>total budget</th>
      <th>Avg reading score</th>
      <th>Avg math score</th>
      <th>total math pass</th>
      <th>total reading pass</th>
      <th>% passing math</th>
      <th>% passing reading</th>
      <th>Per student budget</th>
      <th>% Overall Passing Rate</th>
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
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>2549</td>
      <td>2949</td>
      <td>86.436080</td>
      <td>100.0</td>
      <td>639.0</td>
      <td>93.218040</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>80.744686</td>
      <td>76.842711</td>
      <td>3457</td>
      <td>3999</td>
      <td>86.446612</td>
      <td>100.0</td>
      <td>637.0</td>
      <td>93.223306</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>4007</td>
      <td>4635</td>
      <td>86.450917</td>
      <td>100.0</td>
      <td>652.0</td>
      <td>93.225458</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>80.966394</td>
      <td>77.072464</td>
      <td>4128</td>
      <td>4761</td>
      <td>86.704474</td>
      <td>100.0</td>
      <td>650.0</td>
      <td>93.352237</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>2533</td>
      <td>2917</td>
      <td>86.835790</td>
      <td>100.0</td>
      <td>655.0</td>
      <td>93.417895</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Group student_df by school & grade to get avg math score per grade for each school
math_df = pd.DataFrame(student_df.groupby(["school","grade"])["math_score"].mean())
math_df.unstack()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="4" halign="left">math_score</th>
    </tr>
    <tr>
      <th>grade</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
      <th>9th</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
      <td>77.083676</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
      <td>83.094697</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
      <td>76.403037</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
      <td>77.361345</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
      <td>82.044010</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
      <td>77.438495</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
      <td>83.787402</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
      <td>77.027251</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
      <td>77.187857</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
      <td>83.625455</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
      <td>76.859966</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
      <td>83.420755</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
      <td>83.590022</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
      <td>83.085578</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
      <td>83.264706</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Group student_df by school & grade to get avg reading score per grade for each school
reading_df = pd.DataFrame(student_df.groupby(["school","grade"])["reading_score"].mean())
reading_df.unstack()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="4" halign="left">reading_score</th>
    </tr>
    <tr>
      <th>grade</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
      <th>9th</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
      <td>81.303155</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
      <td>83.676136</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
      <td>81.198598</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
      <td>80.632653</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
      <td>83.369193</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
      <td>80.866860</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
      <td>83.677165</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
      <td>81.290284</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
      <td>81.260714</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
      <td>83.807273</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
      <td>80.993127</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
      <td>84.122642</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
      <td>83.728850</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
      <td>83.939778</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
      <td>83.833333</td>
    </tr>
  </tbody>
</table>
</div>




```python
#define bins & label. Use pd.cut to bin on school_sumry_df dataframe
bins = [0,585,615,645,675]
bin_labels = ["<585","585-615","615-645","645-675"]
school_sumry_df["spending ranges (per student)"] = pd.cut(school_sumry_df["Per student budget"], bins, right = False, labels = bin_labels)
school_sumry_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School type</th>
      <th>Total students</th>
      <th>total budget</th>
      <th>Avg reading score</th>
      <th>Avg math score</th>
      <th>total math pass</th>
      <th>total reading pass</th>
      <th>% passing math</th>
      <th>% passing reading</th>
      <th>Per student budget</th>
      <th>% Overall Passing Rate</th>
      <th>spending ranges (per student)</th>
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
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>2533</td>
      <td>2917</td>
      <td>86.835790</td>
      <td>100.0</td>
      <td>655.0</td>
      <td>93.417895</td>
      <td>645-675</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>2549</td>
      <td>2949</td>
      <td>86.436080</td>
      <td>100.0</td>
      <td>639.0</td>
      <td>93.218040</td>
      <td>615-645</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>1761</td>
      <td>1761</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>600.0</td>
      <td>100.000000</td>
      <td>585-615</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>4007</td>
      <td>4635</td>
      <td>86.450917</td>
      <td>100.0</td>
      <td>652.0</td>
      <td>93.225458</td>
      <td>645-675</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1468</td>
      <td>1468</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>625.0</td>
      <td>100.000000</td>
      <td>615-645</td>
    </tr>
  </tbody>
</table>
</div>




```python
#groupby spending ranges to average math,reading scores
school_bin_df = pd.DataFrame(school_sumry_df.groupby(["spending ranges (per student)"])\
                             ["Avg reading score","Avg math score","% passing math","% passing reading","% Overall Passing Rate"].mean())
school_bin_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Avg reading score</th>
      <th>Avg math score</th>
      <th>% passing math</th>
      <th>% passing reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>spending ranges (per student)</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;585</th>
      <td>83.933814</td>
      <td>83.455399</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>585-615</th>
      <td>83.885211</td>
      <td>83.599686</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>615-645</th>
      <td>81.891436</td>
      <td>79.079225</td>
      <td>91.257336</td>
      <td>100.0</td>
      <td>95.628668</td>
    </tr>
    <tr>
      <th>645-675</th>
      <td>81.027843</td>
      <td>76.997210</td>
      <td>86.663727</td>
      <td>100.0</td>
      <td>93.331863</td>
    </tr>
  </tbody>
</table>
</div>




```python
#define bins & label for school size based on total students. Use pd.cut to bin on school_sumry_df dataframe
bins = [0,1000,2000,5000]
bin_labels = ["small","medium","large"]
school_sumry_df["School size"] = pd.cut(school_sumry_df["Total students"], bins, right = False, labels = bin_labels)
school_sumry_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School type</th>
      <th>Total students</th>
      <th>total budget</th>
      <th>Avg reading score</th>
      <th>Avg math score</th>
      <th>total math pass</th>
      <th>total reading pass</th>
      <th>% passing math</th>
      <th>% passing reading</th>
      <th>Per student budget</th>
      <th>% Overall Passing Rate</th>
      <th>spending ranges (per student)</th>
      <th>School size</th>
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
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>2533</td>
      <td>2917</td>
      <td>86.835790</td>
      <td>100.0</td>
      <td>655.0</td>
      <td>93.417895</td>
      <td>645-675</td>
      <td>large</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>2549</td>
      <td>2949</td>
      <td>86.436080</td>
      <td>100.0</td>
      <td>639.0</td>
      <td>93.218040</td>
      <td>615-645</td>
      <td>large</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>1761</td>
      <td>1761</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>600.0</td>
      <td>100.000000</td>
      <td>585-615</td>
      <td>medium</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>4007</td>
      <td>4635</td>
      <td>86.450917</td>
      <td>100.0</td>
      <td>652.0</td>
      <td>93.225458</td>
      <td>645-675</td>
      <td>large</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1468</td>
      <td>1468</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>625.0</td>
      <td>100.000000</td>
      <td>615-645</td>
      <td>medium</td>
    </tr>
  </tbody>
</table>
</div>




```python
#groupby school size to average math,reading scores
school_size_df = pd.DataFrame(school_sumry_df.groupby(["School size"])\
                             ["Avg reading score","Avg math score","% passing math","% passing reading","% Overall Passing Rate"].mean())
school_size_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Avg reading score</th>
      <th>Avg math score</th>
      <th>% passing math</th>
      <th>% passing reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School size</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>small</th>
      <td>83.929843</td>
      <td>83.821598</td>
      <td>100.0000</td>
      <td>100.0</td>
      <td>100.00000</td>
    </tr>
    <tr>
      <th>medium</th>
      <td>83.864438</td>
      <td>83.374684</td>
      <td>100.0000</td>
      <td>100.0</td>
      <td>100.00000</td>
    </tr>
    <tr>
      <th>large</th>
      <td>81.344493</td>
      <td>77.746417</td>
      <td>88.4419</td>
      <td>100.0</td>
      <td>94.22095</td>
    </tr>
  </tbody>
</table>
</div>




```python
#groupby school type to average math,reading scores
school_type_df = pd.DataFrame(school_sumry_df.groupby(["School type"])\
                             ["Avg reading score","Avg math score","% passing math","% passing reading","% Overall Passing Rate"].mean())
school_type_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Avg reading score</th>
      <th>Avg math score</th>
      <th>% passing math</th>
      <th>% passing reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>83.896421</td>
      <td>83.473852</td>
      <td>100.000000</td>
      <td>100.0</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>District</th>
      <td>80.966636</td>
      <td>76.956733</td>
      <td>86.790742</td>
      <td>100.0</td>
      <td>93.395371</td>
    </tr>
  </tbody>
</table>
</div>


