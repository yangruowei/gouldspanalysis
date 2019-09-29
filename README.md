This is my first data science project to practice data processing and analysis techniques using python. I use the Ipython IDE. 

### Data source

I downloaded the gould sample from the National Bureau of Economic Research (NBER) website: https://www.nber.org/gould/.
Briefly, the gould sample is a historical sample of Union Army soldiers in the U.S. \
Here is the variable list: \
https://www.nber.org/gould/variable_list.lst \
The dataset is tab delimited so I transformed it to real CSV format in excel for further processing.

### Overview

The raw dataset is pretty messy in the following aspects: 
1. A lot of variables are highly missing: about 70% of data is missing in the entire dataset. The dataset has 229 variables with 20,406 rows, and only 90 columns have less than 80% of information missing. 
2. Duplicate columns: Same information was asked slightly differently; I suspect that this is because different forms were used for different people. For example: 
   MARREE and MARSTAT both contain information about marital status, but have different amounts of information \
   All following columns are about grandparent birth places: BRTH_GP1, BRTH_GP2, GRNDBEE, GRNDFBEE, GRNDMBEE, USGP1, USGP2, USGRAND 
3. A lot of entry errors and non-standardized entry. 
   * Wrong information. For example: answer to marital status: Brown. 
   * Same information entered differently. For example:  Military rank: PRIVATE/PR/P/P./PRIVTE/PRT, SERGEANT/SARGEANT/SERGT/SGT/SARGT/1 SERGT/2 SERGT…). In the "name" column, there are also a lot of rows with exact the same name (since a lot of names are incomplete, sometimes with only last name, sometimes fully spelled first and last names, other times initial first, with or without middle initial and/or period, and full last, sometimes just a single letter). For example: "Smith" appeared 35 times in the dataset.
4. Survey organization issues. This needs to be addressed in data processing. 
   * No unique ID assigned for each entry 
   * After loading the dataset into python, the type of data for each column is not organized by the nature of data, for example, age is a non-null object rather than a float type 
   * Some questions can be asked more succinctly, rather than spreading the answers to the same question into separate questions. For example: month day and year of immigration can be organized as a single date variable.

### Steps for data processing
#### selecting and organizing columns
I checked all 229 variables in the variables list and selected 33 variables with reasonably complete information. \
I then re-named all columns for better readability, standardized the column names (all capitalized) and re-orderd the columns that could be roughly grouped into the following groups: demographics, military information, anthropometrics, vigor, appearance, health condition. 
#### data cleaning for specific variables
* A lot of the string variables are quite messy, with no standardized format for answers. I examined these one by one and standardized or re-categorized the possible answers. 
* First, redesignate all seemingly unreasonable or missing answers (such as "NOT GIVEN" in the "Name" column and "BROWN" in the "Age" column) to None or NANs. 
* Upon examining the names of the soldiers, there were several cases of people with the same name. For example, 35 observations had only the name "Smith" (most frequently occurring). There is probably a need to deduplicate the observations with similar information that could be indicating the same soldier, since there were no unique personal IDs. For example, could J. SMITH and JOHN SMITH and J SMITH be the same person? I used the pandas dedupe package to achieve this.


* Note: count Age mean: 
In [11]: gould['Age'].mean()                                                    
Out[11]: 25.463815608305335
American:
In [12]: gould['American'].value_counts()                                       
Out[12]: 
Y    3437
N    2284
In [23]: gould['Marital_status'].value_counts()                                 
Out[23]: 
S    11228
M     4803
W      240
Name: Marital_status, dtype: int64

In [24]: gould['Rank'].value_counts()                                           
Out[24]: 
PRIVATE             14497
CORPORAL             1282
SERGEANT             1241
SEAMAN                650
LANDSMAN              305
MUSICAN/BAND          249
HANDYMAN/SUPPORT      176
FIREMAN               159
LIEUTENANT            112
CAPTAIN                73
OTHER                  48
DOCTOR/HOSPITAL        34
OTHER_RANKS            17
CLERGY                 14
Name: Rank, dtype: int64

