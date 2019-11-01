What am I looking at? 
===================

Questions for new data projects:

# Data structure question

- Do all records in the dataset contain the same fields?

- How can you access the same fields across records? By position? By name?

- Are the data entered by people? If so, there might be a high incidence of misspellings and nonstandard abbreviations.

- For date times, are time zones included or adjusted into a standard time zone like UTC? If the times are presented in 12-hour format, is AM/PM demarcated? Are the positions of month and day fields ambiguous (e.g., 01/02/17 versus 02/01/17)? Are there signs that the information is wrong (e.g., across your dataset, do you have an unusual amount of people with date-of-births corresponding to the first year of a decade like 1970, 1980, 1990, or born in January or on the first date of the month)?

- Were all the records and record fields collected/measured at the same time? If not, is the temporal range significant?

- How are the records delimited/separated in the dataset? Do you need sophisticated parsing logic to separate the records from one another?

- Have some records or record field values been modified after the time of creation? Are the timestamps of these modifications available?

- Can you determine if the data is “stale”? 

- Is it sufficient to sample the records and manually verify the data? Can you automatically verify it by using third-party services

- How are the record fields delimited from one another? Do you need to parse them?

- How are record fields encoded? Human readable strings? Binary numbers? Hash keys? Compressed? Enumerated codes?

- What is the complexity of the encoding? Primitive elements like integers, decimal numbers, short strings, and so on? Higher-order elements like key-value sets or arrays?

- What are the semantics of the encoded data? Do these semantics entail associated data quality, consistency, and accuracy checks?

- What are the relationship types between records and the record fields? 
 
  - Singular (record should have one and only one value for a field, like customer date of birth)?  

  - Set-based (record could have many values for the field, like customer shipping addresses)?

# BASIC QUESTIONS TO ASSESS DATA GRANULARITY


- What kind of thing (person, object, relationship, event, etc.) do the records represent?

- Are the records homogeneous (represent the same kinds of things)? Or heterogeneous?

- What alternative interpretations of the records are there? For example, if the records appear to be customers, could they actually be all known contacts (only some of which are customers)?

