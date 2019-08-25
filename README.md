This is my first data science project to practice data processing and analysis techniques using python. I use the Ipython IDE. 

### Data source:

I downloaded the gould sample from the National Bureau of Economic Research (NBER) website: https://www.nber.org/gould/.
Briefly, the gould sample is a historical sample of Union Army soldiers in the U.S. 
Here is the variable list:
https://www.nber.org/gould/variable_list.lst
The dataset is tab delimited so I transformed it to real CSV format in excel for further processing.

### Overview:

The raw dataset is pretty messy in the following aspects:
1. A lot of variables are highly missing: about 70% of data is missing in the entire dataset. The dataset has 229 variables with 20,406 rows, and only 90 columns have less than 80% of information missing. 
2. Duplicate columns: Same information was asked slightly differently; I suspect that this is because different forms are used for different people. For example: 
   MARREE and MARSTAT both contain information about marital status, but have different amounts of information
   All following columns are about grandparent birth places: BRTH_GP1, BRTH_GP2, GRNDBEE, GRNDFBEE, GRNDMBEE, USGP1, USGP2, USGRAND 
3. A lot of entry errors and non-standardized entry. 
   Wrong information. For example: answer to marital status: Brown.
   Same information entered differently. For example:  Military rank: PRIVATE/PR/P/P./PRIVTE/PRT, SERGEANT/SARGEANT/SERGT/SGT/SARGT/1 SERGT/2 SERGT…)
4. Survey organization issues. This needs to be addressed in data processing.
   No unique ID assigned for each entry
   After loading the dataset into python, the type of data for each column is not organized by the nature of data, for example, age is a non-null object rather than a float type
   Some questions can be asked more succinctly, rather than spreading the answers to the same question into separate questions. For example: month day and year of immigration can be organized as a single date variable.
