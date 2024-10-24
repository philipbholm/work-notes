# Formulas
This document outlines features that have been requested by customers for calculated variables / aggregation rules. 

## Calculated variables
### Requirements
- Create a text template based on category variables
	- Useful to reduce writing when adding to patient journal
	- IF category1 == "A" THEN
		- Start output text with a specified sentence
	- IF category2 == "Some value"
		- Append text to the values from above
- Math formulas, such as ln, exp, mean, sum
	- It should be possible to remove missing values
- Mapping from category variable to score
	- IF value == "Mild" THEN 1
	- IF value == "Severe" THEN 2
- Mapping from numerical range to score
	- IF age > 50 THEN 0
	- IF 50 <= age < 60 THEN 1
- Handling of missing values (both numeric and categorical)
	- IF value is None THEN 0
- Conditional output based on category variable
	- IF sex == "Male" THEN ...
	- IF sex == "Female" THEN ...
- Calculated variables as input to another calculated variable
	- Compute `BMI`, then create a new variable `Overweight` based on `BMI`
- AND / OR statement
	- IF sex = "Female" AND age > 50 THEN ...
	- IF sex = "Male" OR age < 50 THEN ...
- Combine date calculations with numeric variables 
	- (n_sigarettes/20) * (end_date_smoking - start_date_smoking)
	- If end_date_smoking is missing, use a specific date or another variable (visit date)
- Support for multiple category types
	- IF value IN ["A", "B"] THEN ...
	- IF value == ["A", "B"] THEN ...
- Support for duration variable type
	- `length_of_stay` + 30 days
	- `end_date` - `start_date` as a duration type in specified time unit
- Rolling percentage changes
- Filter on calculated variables
	- It should be possible to the user to filter on calculated variables
- Use of calculated variables in aggregation rules
	- Compute a score for each entry in a series, then compute the average score

### Example indexes/scores
- [Farmingham Risk Score](https://www.mdcalc.com/calc/38/framingham-risk-score-hard-coronary-heart-disease)
- [APACHE II Score](https://www.mdcalc.com/calc/1868/apache-ii-score#creator-insights)
- [CPQoL Score](https://www.ausacpdm.org.au/research/cpqol/)
- [ESSDAI](https://rheumcalc.com/essdai/)
- [WOMAC Osteoarthritis Index](https://www.physio-pedia.com/WOMAC_Osteoarthritis_Index)
- [Well's Criteria for DVT](https://www.mdcalc.com/calc/362/wells-criteria-dvt)
- [Charlson Comorbidity Index](https://www.mdcalc.com/calc/3917/charlson-comorbidity-index-cci)
- [EASI score](https://www.mdcalc.com/calc/10427/eczema-area-severity-index-easi)


## Aggregation rules
### Requirements
- Search/filter/sort on aggregated values
- Using aggregated values in analyses
- Calculated variables as input to aggregated values
- Aggregated values as input to calculated variables
- Nested aggregated values
- Set a variable as a sorting variable
    - This is the most requested feature for aggregated values
    - Users typically use a date variable as the identifier
        - Unique contraints might be useful here
    - It is common to want the latest value in a series
        - Right now the latest entry is based on the created timestamp, which causes problems
- The entry count in a series has been highly requested

### Examples
- Remission-free on last control
    - Sort by `date`
    - Check the last value of `remission`
    - IF value = `Remission` THEN `Yes` ELSE `No`
- How many days before remission?
    - Sort by `date`
    - Check the last value of `remission`
    - IF value IS NOT NULL
        - THEN `control_date` - `surgery_date`
            - `surgery_date` might be from a series level up
        - ELSE NULL
- Is the patient healthy?
    - The patient has surgeries on the left and right ear
    - To be healthy, the no controls for the last surgery on each ear can have remission
    - Sort surgery series by `date`
        - Check left side
            - Filter `left` on `ear` variable
            - Find all entries in `controls` series connected to this surgery
            - If none of the entries has the value `remisssion`, then the left side is healthy
        - Check right side
    - IF both sides healthy THEN patient healthy
- Starting dose of medication
    - Sort by `date`
    - Filter on `medication` == `Rux`
    - IF any values left
        - THEN return the first value of `dosage`
        - ELSE return NULL
- Duration of therapy
    - Sort by `start_date`
    - Compute the difference between the last value of `end_date` and the first value of `start_date`
    - If the last value of `end_date` is NULL, then the patient is still on medication
        - Alternative 1: Use todays date (or specific date)
        - Alternative 2: The date 3 years after `diagnosis_date` (variable from main level)
- Is baseline hemoglobin less than 100?
    - Sort by `date`
    - Check first value of `hemoglobin`
    - IF `hemoglobin` < 100 THEN `Yes` ELSE `No`
- Average measurement across both sides
    - Compute the mean value across all entries for `left_side` and `right_side`
    - Right now it is only possible to compute the average for one variable
- Compute sensitivity and specificity
    - Given two category variables `predicted_value` and `true_value`
        - Each variable can have the values `Positive` and `Negative`
    - Create a crosstable of the categories
    - Compute sensitivity and specificity: https://thosenerdygirls.org/sensitivity-and-specificity/
    - IMPORTANT: The ordering of the rows/colums in the crosstable will affect the results