In [28]: gould['Enlist_term'].value_counts()                                    
Out[28]: 
3 TO 4 YEARS             13253
1 TO 2 YEARS              2428
5 OR MORE YEARS            561
WAR/NO SPECIFIED TERM      501
2 TO 3 YEARS               482
4 TO 5 YEARS               146
LESS THAN 1 YEAR            63
OTHER UNSPECIFIED            8
Name: Enlist_term, dtype: int64

In [35]: gould['Examiner'].value_counts()                                       
Out[35]: 
S B BUCKLEY        6411
C D LEWIS          2416
ARTHUR PHINNEY     1762
WM S BAKER         1570
JOHN ELNSER        1019
E B FAIRCHILD       994
FRANK H SMITH       990
F PRALE             678
JAMES RUSSELL       629
H T MYERS           325
SIGOURNEY WALES     309
G AVERY             296
THOMAS FURNIFS      240
J M STANK           164
BRENT G WILDER       29
Name: Examiner, dtype: int64

In [61]: gould['Height'].mean()                                                 
Out[61]: 67.02499922445028

In [63]: gould['Weight'].mean()                                                 
Out[63]: 145.27235756589593

In [67]: gould['Waist_circumference'].mean()                                    
Out[67]: 31.533619034609554

In [70]: gould['Hip_circumference'].mean()                                      
Out[70]: 36.40772857879614

Shoulder width outlier: 115.796875       1
In [73]: gould['Shoulder_width'].mean()                                         
Out[73]: 14.548599723044939

In [75]: gould['Arm_length'].mean()                                             
Out[75]: 29.36053375051804
OUtlier: 0.199982        1

In [79]: gould['Height_to_neck'].mean()                                         
Out[79]: 57.12283036737298
OUtlier:
0.299988        2
5.000000        1

In [81]: gould['Height_to_perinaeum'].mean()                                    
Out[81]: 31.298136525607465
Outlier: 2.500000        1
3.799805        1
3.500000        1

In [84]: gould['Neck_breadth'].mean()                                           
Out[84]: 4.198265939545396
Outlier: 41.000000       1

In [87]: gould['Pelvis_breadth'].mean()                                         
Out[87]: 12.205474131425747

In [90]: gould['Chest_capacity'].mean()                                         
Out[90]: 178.2178788549421

In [96]: gould['Dynamome'].mean()                                               
Out[96]: 320.8356509509184

In [131]: gould['Eye_clr'].value_counts()                                       
Out[131]: 
BLUE      7303
BROWN     4093
GRAY      2489
BLACK     2090
HAZEL     1950
DARK        60
GREEN        5
YELLOW       3
OTHER        3

In [28]: gould['Haircolr'].value_counts()                                       
Out[28]: 
BROWN    11943
BLACK     3455
RED        366
OTHER      337
GRAY       146
Name: Haircolr, dtype: int64

In [62]: gould['Complexion'].value_counts()                                     
Out[62]: 
FAIR      6797
DARK      3511
RED       1766
BLACK     1116
LIGHT      931
MEDIUM     437
ROBUST     430
BROWN      219
YELLOW     118
OTHER       24
Name: Complexion, dtype: int64

In [82]: gould['Muscle_dev'].value_counts()                                     
Out[82]: 
GOOD         11685
MODERATE      3142
LARGE          976
FULL           272
SMALL           88
DEFICIENT       26
Name: Muscle_dev, dtype: int64

In [86]: gould['St_vigor'].value_counts()                                       
Out[86]: 
Y    14909
N     2761
Name: St_vigor, dtype: int64

In [92]: gould['Athletic_recreation'].value_counts()                            
Out[92]: 
N    6100
Y    4867
Name: Athletic_recreation, dtype: int64

In [95]: gould['Eyes_prom'].value_counts()                                      
Out[95]: 
N    13596
Y     2083
Name: Eyes_prom, dtype: int64

In [105]: gould['Teeth_con'].value_counts()                                     
Out[105]: 
GOOD    12382
POOR     2715
FAIR      458


Doesn't seem to convert to categorical variables once saved to dataset

How to process place/location variables? 
St_ctry_birth, Father_birthplace, Enlist_location, Regi_state, Examplac.
