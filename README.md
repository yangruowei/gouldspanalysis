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
* Upon examining the names of the soldiers, there were several cases of people with the same name. For example, 35 observations had only the name "Smith" (most frequently occurring). There is probably a need to deduplicate the observations with similar information that could be indicating the same soldier, since there were no unique personal IDs. For example, could J. SMITH and JOHN SMITH and J SMITH be the same person? I used the pandas dedupe package to achieve this. During the labelling process, there didn't seem to be records that are duplicates of each other, and the dedupe process could not be completed. I went ahead and created a unique ID for all records.

### Data visualization
#### Univariate analysis
Countplots for categorical variables:
Description
Histograms for numerical variables:
Description
#### Bivariate analysis
Bivariate analysis:
boxplots, scatterplots?
#### Geogrophical visualization
Make use of place/location variables: St_ctry_birth, Father_birthplace, Enlist_location, Regi_state, Examplac. -> create geographical maps.

### Modelling
Classification/regression? demographics, anthropometrics to predict health condition



Doesn't seem to convert to categorical variables once saved to dataset


